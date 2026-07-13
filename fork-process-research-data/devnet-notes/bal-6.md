# bal-devnet-6 spec

:::info
:mega: bal-devnet-6 target launch: **Friday 01.05**.
:::

:::info
BAL benchmark dashboard https://nerolation.github.io/bal-dashboard/. Based on data from https://benchmarkoor.core.ethpandaops.io. Requires benchmarkoor api key.
:::

:::info
❗ EIP-8037: bal-devnet-6 keeps the **static** `cost_per_state_byte = 1174` from bal-devnet-3. We will use opcode level accounting.
:::

:::info
❗ Primary focus of this devnet is **EIP-8037 stabilization**: picking up the spec clarifications, follow-up tests, and the spec decisions from the [state gas accounting review](https://github.com/misilva73/evm-gas-repricings/blob/main/reports/eip-8037/spec_review_state_gas_accounting.md).
:::

:::info
❗ **EIP-7976 and EIP-7981 must be implemented** by all EL clients for bal-devnet-6. Both were added post-ACDT; spec/test side is ready, client implementations are the remaining gate.
:::

:::success
✅ **All 7 EIP-8037 state gas accounting decisions resolved** at Glamsterdam Repricing Breakout #6 (2026-04-15). [Recording](https://www.youtube.com/watch?v=U8RvbLPX188). See "Changes vs. bal-devnet-3" below for the full decision list.
:::

## EIP List for bal-devnet-6

| EIP | Title | Status |
|--------|------|--------|
| [EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log — **now includes `CREATE`/`CREATE2`** | :up: |
| [EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds | |
| [EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode | |
| [EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists — **block access index `uint32`** | :up: |
| [EIP-7954](https://eips.ethereum.org/EIPS/eip-7954) | Increase Maximum Contract Size | |
| [EIP-7975](https://eips.ethereum.org/EIPS/eip-7975) | eth/70 – partial block receipt lists | optional |
| [EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE | |
| [EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) | State Creation Gas Cost Increase — ~~dynamic `cpsb`~~ **static `cpsb=1174`** :exclamation: | :up: |
| [EIP-8159](https://eips.ethereum.org/EIPS/eip-8159) | eth/71 – Block Access List Exchange — **empty BAL response `0x80`** | optional, :up: |
| [EIP-7976](https://eips.ethereum.org/EIPS/eip-7976) | Increase Calldata Floor Cost (64/64) | :new: |
| [EIP-7981](https://eips.ethereum.org/EIPS/eip-7981) | Increase Access List Cost | :new: |

**Key:**
- :up: EIP has updated since bal-devnet-3
- :new: New EIP added

---

## Changes vs. bal-devnet-3

| Area | bal-devnet-3 | bal-devnet-6 |
|------|--------------|--------------|
| EIP-8037 `cost_per_state_byte` | Hardcoded `1174` (fork constant) | **Dynamic**, derived from header gas limit (quantized) |
| EIP-8037 CREATE initcode ordering | Pre-check charge | Charge moved **after** initcode size validation ([specs#2608](https://github.com/ethereum/execution-specs/pull/2608)) |
| EIP-8037 cross-EIP interaction | — | EIP-8024 + EIP-8037 test regressions fixed ([specs#2656](https://github.com/ethereum/execution-specs/pull/2656)) |
| EIP-7708 log emission | `CALL*` + `SELFDESTRUCT` | `CALL*` + `SELFDESTRUCT` + **`CREATE` / `CREATE2`** — [EIPs#11474](https://github.com/ethereum/EIPs/pull/11474) merged 2026-04-20. Every EL must emit ETH-transfer logs on contract creation. |
| EIP-7928 block access index | `uint64` | `uint32` — [EIPs#11550](https://github.com/ethereum/EIPs/pull/11550) merged 2026-04-20 |
| EIP-8159 empty BAL response | `0xc0` (empty list) | `0x80` (empty string) — [EIPs#11553](https://github.com/ethereum/EIPs/pull/11553) merged 2026-04-21, **after `snøbal-devnet-6@v1.0.0` cut**. Matches EIP-8189 / snap/1 `ByteCodes`/`TrieNodes` convention; avoids collision with empty BALs on chains without system contracts. |
| Top-level reservoir refund | State gas persists on top-level failure | **Zeroed on failure.** See change #1. |
| 0→x→0 SSTORE reservoir refill | Refund via capped `refund_counter` (20%) | **Refund directly to reservoir** — no cap. See change #2. |
| CREATE failure state gas | Charged unconditionally in caller frame | **Upfront charge kept, refund to reservoir** on silent failure (balance, nonce, collision, stack depth). See change #3. |
| Same-tx SELFDESTRUCT refund | No refund of create state gas | **Refund state gas + storage costs** to reservoir at end of tx when SELFDESTRUCT destroys a same-tx-created account. See change #4. |
| CALL to self-destructed same-tx account | No state gas charge (`is_account_alive` correct) | **No change** — behaviour is correct. See change #5. |
| EIP-7702 `intrinsic_state_gas` | Mutated mid-execution on delegation to existing account | **Stop mutating** — keep worst-case upfront charge, refund to reservoir when auth targets existing account. See change #6. |
| Tx inclusion validity | 1D regular-gas check only | **2D check** — both regular gas and state gas validated per-tx before execution. See change #7. |
| Spillover classification | Ambiguous (bal-devnet-3 client split root cause) | **Clarified** — [EIPs#11522](https://github.com/ethereum/EIPs/pull/11522) merged 2026-04-14 |

---

## Execution Layer Client Support

| EIP |               Geth | Besu | Reth | Nethermind | Erigon | Nimbus-EL | Ethrex |
|-----|              :----:|:----:|:----:|:----------:|:------:|:---------:|:------:|
| 7708 (ETH Logs)    | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7778 (Gas Refunds) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7843 (SLOTNUM)     | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7928 (BAL)         | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7954 (MContract)   | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7975 (eth/70)      | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| 8024 (SWAPN/DUPN)  | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 8037 (dyn-cpsb)    | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: |
| 7976 (Calldata)    | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: |
| 7981 (AL Cost)     | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: |
| 8159 (eth/71)      | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | :hammer: |

### bal-devnet-3 regression status (needs re-verification on devnet-6 branches)

| Bug | Client | Status | Reference |
|-----|--------|--------|-----------|
| Nethermind: tx gas-limit validated against 1D `header.GasUsed` — artificially shrinks available budget when state dim dominates | Nethermind | Fixed on `bal-devnet-3` branch | [nethermind compare](https://github.com/NethermindEth/nethermind/compare/bal-devnet-3...qu0b:qu0b/fix/newaccount-stategas) |
| Besu: reservoir consumed on top-level exceptional halt after child spill-restore (delta = `112 × cpsb`) | Besu | Fixed in `f4c8b36` | [besu compare](https://github.com/besu-eth/besu/compare/bal-devnet-3...qu0b:qu0b/fix/reservoir-zero-on-halt) |
| Geth: nested CREATE failure incorrectly restores `GAS_NEW_ACCOUNT` to parent reservoir | Geth | Reported | Tests landed in [execution-specs#2718](https://github.com/ethereum/execution-specs/pull/2718) (rework of closed #2639); now in `snøbal-devnet-6@v1.0.0` |
| Erigon: 2D gas pool | Erigon | Fixed on `bal-devnet-3` | [erigon compare](https://github.com/erigontech/erigon/compare/bal-devnet-3...qu0b:qu0b/fix/gaspool-2d) |

---

## Consensus Layer Support

| Feature/EIP | Lodestar | Lighthouse | Prysm |
|-------------|----------|------------|-------|
| EIP-7928     | ✅ | ✅ | bal-devnet-1 |
| EIP-7843     | ✅ | ✅ | ❌ |

No CL spec changes required for bal-devnet-6 (same as bal-devnet-3). CL teams only need to rebuild against `devnets/bal/4` tagged images.

---

## Test Releases

- **Consensus Specs:** [`v1.6.1`](https://github.com/ethereum/consensus-specs/releases/tag/v1.6.1) — unchanged from bal-devnet-3.
- **Execution Specs:** [`snøbal-devnet-6@v1.0.0`](https://github.com/ethereum/execution-spec-tests/releases/tag/snøbal-devnet-6@v1.0.0) — tagged 2026-04-21 on `devnets/bal/4` (commit `524b446`).

---

## Spec versions required & Open PRs

### EIP-8037 state-gas changes

All 7 state-gas changes from the 2026-04-15 breakout plus the full dynamic `cost_per_state_byte` are now in both canonical EIP-8037 text and `tests-bal@v5.7.0`.

| # | Change | EIPs PR | EELS PR |
|---|----------|---------|---------|
| 1 | Top-level failure — zero execution state gas | [EIPs#11476](https://github.com/ethereum/EIPs/pull/11476) | [specs#2689](https://github.com/ethereum/execution-specs/pull/2689) |
| 2 | 0→x→0 SSTORE — refund direct to reservoir (no `refund_counter`) | [EIPs#11532](https://github.com/ethereum/EIPs/pull/11532), [EIPs#11548](https://github.com/ethereum/EIPs/pull/11548) | [specs#2698](https://github.com/ethereum/execution-specs/pull/2698) |
| 3 | CREATE silent-failure — upfront charge kept, refund to reservoir | [EIPs#11532](https://github.com/ethereum/EIPs/pull/11532) | [specs#2704](https://github.com/ethereum/execution-specs/pull/2704) |
| 4 | Same-tx SELFDESTRUCT — refund state gas + storage costs at end of tx | [EIPs#11532](https://github.com/ethereum/EIPs/pull/11532) | [specs#2707](https://github.com/ethereum/execution-specs/pull/2707) |
| 5 | CALL to same-tx selfdestructed account — no change (test only) | [EIPs#11532](https://github.com/ethereum/EIPs/pull/11532) | [specs#2646](https://github.com/ethereum/execution-specs/pull/2646) |
| 6 | EIP-7702 `intrinsic_state_gas` — stop mid-execution mutation, refund when auth targets existing account | [EIPs#11532](https://github.com/ethereum/EIPs/pull/11532) | [specs#2711](https://github.com/ethereum/execution-specs/pull/2711) |
| 7 | Per-dimension tx inclusion check (2D regular + state gas) | [EIPs#11536](https://github.com/ethereum/EIPs/pull/11536) | [specs#2703](https://github.com/ethereum/execution-specs/pull/2703) |

**Open spec gap:** change 2 ↔ 4 nested-child-frame refund interaction is implemented in tests via [specs#2733](https://github.com/ethereum/execution-specs/pull/2733); canonical EIP-8037 text still needs a clarification PR.


### Execution APIs

| PR | Title | Author | Note |
|----|-------|--------|------|
| [#786](https://github.com/ethereum/execution-apis/pull/786) | engine: Restrict no-reorg to the prefix of known finalized | mkalinin | required for glamsterdam-devnets, optional for bal-devnet-6 |

---

## Client BAL features

Unchanged from bal-devnet-3. Flags below let us toggle BAL-based optimizations independently.

| EL Client   | Exec Par. | Batch IO (required) | State Par. |
|-------------|:---------:|:-------------------:|:----------:|
| Geth        | ✅ | ✅ | ✅ |
| Nethermind  | ✅ | ✅ | ✅ |
| Erigon      | ✅ | ❓ | ❓ |
| Besu        | ✅ | ✅ | ✅ |
| Reth        | ✅ | ✅ | ✅ |

## Feature flags

### Besu

| Option | Default | Type | Description |
|--------|---------|------|-------------|
| `--Xbal-optimization-enabled` | `true` | boolean | Disable BAL-based optimizations. |
| `--Xbal-perfect-parallelization-enabled` | `true` | boolean | Disable BAL-based perfect parallelization. |
| `--Xbal-lenient-on-state-root-mismatch` | `true` | boolean | Log instead of throw on state-root mismatch. |
| `--Xbal-trust-state-root` | `false` | boolean | Trust BAL-computed state root without verification. |
| `--Xbal-log-bals-on-mismatch` | `false` | boolean | Log constructed-block BAL when they differ. |
| `--Xbal-api-enabled` | `false` | boolean | Enable `eth_getBlockAccessListByBlock*` and BALs in simulation. |
| `--Xbal-state-root-timeout` | `1000` | long | Timeout (ms) awaiting BAL-computed state root. |
| `--Xbal-processing-timeout` | `1000` | long | Timeout (ms) awaiting BAL tx processing results. |
| `--Xbal-prefetch-reading-enabled` | `false` | boolean | Prefetch state based on BAL read ops. |
| `--Xbal-prefetch-sorting-enabled` | `true` | boolean | Sort optimization during BAL prefetch. |

### Reth

```rust
/// EIP-7928 BAL-based parallel execution. Falls back to tx-based prewarming when disabled.
disable_bal_parallel_execution: bool,
/// BAL-driven parallel state-root computation. Hashed post-state not sent to multiproof task when disabled.
disable_bal_parallel_state_root: bool,
/// BAL batched IO during prewarming. Falls back to per-slot reads when disabled.
disable_bal_batch_io: bool,
```

CLI: `--engine.disable-bal-parallel-execution`, `--engine.disable-bal-parallel-state-root`, `--engine.disable-bal-batch-io`.

### Geth

`--bal.executionmode=<full|sequential|nobatchio>`.

### Nethermind

```
--Blocks.ParallelExecution=true 
--Blocks.ParallelExecutionBatchRead=true
```

### Erigon

WIP. `IGNORE_BAL` env flag from [erigon#19903](https://github.com/erigontech/erigon/pull/19903).

---

## Local testing

Kurtosis example (bump `bal-devnet-3` tags to `bal-devnet-6` once client branches exist):

```yaml
participants:
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-6
    supernode: true
    cl_log_level: debug
    el_type: geth
    el_image: ethpandaops/geth:bal-devnet-6
    el_extra_params: ["--history.state=0", "--gcmode=archive", "--syncmode=full"]
    el_log_level: debug
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-6
    supernode: true
    cl_log_level: debug
    el_type: besu
    el_image: ethpandaops/besu:bal-devnet-6
    el_log_level: debug
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-6
    supernode: true
    cl_log_level: debug
    el_type: reth
    el_image: ethpandaops/reth:bal-devnet-6
    el_extra_params: ["--txpool.max-account-slots=256"]
    el_log_level: debug
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-6
    supernode: true
    cl_log_level: debug
    el_type: nethermind
    el_image: ethpandaops/nethermind:bal-devnet-6
    el_log_level: debug
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-6
    supernode: true
    cl_log_level: debug
    el_type: erigon
    el_image: ethpandaops/erigon:bal-devnet-6
    el_log_level: debug
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-6
    supernode: true
    cl_log_level: debug
    el_type: ethrex
    el_image: ethpandaops/ethrex:bal-devnet-6
    el_extra_params: ["--syncmode=full"]
    el_extra_env_vars:
      RUST_LOG: "info"
    el_log_level: debug
    count: 1
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-6
    supernode: true
    cl_log_level: debug
    el_type: nimbus
    el_image: ethpandaops/nimbus-eth1:bal-devnet-6
    el_log_level: debug
    count: 1
network_params:
  preset: minimal
  seconds_per_slot: 6
  genesis_delay: 30
  fulu_fork_epoch: 0
  gloas_fork_epoch: 2
  # 100M block gas limit per devnet-4 spec; also scales EIP-8037 cpsb correctly.
  genesis_gaslimit: 100000000
  gas_limit: 100000000
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

Previous devnet spec sheet: https://notes.ethereum.org/@ethpandaops/bal-devnet-3