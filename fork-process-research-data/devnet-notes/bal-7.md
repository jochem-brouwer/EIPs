# bal-devnet-7 spec

:::info
:mega: bal-devnet-7 target launch: **Monday 18.05** (subject to ACDT today).
:::

:::info
:checkered_flag: **Last `bal-` prefixed devnet.** Consolidates the bal-devnet-3 optimisations with the EIP-8037 spec changes finalised since bal-devnet-6 — confirmed in [`tests-bal@v7.0.0`](https://github.com/ethereum/execution-specs/releases/tag/tests-bal@v7.0.0). Future devnet releases will be prefixed `glamsterdam-`.
:::

:::info
❗ **EIP-8037 final shape** — all spec changes for the EIP land here, pulled from four EIPs PRs: [#11573](https://github.com/ethereum/EIPs/pull/11573) (fixed CPSB + frame accounting, **merged**), [#11606](https://github.com/ethereum/EIPs/pull/11606) (SELFDESTRUCT charges, **merged**), [#11611](https://github.com/ethereum/EIPs/pull/11611) (bal-devnet-6 bug fixes, open), [#11616](https://github.com/ethereum/EIPs/pull/11616) (parameter recalibration, open). EELS reference branch: [`devnets/bal/7`](https://github.com/ethereum/execution-specs/tree/devnets/bal/7).
:::

:::info
❗ **Parameters:** `cost_per_state_byte` **`1174 → 1530`** (fixed), `STATE_BYTES_PER_NEW_ACCOUNT` `112 → 120`, `STATE_BYTES_PER_STORAGE_SET` `32 → 64`, `STATE_BYTES_PER_AUTH_BASE` unchanged at `23`. `SYSTEM_CALL_GAS_LIMIT` raised from `30M` to `30M + STATE_BYTES_PER_STORAGE_SET × CPSB × SYSTEM_MAX_SSTORES_PER_CALL` (`SYSTEM_MAX_SSTORES_PER_CALL = 16`); the extra is allocated to `state_gas_reservoir` so system calls cannot OOG on state-gas growth alone.
:::

:::info
❗ **eth/70 (EIP-7975) and eth/71 (EIP-8159) are now mandatory** — moved from `optional` after the SFI check in #block-access-lists (2026-05-08). Every EL must support both for bal-devnet-7.
:::

## EIP List for bal-devnet-7

| EIP | Title | Status |
|--------|------|--------|
| [EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log (incl. CREATE/CREATE2) | |
| [EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds | |
| [EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode | |
| [EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists | |
| [EIP-7954](https://eips.ethereum.org/EIPS/eip-7954) | Increase Maximum Contract Size | |
| [EIP-7975](https://eips.ethereum.org/EIPS/eip-7975) | eth/70 – partial block receipt lists | :exclamation: **required** |
| [EIP-7976](https://eips.ethereum.org/EIPS/eip-7976) | Increase Calldata Floor Cost (64/64) | |
| [EIP-7981](https://eips.ethereum.org/EIPS/eip-7981) | Increase Access List Cost | |
| [EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE | |
| [EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) | State Creation Gas — **CPSB=1530**, system-call gas bump, halt/revert + SELFDESTRUCT refunds | :up: |
| [EIP-8159](https://eips.ethereum.org/EIPS/eip-8159) | eth/71 – Block Access List Exchange | :exclamation: **required** |

**Key:** :up: updated since bal-devnet-6 · :exclamation: scope changed

---

## EIP-8037 spec changes in bal-devnet-7

Per the [`tests-bal@v7.0.0` release notes](https://github.com/ethereum/execution-specs/releases/tag/tests-bal@v7.0.0). EELS branch: `devnets/bal/7`.

| # | Change | EIPs PR | EELS PR / commit |
|---|--------|---------|------------------|
| 1 | Parameters: `CPSB 1174 → 1530`, `NEW_ACCOUNT 112 → 120`, `STORAGE_SET 32 → 64` (whole-suite recalibration) | [#11616](https://github.com/ethereum/EIPs/pull/11616) (open) | [specs#2827](https://github.com/ethereum/execution-specs/pull/2827) |
| 2 | `SYSTEM_CALL_GAS_LIMIT = 30M + STATE_BYTES_PER_STORAGE_SET × CPSB × SYSTEM_MAX_SSTORES_PER_CALL` (16 slots). Extra goes to `state_gas_reservoir` so system calls can't OOG on state-gas alone. | — | commit [`7b3e8016`](https://github.com/ethereum/execution-specs/commit/7b3e8016b1) |
| 3 | Top-level exceptional halt: refund full `state_gas_used`, including the portion that spilled into `gas_left`, back to the reservoir | [#11611](https://github.com/ethereum/EIPs/pull/11611) (open) | [specs#2815](https://github.com/ethereum/execution-specs/pull/2815) |
| 4 | EIP-7702 authorization state-gas refund is included in `block.state_gas_used` — block-level 2D accounting now matches per-tx | [#11611](https://github.com/ethereum/EIPs/pull/11611) (open) | [specs#2816](https://github.com/ethereum/execution-specs/pull/2816) |
| 5 | Tx-level CREATE halt/revert: refund intrinsic `NEW_ACCOUNT × CPSB` charge to the reservoir | [#11611](https://github.com/ethereum/EIPs/pull/11611) (open) | [specs#2823](https://github.com/ethereum/execution-specs/pull/2823) |
| 6 | Isolated `0 → x → 0` SSTORE refund: same-frame revert returns the charge to the reservoir (no propagation as refund) | [#11611](https://github.com/ethereum/EIPs/pull/11611) (open) | [specs#2823](https://github.com/ethereum/execution-specs/pull/2823) |
| 7 | CREATE address-collision: regular gas burned by the collision is now counted in block regular gas | [#11611](https://github.com/ethereum/EIPs/pull/11611) (open) | [specs#2823](https://github.com/ethereum/execution-specs/pull/2823) |
| 8 | Tx-level CREATE + same-tx `SELFDESTRUCT`: refund intrinsic `NEW_ACCOUNT × CPSB` so a non-persisting account isn't over-charged | [#11606](https://github.com/ethereum/EIPs/pull/11606) (merged) | [specs#2828](https://github.com/ethereum/execution-specs/pull/2828) |

Changes 3–8 directly close the bugs Dragan reported in `#el-testing` on 2026-05-04 against `snobal-devnet-6@v1.1.x` (tx-CREATE halt/revert, 7702-in-`block.state_gas_used`, isolated `0→x→0`, CREATE-collision). The `SELFDESTRUCT` charge clarification (#11606) was an outcome of the bal-devnet-6 review and is now spec-canonical.

### Other changes vs. bal-devnet-6

| Area | bal-devnet-6 | bal-devnet-7 |
|------|--------------|--------------|
| Reference block gas limit (for CPSB derivation) | 96M | **150M** — devnet `gas_limit` raised to match |
| Target state growth | 100 GiB/yr | **120 GiB/yr** |
| eth/70 (EIP-7975) | optional | **mandatory** |
| eth/71 (EIP-8159) | optional | **mandatory** |
| `STATE_BYTES_PER_AUTH_BASE` | 23 | 23 (unchanged; called out for completeness) |

---

## Execution Layer Client Support

| EIP | Geth | Besu | Reth | Nethermind | Erigon | Nimbus-EL | Ethrex |
|-----|:----:|:----:|:----:|:----------:|:------:|:---------:|:------:|
| 7708 / 7778 / 7843 / 7928 / 7954 / 7976 / 7981 / 8024 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7975 (eth/70 — **required**) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 8037 (CPSB=1530 + #11611) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 8159 (eth/71 — **required**) | ✅  | ✅ | ✅  | ✅ | ✅ | ✅ | ✅ |

---

## Consensus Layer

Unchanged from bal-devnet-6. CL teams rebuild against `devnets/bal/7` images.

| Feature/EIP | Lodestar | Lighthouse | Prysm |
|-------------|:--------:|:----------:|:-----:|
| EIP-7928 | ✅ | ✅ | bal-devnet-1 |
| EIP-7843 | ✅ | ✅ | ❌ |

`consensus-specs` stays on [`v1.6.1`](https://github.com/ethereum/consensus-specs/releases/tag/v1.6.1).

---

## Test releases

- **Consensus specs:** [`v1.6.1`](https://github.com/ethereum/consensus-specs/releases/tag/v1.6.1) — unchanged.
- **Execution specs (EELS):** [`tests-bal@v7.2.0`](https://github.com/ethereum/execution-specs/releases/tag/tests-bal@v7.2.0). Branch [`devnets/bal/7`](https://github.com/ethereum/execution-specs/tree/devnets/bal/7).




---

## Open execution-apis PRs

| PR | Author | Note |
|----|--------|------|
| [#794](https://github.com/ethereum/execution-apis/pull/794) | `nero_eth` | `BlockAccessIndex` `uint64 → uint32`, `blockAccessListHash` on block header, `debug_getRawBlockAccessList`, error code → `-32001 Resource not found`. **Required for bal-devnet-7.** |
| [#786](https://github.com/ethereum/execution-apis/pull/786) | `mkalinin` | Engine: restrict no-reorg to known-finalized prefix. Required for Glamsterdam, optional here. |

---

## Client BAL features

Unchanged from bal-devnet-6 — per-client flag tables are at https://notes.ethereum.org/@ethpandaops/bal-devnet-6#Feature-flags.

| EL Client | Exec Par. | Batch IO | State Par. |
|-----------|:---------:|:--------:|:----------:|
| Geth        | ✅ | ✅ | ✅ |
| Nethermind  | ✅ | ✅ | ✅ |
| Erigon      | ✅ | ❓ | ❓ |
| Besu        | ✅ | ✅ | ✅ |
| Reth        | ✅ | ✅ | ✅ |

---

## Local testing

```yaml
participants:
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-7
    supernode: true
    el_type: geth
    el_image: ethpandaops/geth:bal-devnet-7
    el_extra_params: ["--history.state=0", "--gcmode=archive", "--syncmode=full"]
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-7
    supernode: true
    el_type: besu
    el_image: ethpandaops/besu:bal-devnet-7
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-7
    supernode: true
    el_type: reth
    el_image: ethpandaops/reth:bal-devnet-7
    el_extra_params: ["--txpool.max-account-slots=256"]
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-7
    supernode: true
    el_type: nethermind
    el_image: ethpandaops/nethermind:bal-devnet-7
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-7
    supernode: true
    el_type: erigon
    el_image: ethpandaops/erigon:bal-devnet-7
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-7
    supernode: true
    el_type: ethrex
    el_image: ethpandaops/ethrex:bal-devnet-7
    el_extra_params: ["--syncmode=full", "--http.api=eth,net,web3,debug,admin,txpool"]
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-7
    supernode: true
    el_type: nimbus
    el_image: ethpandaops/nimbus-eth1:bal-devnet-7
    count: 1
network_params:
  preset: minimal
  seconds_per_slot: 6
  genesis_delay: 30
  fulu_fork_epoch: 0
  gloas_fork_epoch: 2
  # 150M matches the EIP-8037 reference gas limit used to derive CPSB=1530.
  genesis_gaslimit: 150000000
  gas_limit: 150000000
snooper_enabled: true
dora_params:
  image: ethpandaops/dora:eip7928-support
spamoor_params:
  image: ethpandaops/spamoor:master
  spammers:
    - scenario: evm-fuzz
      config: {funding_gas_limit: 2000000, throughput: 50, payload_seed: "0x0400"}
    - scenario: eoatx
      config: {funding_gas_limit: 2000000, throughput: 50, gas_limit: 200000}
    - scenario: deploytx
      config: {funding_gas_limit: 2000000, throughput: 10, bytecodes: "0x6000", gas_limit: 10000000}
    - scenario: storagerefundtx
      config: {funding_gas_limit: 2000000, throughput: 20, slots_per_call: 500}
    - scenario: setcodetx
      config: {funding_gas_limit: 2000000, throughput: 20}
ethereum_genesis_generator_params:
  image: ethpandaops/ethereum-genesis-generator:6.0.2
additional_services: [dora, spamoor]
port_publisher:
  additional_services:
    enabled: true
    public_port_start: 64400
```

---

## Metrics

https://notes.ethereum.org/@ethpandaops/bal-otel

Previous devnet spec sheet: https://notes.ethereum.org/@ethpandaops/bal-devnet-6