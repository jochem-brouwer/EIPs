# glamsterdam-devnet-0 spec
:::info
:mega: glamsterdam-devnet-0 targets to launch on the 24th of April 2026.
:::



:::info
❗ EL clients: **EIP-8037** ~~will switch from hard coded **cost per state byte** to a variable cost based on the block gas limit. Similar to bal-devnet-3 or 4.~~ Will stay static.
:::

## EIP List for glamsterdam-devnet-0

| EIP | Title |Status
|--------|-----|-------|
|[EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log |
|[EIP-7732](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7732.md) | Enshrined Proposer-Builder Separation | :up:
|[EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds |
|[EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode |
|[EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists | :up:
|[EIP-7954](https://eips.ethereum.org/EIPS/eip-7954) | Increase Maximum Contract Size |
|[EIP-7975](https://eips.ethereum.org/EIPS/eip-7975) | eth/70 - partial block receipt lists | optional
|[EIP-7976](https://eips.ethereum.org/EIPS/eip-7976) | Increase Calldata Floor Cost (64/64) | :new: - maybe
|[EIP-7981](https://eips.ethereum.org/EIPS/eip-7981) | Increase Access List Cost | :new: - maybe
|[EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE
|[EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) | State Creation Gas Cost Increase | :up:
|[EIP-8159](https://eips.ethereum.org/EIPS/eip-8159) | eth/71 - Block Access List Exchange | optional



**Key:**
- :up: EIP has updated!
- :new: New EIP added.


### Implementation tracker EL

## Execution Layer Client Support
Here you go:

| EIP |               Geth | Besu | Reth | Nethermind | Erigon | Nimbus-EL | Ethrex |
|-----|              :----:|:----:|:----:|:----------:|:------:|:---------:|:------:|
| 7708 (ETH Logs)    | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7778 (Gas Refunds) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7843 (SLOTNUM)     | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7928 (BAL)         | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7954 (MContract)   | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| 7975 (eth/70)      | ✅ | :hammer: | ❌ | ✅ | ❌ | ❌ | ❌ |
| 8024 (SWAPN/DUPN)  | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 8037 (stateIncr)   | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| 8159 (eth/71)      | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| 7732 ePBS          | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |


### Implementation tracker CL
| Feature/EIP | Lodestar | Lighthouse | Prysm |
| ----------- | -------- | ---------- | ----- |
| **EIP-7928** |  |  | glamsterdam-devnet-0 |


### Test Releases

**Consensus Specs:** [v1.7.0-alpha.5](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.5) - :heavy_check_mark: 


**Execution Specs:** [bal@v5.6.1](https://github.com/ethereum/execution-spec-tests/releases/tag/bal@v5.6.1) or [bal@v5.7.0](https://github.com/ethereum/execution-spec-tests/releases/tag/bal@v5.7.0) pending release :exclamation: pending devnet3 or 4 scoping


### Spec versions required & Open PRs

**EIPs**
- [PR-11476 - Update EIP-8037: Refund state gas on all frame failures including top level](https://github.com/ethereum/EIPs/pull/11476) Merged :heavy_check_mark:

<!--
Top-level reservoir refund (ethereum/EIPs#11476): Currently, subcall revert/halt returns state gas to the parent's reservoir, but at depth 0 the sender still pays for state gas even when state changes are rolled back. The proposed change resets execution_state_gas_used to zero on top-level failure too, making behavior consistent at all call depths. Consensus is forming on the latter, but needs more discussion to isolate edge cases.
https://github.com/ethereum/EIPs/pull/11468 or https://github.com/ethereum/EIPs/pull/11476
0 -> x -> 0 storage reservoir refill: When storage goes 0 -> x -> 0 in the same transaction, the net state growth is zero but state gas is not returned to the reservoir. @rakita proposes refilling the reservoir when storage returns to its original zero value, analogous to how EIP-3529 refunds regular gas for storage clearing. Same applies to SELFDESTRUCT of a locally created account (CREATE charges state gas, SELFDESTRUCT removes it, net zero). No EIP PR yet and needs more discussion.
-->

**Builder Specs**
- [PR-138 - Staked Builder API for Glamsterdam](https://github.com/ethereum/builder-specs/pull/138) Open :exclamation:

**Consensus Specs**
- [PR-4817 - Onboard builders at the fork](https://github.com/ethereum/consensus-specs/pull/4817) Merged :heavy_check_mark:
- [PR-4831 - DoS prevention measures for p2p bids](https://github.com/ethereum/consensus-specs/pull/4831) Merged :heavy_check_mark:
- [PR-4840 - Add support for eip7843 to Gloas](https://github.com/ethereum/consensus-specs/pull/4840) Merged :heavy_check_mark:
- [PR-4892 - Remove impossible branch in forkchoice](https://github.com/ethereum/consensus-specs/pull/4892) Merged :heavy_check_mark: 
- [PR-5056 - Add check on bid gossip for blob kzg commitment len](https://github.com/ethereum/consensus-specs/pull/5056) Merged :heavy_check_mark:
- [PR-5067 - Fix Gloas genesis anchor state](https://github.com/ethereum/consensus-specs/pull/5067) Merged :heavy_check_mark:
- [PR-5069 - Use expected withdrawals from state when parent block is empty](https://github.com/ethereum/consensus-specs/pull/5069) Merged :heavy_check_mark:
- [PR-5094 - Defer payload processing to next block](https://github.com/ethereum/consensus-specs/pull/5094) Merged :heavy_check_mark:
- [PR-5095 - Remove epoch param from slot deadline functions](https://github.com/ethereum/consensus-specs/pull/5095) Merged :heavy_check_mark:
- [PR-5113 - Swap latest_block_hash / latest_execution_payload_bid](https://github.com/ethereum/consensus-specs/pull/5113) Merged :heavy_check_mark:

**Execution APIs**
- [PR-786 - engine: Restrict no-reorg to the prefix of known finalized](https://github.com/ethereum/execution-apis/pull/786) Merged :heavy_check_mark: 

**Networking**
- [EIP-7928 GetBlockAccessLists, BlockAccessLists p2p](https://github.com/ethereum/devp2p/pull/264) Open :exclamation:

**Execution Specs**

**Other**

---

## Local testing

Kurtosis example:
```yaml title="kurtosis.yaml"
participants_matrix:
  el:
    - el_type: erigon
      el_image: ethpandaops/erigon:glamsterdam-devnet-0
  cl:
    - cl_type: prysm
      cl_image: ethpandaops/prysm-beacon-chain:glamsterdam-devnet-0-minimal
    - cl_type: lodestar
      cl_image: ethpandaops/lodestar:glamsterdam-devnet-0
    - cl_type: lighthouse
      cl_image: ethpandaops/lighthouse:glamsterdam-devnet-0
    - cl_type: nimbus
      cl_image: ethpandaops/nimbus-eth2:glamsterdam-devnet-0-minimal
    - cl_type: teku
      cl_image: ethpandaops/teku:glamsterdam-devnet-0
  count: 1

network_params:
  gloas_fork_epoch: 1
  preset: minimal
  withdrawal_type: "0x01"
  validator_balance: 40000

additional_services:
  - dora
  - assertoor
  - spamoor
  - checkpointz
spamoor_params:
  image: ethpandaops/spamoor:master

assertoor_params:
  run_stability_check: false
  run_block_proposal_check: false
  tests:
    - { file: "https://raw.githubusercontent.com/ethpandaops/assertoor/refs/heads/master/playbooks/gloas-dev/builder-lifecycle.yaml" }

snooper_enabled: false
global_log_level: debug

port_publisher:
  additional_services:
    enabled: true

checkpointz_params:
  image: ethpandaops/checkpointz:gloas-latest

ethereum_genesis_generator_params:
  image: ethpandaops/ethereum-genesis-generator:6.0.3
```

---


## Client BAL features

| EL Client   | Exec Par. | Batch IO (required) | State Par. |
|-------------|:---------:|:--------:|:----------:|
| Geth        | ✅        | ✅       | ✅         |
| Nethermind  | ❓        | ❓       | ✅         |
| Erigon      | ✅        | ❓       | ❓         |
| Besu        | ✅        | ✅       | ✅         |
| Reth        | ✅        | ✅       | ✅         |

These flags allow us to enable / disable devnet-2 features. Especially BAL related features.

## Feature flags

### Besu


| Option                                         | Default | Type    | Description                                                                                                            |
|------------------------------------------------|---------|---------|------------------------------------------------------------------------------------------------------------------------|
| `--Xbal-optimization-enabled`                  | `true`  | boolean | Allows disabling BAL-based optimizations.                                                                              |
| `--Xbal-perfect-parallelization-enabled`       | `true`  | boolean | Allows disabling BAL-based perfect parallelization even when BALs are present.                                         |
| `--Xbal-lenient-on-state-root-mismatch`        | `true`  | boolean | Log an error instead of throwing when the BAL-computed state root does not match the synchronously computed root.      |
| `--Xbal-trust-state-root`                      | `false` | boolean | Trust the BAL-computed state root without verification.                                                                |
| `--Xbal-log-bals-on-mismatch`                  | `false` | boolean | Log the constructed block’s BAL when they differ.                                                                      |
| `--Xbal-api-enabled`                           | `false` | Boolean | Enable `eth_getBlockAccessListByBlockNumber` and `eth_getBlockAccessListByBlockHash` methods and BALs in simulation.   |
| `--Xbal-state-root-timeout`                    | `1000`  | long    | Timeout in ms when waiting for the BAL-computed state root.                                                            |
| `--Xbal-processing-timeout`                    | `1000`  | long    | Timeout in ms when waiting for BAL transaction processing results.                                                     |
| `--Xbal-prefetch-reading-enabled`              | `false` | boolean | Enable prefetching of state data based on BAL read operations.                                                         |
| `--Xbal-prefetch-sorting-enabled`              | `true`  | boolean | Enable sorting optimization during BAL prefetch operations.                                                            |
### Reth



```rust
/// Whether to disable BAL (Block Access List, EIP-7928) based parallel execution.
/// When disabled, falls back to transaction-based prewarming even when a BAL is available.
    disable_bal_parallel_execution: bool,
/// Whether to disable BAL-driven parallel state root computation.
/// When disabled, the BAL hashed post state is not sent to the multiproof task for
/// early parallel state root computation.
    disable_bal_parallel_state_root: bool,

/// Whether to disable BAL (Block Access List) batched IO during prewarming.
/// When disabled, falls back to individual per-slot storage reads instead of
/// batched cursor reads via `storage_range`.
    disable_bal_batch_io: bool

Flags:
      --engine.disable-bal-parallel-execution
          Disable BAL (Block Access List, EIP-7928) based parallel execution. When set, falls back to transaction-based prewarming even when a BAL is available

      --engine.disable-bal-parallel-state-root
          Disable BAL-driven parallel state root computation. When set, the BAL hashed post state is not sent to the multiproof task for early parallel state root computation

      --engine.disable-bal-batch-io
          Disable BAL (Block Access List) batched IO during prewarming. When set, falls back to individual per-slot storage reads instead of batched cursor reads

```
### Nethermind

WIP

### Geth

Possible values full, sequential, nobatchio.

`--bal.executionmode=nobatchio`

### Erigon

WIP

IGNORE_BAL (see https://github.com/erigontech/erigon/pull/19903)








## Metrics

https://notes.ethereum.org/@ethpandaops/bal-otel

Previous devnet spec sheet for reference: https://notes.ethereum.org/@ethpandaops/bal-devnet-2