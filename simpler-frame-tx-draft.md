---
eip: TBD
title: Simpler Frame Transaction
description: Generalized transaction type with minimal protocol surface; supersedes EIP-8141 v1 + EIP-8250 + EIP-8266
status: Draft (working document — not yet submitted)
type: Standards Track
category: Core
created: 2026-05-22
requires: 1559, 2718, 4844, 7702
discussions-to: TBD
---

## Abstract

A new transaction type (`FRAME_TX_TYPE = 0x06`) that decomposes a transaction into a sequence of *frames* (contract calls with mode-defined caller and side-effect rules), validated and executed under two protocol primitives:

1. The `APPROVE` instruction sets transaction-scoped flags (`payer`, `sender_approved`) as approval effects.
2. A frame in `VERIFY` mode may `SSTORE` to its resolved target's own storage; those writes are also approval effects.

Approval effects are journaled outside the revert journal of subsequent frames and outside any atomic-batch rollback. Everything else — replay protection schemes, expiry, paymasters, batching, signature schemes, session keys, multi-dimensional nonces, nullifier-based privacy — composes from these two primitives plus user-deployed contract code.

The design supersedes EIP-8141 v1 (current `master`), EIP-8250 (keyed nonces via NONCE_MANAGER system contract), and the EIP-8266 expiring-nonce mechanism, by eliminating the system contracts those proposals introduce and folding their use cases into contract-managed state.

## Motivation

EIP-8141 v1 introduced frame transactions but accumulated a growing protocol surface to accommodate downstream use cases: keyed nonces (EIP-8250's `NONCE_MANAGER`), expiring nonces (EIP-8266's `NONCE_RING`), flexible nonces + signer binding + guarantors (PR #11681's `AuthManager`), guarantors alone (PR #11555). Each adds a new system contract, new envelope fields, new gas constants, and new protocol-managed approval-effect rules.

The core observation: all these additions implement *one* underlying primitive — "the verifier's own storage may be written during validation, and that write survives later-frame reverts." With that primitive exposed directly, every downstream use case becomes a contract pattern, priced naturally by `SSTORE` economics, with no new system contracts.

This EIP exposes that primitive and removes the system-contract apparatus. The execution layer's surface for account abstraction reduces to two protocol primitives plus a small number of frame-execution rules. New features (post-quantum signature schemes, session keys, subscription auth, novel replay schemes) become contract patterns plus optional mempool template recognition, requiring no further consensus changes.

## Specification

### Constants

| Name                       | Value             |
|----------------------------|-------------------|
| `FRAME_TX_TYPE`            | `0x06`            |
| `FRAME_TX_INTRINSIC_COST`  | `15000`           |
| `FRAME_TX_PER_FRAME_COST`  | `475`             |
| `ENTRY_POINT`              | `address(0xaa)`   |
| `MAX_FRAMES`               | `64` (TODO: revisit; may derive from gas limit) |

No system contracts are defined by this EIP. There is no `NONCE_MANAGER`, no `AuthManager`, no `NONCE_RING`, no `EXPIRY_VERIFIER` at a fixed address.

### Envelope

The transaction envelope contains only what the protocol must read before EVM execution begins (block-level fee accounting, block-construction commitments, sender identity, frame list, signature list):

```text
[chain_id, sender, frames, signatures,
 max_priority_fee_per_gas, max_fee_per_gas,
 max_fee_per_blob_gas, blob_versioned_hashes,
 <REPLAY_PROTECTION_FIELD>]
```

Where `<REPLAY_PROTECTION_FIELD>` is **TODO** — see [Replay Protection](#replay-protection) below.

Notably absent:
- No outer `nonce` field. Replay protection is handled per [Replay Protection](#replay-protection).
- No `signer` envelope field (as in PR #11681). Multi-signer authentication is expressed via the `signatures` list.
- No `expiry` envelope field. Expiry is a contract pattern (see [Expiry](#expiry-as-a-contract-pattern)).

### Frame

A frame is `[mode, flags, target, gas_limit, value, data]`.

#### Modes

| `mode` | Name      | Caller        | Storage-write rule                          |
|--------|-----------|---------------|---------------------------------------------|
| 0      | `DEFAULT` | `ENTRY_POINT` | Full EVM access                             |
| 1      | `VERIFY`  | `ENTRY_POINT` | `STATICCALL`-like, except: `APPROVE`, and `SSTORE` to `resolved_target`'s own storage |
| 2      | `SENDER`  | `tx.sender`   | Full EVM access; requires `sender_approved == true` from a prior `VERIFY` frame |

`resolved_target` is `frame.target` if non-null, otherwise `tx.sender`.

#### Flags

| Bits | Meaning             |
|------|---------------------|
| 0–1  | Approval scope (used by `APPROVE` to determine `APPROVE_NONE`/`APPROVE_PAYMENT`/`APPROVE_EXECUTION`/`APPROVE_EXECUTION_AND_PAYMENT`) |
| 2    | Atomic batch (this frame joins a contiguous atomic group with the next frame) |
| 3+   | Reserved          |

TODO: consider collapsing the 2-bit approval scope into a generalized "approval flag set" pattern that future EIPs can extend without re-encoding existing bits.

### Approval Effects

Two kinds of state change occurring inside a successfully-exiting frame are **approval effects**:

1. The `APPROVE` instruction's updates to `payer`, `sender_approved`, and any future tx-scoped approval flags.
2. `SSTORE` operations performed inside a `VERIFY` frame that write to `resolved_target`'s own storage (the contract's own slots, whether written directly or via `DELEGATECALL` from `resolved_target`).

Approval effects are journaled **outside** the revert journal of subsequent frames and outside any later atomic-batch snapshot. Concretely:

- If a frame reverts, its approval effects are discarded along with the rest of its revert journal.
- If a frame exits successfully (via `RETURN`, `STOP`, or `APPROVE`), its approval effects are committed and **MUST NOT** be reverted by later frame reverts, atomic-batch rollback, or any other in-transaction mechanism.

`SSTORE`s outside `resolved_target`'s own storage are NOT permitted in `VERIFY` mode; the frame reverts on such an attempt. `SSTORE`s in `VERIFY` mode are priced under ordinary [EIP-2200](./eip-2200.md) rules. No surcharge for first-use slots beyond what `SSTORE` already charges.

### Replay Protection

**TODO — this is the unresolved part of the design.**

The replay-protection mechanism must satisfy four constraints simultaneously:

1. **Universal coverage.** Every frame transaction must be non-replayable, including those with custom verifier code submitted via private channels. The "tx hash appears at most once in the canonical chain" invariant must hold.
2. **Verifier independence.** The mechanism must not depend on the verifier contract's correctness. A buggy or malicious verifier must not be able to enable replay.
3. **Unlinkability.** The mechanism must not introduce observable links between transactions from the same sender or between frames within one transaction beyond what is already public.
4. **Bounded state cost.** Per-transaction state growth must be acceptable. The naive "store every tx_id forever in a global registry" approach (~830 MB/year at current throughput) is rejected.

Candidate mechanisms considered and rejected so far:

- **Outer envelope nonce (legacy + EIP-8141 v1).** Satisfies (1) and (2) but blocks concurrency for shared-sender patterns (privacy pools), violating the implicit fifth constraint that motivated EIP-8250.
- **Keyed nonces in a `NONCE_MANAGER` system contract (EIP-8250).** Satisfies (1)–(4) for the keyed case but introduces a system contract, envelope schema migration, and a separate approval-effects journaling rule.
- **Verifier-managed replay state in own storage (`simpler-frame-txs` initial proposal).** Satisfies (1) only up to verifier correctness — fails constraint (2).
- **Universal protocol-managed `tx_id` registry.** Satisfies (1)–(3) but fails (4) due to unbounded state growth.
- **Ring-buffer with bounded retention (EIP-8266-style, generalized to all txs).** Satisfies (4) by bounding state, satisfies (1)–(3) within the retention window, but allows replay after the retention window — which is acceptable only if signers commit to deadlines consistent with the window.

The design space remaining to explore:

- Per-account ring buffer (state cost scales with active accounts, not total txs)
- Cryptographic accumulator (constant-size state, expensive proofs)
- Hybrid: verifier-managed by default + protocol-level structural check that catches naive verifiers
- Time-bounded retention with explicit per-tx deadline (forces signers to pick deadlines; pruning is automatic)

Until this is resolved, the rest of this EIP assumes that *some* mechanism exists that satisfies the four constraints. The other primitives in this spec are independent of which mechanism is chosen.

### Signatures

The envelope `signatures` field is a list of `[scheme, signer, msg, signature]` objects, modeled on PR #11481 (`Update EIP-8141: add signatures list to outer tx` by lightclient).

Every signature in the list **MUST** validate successfully before any frame executes. If any signature is malformed or invalid, the whole transaction is invalid.

#### Schemes

| `scheme` | Name        | Signature encoding                          | Gas cost |
|----------|-------------|---------------------------------------------|----------|
| `0x0`    | `SECP256K1` | `v(1) ‖ r(32) ‖ s(32)`                      | 2800     |
| `0x1`    | `P256`      | `r(32) ‖ s(32) ‖ qx(32) ‖ qy(32)`           | 6700     |
| `0x2`..  | reserved    | reserved                                    | reserved |

For `SECP256K1` and `P256`, `signer` is a 20-byte Ethereum address. For `P256`, the signer address must equal `keccak256(qx ‖ qy)[12:]`.

#### `msg` semantics

- If `len(msg) == 0`, the signature is over `compute_sig_hash(tx)`.
- If `len(msg) == 32`, the signature is over the explicit digest `msg`. The 32-byte zero digest is invalid (reserved as the EVM-visible representation of "no signature for this stack value").
- Any other `msg` length invalidates the transaction.

#### Signature hash

```python
def compute_sig_hash(tx) -> bytes32:
    # Elide raw signature bytes for signatures over the canonical hash
    # (a signature can't commit to its own bytes).
    for i, sig in enumerate(tx.signatures):
        if len(sig.msg) == 0:
            tx.signatures[i].signature = Bytes()
    return keccak256(bytes([FRAME_TX_TYPE]) + rlp(tx))
```

Note that this rule is uniform: only raw signature bytes of self-referential signatures are elided. All frame data (including any future expiry-deadline data) is committed by the signature hash.

#### Raw signature bytes are not introspectable from EVM

Smart-account contracts inspect signatures via the `SIGPARAM` opcode (see [Introspection](#introspection)), which returns signer / scheme / msg / signature-length metadata. Raw signature bytes are intentionally not accessible from the EVM. This reserves a future hook for signature aggregation (replacing many individual signatures with one aggregated witness).

### Default Code

When a frame's `resolved_target` has empty code (no contract, no EIP-7702 delegation), the protocol executes a built-in default code behavior.

For `VERIFY` mode:

1. Read `allowed_scope = frame.flags & APPROVE_SCOPE_MASK`. If `allowed_scope == APPROVE_NONE`, revert.
2. If `allowed_scope & APPROVE_EXECUTION != 0` and `resolved_target != tx.sender`, revert.
3. Look up a signature `sig` in `tx.signatures` such that:
   - `sig.scheme == SECP256K1`
   - `sig.signer == resolved_target`
   - `sig.msg == keccak256(compute_sig_hash(tx) ‖ uint64_be(state[tx.sender].nonce))` (augmented digest)
4. If no such signature exists, revert.
5. Increment `state[tx.sender].nonce` as an approval effect (preserves legacy CREATE-address semantics).
6. Call `APPROVE(allowed_scope)`.

For `SENDER` or `DEFAULT` mode: return successfully as if calling empty code.

The augmented-digest construction binds the sender's signature to the current pre-state account nonce, providing the equivalent of legacy nonce-based replay protection for EOAs. After inclusion, the account nonce advances, and the same signature no longer matches the augmented digest.

TODO: this is partially redundant with whatever universal replay-protection mechanism is settled in [Replay Protection](#replay-protection). If the universal mechanism subsumes per-tx uniqueness, the default-code augmented-digest construction may simplify to a signature over `compute_sig_hash(tx)` directly.

### `APPROVE` Instruction (`0xaa`)

Exits the current frame successfully and updates transaction-scoped approval flags based on the `scope` operand:

- `APPROVE_NONE` (`0x0`): no flags set.
- `APPROVE_PAYMENT` (`0x1`): sets `payer = resolved_target`.
- `APPROVE_EXECUTION` (`0x2`): sets `sender_approved = true` (only valid if `resolved_target == tx.sender`).
- `APPROVE_EXECUTION_AND_PAYMENT` (`0x3`): both.

These updates are approval effects.

### Introspection

Opcodes for reading structured transaction data from EVM:

- `TXPARAM(0xb0)` — read envelope fields (chain_id, sender, fees, sig hash, frame count, current frame index, signature count, etc.)
- `FRAMEDATALOAD(0xb1)`, `FRAMEDATACOPY(0xb2)` — read frame data
- `FRAMEPARAM(0xb3)` — read per-frame metadata (mode, flags, target, gas, value, length, status, etc.)
- `SIGPARAM(0xb4)` — read per-signature metadata (signer, scheme, msg, signature length); raw signature bytes are NOT accessible

TODO: consider collapsing these into a single parameterized read opcode (e.g., `TXLOAD(domain, index, param)`). The current granularity reflects historical accretion as features were added.

### Atomic Batching

A frame with bit 2 of `flags` set joins an atomic group with the immediately following frame. The group's boundary is the first frame in the contiguous sequence whose bit 2 is not set. If any frame in the group reverts, the protocol rolls back to the state immediately before the group started.

Approval effects committed *during* the group are NOT reverted by atomic-batch rollback (this is the point of journaling them separately).

### Receipt

`[cumulative_gas_used, payer, [per_frame_receipt, ...]]`, where each `per_frame_receipt` is `[status, gas_used, logs]`. Status code `0x3` indicates a frame skipped due to atomic-batch failure.

### Mempool

Public-mempool admission rules constrain which frame transactions propagate freely. Outside the public mempool, builders may include transactions via private channels.

#### Recognized verifier templates

The mempool propagates transactions only if the verifier frame's resolved code is recognized as a safe template. Three categories:

1. **Default code** (empty code). The protocol's built-in EOA path. Always recognized.
2. **Canonical templates** (recognized smart-account contracts, identified by runtime-code match). Includes the canonical paymaster, the canonical expiry verifier (see below), and other community-standardized templates.
3. **Guarantor-backed transactions** (see PR #11555). A transaction with a `pay` frame from a recognized guarantor is admitted regardless of sender verifier code.

The set of recognized canonical templates is a versioned policy external to this EIP. Adding a new template does not require modifying the execution-layer specification.

#### Storage dependency tracking

The mempool simulates the validation prefix and records every storage slot the verifier reads or writes. These slots become the transaction's revalidation dependencies. When a new block touches any of them, affected pending transactions are re-simulated.

This is the [ERC-7562](./eip-7562.md) [STO-010] model: own-storage access is always allowed during validation, and dependency tracking handles invalidation.

#### Concurrency

The mempool admits multiple concurrent pending transactions from the same sender if their dependency sets are disjoint. This enables shared-sender designs (privacy pools, session-key wallets) where many independent users transact through one address without serializing.

### Expiry (as a contract pattern)

A canonical `ExpiryVerifier` runtime code pattern is recognized by the mempool (not by a hardcoded address). Any contract whose runtime code matches the pattern is admitted as an expiry frame. The mempool drops transactions whose expiry deadline has passed. The `TIMESTAMP` opcode is permitted during validation only inside frames whose target code matches this pattern.

Canonical pattern (8-byte deadline calldata, `STOP` if `block.timestamp <= deadline`, else `REVERT`):

```
0x60083614600a575f5ffd5b5f3560c01c4211601657005b5f5ffd
```

No `EXPIRY_VERIFIER` constant. Anyone may deploy an instance. Recognition is by code match, identical to the canonical paymaster pattern.

### EIP-7702 Integration

EOAs with an EIP-7702 delegation indicator at their account code execute the delegated code when called as a frame target. This is unchanged from EIP-7702's existing semantics; this EIP does not add a new way to install delegations.

The combination yields full smart-account capability at the EOA's address:

- The delegated code runs as the verifier when the EOA is `resolved_target` in a `VERIFY` frame.
- The delegated code may `SSTORE` to the EOA's own storage as approval effects (multi-dim nonces, nullifier tables, session-key registries, etc.).
- The EOA's signature scheme is whatever the delegated code implements.

Installing a delegation indicator remains the responsibility of EIP-7702 (or a future EIP that extends the `0xef01XX` delegation namespace, e.g., EIP-8164 for post-quantum delegations).

## Two-Nonce Split

The legacy `state[address].nonce` field has been used for three purposes simultaneously: replay protection, `CREATE` address derivation, and application sequence tracking. These are now treated as distinct concerns:

- **Replay nonce** (TODO): handled by whatever mechanism is settled in [Replay Protection](#replay-protection). Does not modify `state[address].nonce` directly (though the default code does, as a transitional measure).
- **Account counter** (`state[address].nonce`): writable via the default code's approval-effect bump, by `CREATE`/`CREATE2`/`CREATE3` from the account, and (TODO) by an account-controlled opcode. Used for `CREATE`-address derivation and for any application that wants a per-account monotonic counter. **Not** auto-incremented on every frame transaction.
- **Application sequence counters**: contract-defined. Stored in the contract's own storage. Updated as approval effects when the contract chooses.

This separation eliminates the conflation that produced the "CREATE address might not be what you expect when using keyed nonces" warnings in EIP-8250 and the analogous issues in EIP-8266 and PR #11681.

## Rationale

### Minimal protocol surface

The execution layer's job is to commit transactions, sequence frame execution, and enforce a small number of journaling rules. Specifically:

1. The frame sequence is well-formed.
2. APPROVE and own-storage SSTORE in VERIFY are journaled as approval effects.
3. Mode-specific caller identity and side-effect rules are enforced.

Everything else (replay schemes, expiry, paymasters, batching, signature schemes, session keys, subscription patterns, intent-style transactions, privacy protocols) is implementable in pure EVM as user-deployed contract code. New features become new contract patterns, optionally recognized by the mempool as templates, with no consensus changes.

### Why this supersedes EIP-8250 and EIP-8266 individually

EIP-8250 adds keyed nonces via a `NONCE_MANAGER` system contract, an envelope schema migration, a 20 000-gas first-use surcharge, and a second approval-effects journaling rule layered on top of `APPROVE`. All of this exists because the current EIP-8141 v1 does not allow `SSTORE` during `VERIFY`. Once it does, the same use case is a 5-line smart-account verifier with a `mapping(uint256 => uint64)` of keyed nonces in its own storage. No system contract. No envelope migration. No new gas constant.

EIP-8266 adds expiring nonces via a sentinel envelope value and a `NONCE_RING` system contract. The same use case becomes a code-matched canonical ring-buffer verifier or a contract-managed nullifier set. No system contract.

PR #11681's `AuthManager` consolidates the above plus signer binding and guarantors into one system contract. The keyed-nonce dimension dissolves as above. Signer binding (modifying `ECRECOVER` to consult a `verified_signers` cache) and guarantors (PR #11555's payer primitive) are genuinely useful and are orthogonal to replay protection — they can be adopted independently, as separate EIPs, without the consolidated system contract.

### Verifier independence vs. mempool template recognition

The execution layer does not enforce verifier correctness. A buggy contract can fail replay protection and produce replayable transactions. The defense lives at the mempool layer: only transactions whose verifier matches a recognized template propagate through the public mempool. Builders accepting non-template verifiers via private channels accept the risk.

This is the same trust model that ERC-4337 has operated under since 2024. The protocol provides primitives; the mempool provides discipline.

A protocol-level structural check (e.g., "the verifier MUST `SSTORE` to its own storage during the validation prefix") can be layered on top to catch the naive-verifier class of bugs at consensus level, without requiring full simulation. See [Replay Protection](#replay-protection) for the open design question.

### Why blobs stay in the envelope

`max_fee_per_blob_gas` and `blob_versioned_hashes` cannot be moved into frame data. Block builders must read blob count and per-blob fee from the transaction header before any EVM execution. KZG verification happens at the consensus layer. Blob garbage collection coordinates with the consensus layer on a separate schedule. None of this is reachable from EVM bytecode.

The EVM-visible blob interface (`BLOBHASH` opcode) is preserved unchanged.

## Backwards Compatibility

`state[address].nonce` retains its legacy semantics for non-frame transactions (legacy, 1559, 4844, 7702). For frame transactions:

- EOAs via default code increment `state[tx.sender].nonce` as an approval effect (legacy-compatible).
- Smart accounts and 7702-delegated EOAs do not necessarily increment it, depending on their verifier code.

This preserves the sequence space for users transitioning between transaction types and preserves `CREATE`-address derivation semantics for accounts that perform contract creation.

The `ORIGIN` opcode returns the frame's caller (`ENTRY_POINT` or `tx.sender` depending on mode), consistent with EIP-7702's existing modification.

[EIP-3607](./eip-3607.md)'s restriction (rejecting transactions whose sender has deployed code) does NOT apply to frame transactions: `SENDER`-mode frames originate calls where `tx.sender` is a contract account.

## Security Considerations

TODO — to be expanded once [Replay Protection](#replay-protection) is settled. Topics to cover:

- Trust model for verifier templates (template registry governance, template auditing requirements)
- DoS resistance of mempool simulation (signature pre-validation gates much of the budget)
- Front-running risk of deploy frames (initcode must be safe to submit by any party)
- Cross-frame data visibility during validation (frames can introspect later frames via FRAMEPARAM etc.)
- State-growth implications of whatever replay mechanism is settled
- Approval-effects journaling and the persistence guarantee
- EIP-7702 delegation indicator interactions with frame execution

## Open Questions / TODO

1. **Replay protection mechanism.** The four constraints (universal coverage, verifier independence, unlinkability, bounded state cost) have no obvious simultaneous satisfier yet. Candidate directions:
   - Per-account ring buffer (state scales with active accounts rather than total txs)
   - Time-bounded retention with per-tx deadline (signers commit to a deadline; protocol prunes after)
   - Cryptographic accumulator over consumed tx_ids (constant state, expensive proofs)
   - Hybrid: verifier-managed by default + structural check at protocol level for naive-verifier mitigation
   - Something else
2. **Account-counter opcode.** Whether to expose a `BUMPNONCE` or `READACCOUNTCOUNTER` opcode for application use, vs. requiring CREATE to be the only protocol-driven bump path.
3. **Introspection opcode consolidation.** Whether to collapse `TXPARAM`/`FRAMEPARAM`/`FRAMEDATALOAD`/`FRAMEDATACOPY`/`SIGPARAM` into a single parameterized read.
4. **Approval scope generalization.** Whether the 2-bit `approval_scope` flags field should be a general "approval flag set" extensible by future EIPs (e.g., `APPROVE_GUARANTEE` from PR #11555).
5. **Signature aggregation specifics.** PR #11481 reserves the hook by hiding raw signature bytes from EVM. The concrete aggregation scheme (BLS rogue-key-secure aggregate? SNARK-aggregated PQ?) is deferred to a future EIP.
6. **Public key alias mechanism.** PR #11481 sketches `0xef02 || version || key_type || pubkey_len || pubkey` for state-backed large-PQ-key aliases. Whether to include in this EIP or defer to a separate one.
7. **`MAX_FRAMES`** and other arbitrary numeric limits. Probably derive from gas rather than hard-code.
8. **Validation-prefix patterns.** Enumeration of the recognized mempool shapes (`[self_verify]`, `[deploy, self_verify]`, `[only_verify, pay]`, `[deploy, only_verify, pay]`, possibly more for guarantors).
9. **Activation mechanics.** Fork timing, mempool migration strategy, interaction with EIP-7702 already shipped in Pectra.
10. **Default-code interaction with replay protection.** Once the universal replay mechanism is settled, the augmented-digest construction in the default code may simplify or change.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
