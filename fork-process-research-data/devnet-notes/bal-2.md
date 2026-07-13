# bal-devnet-2 spec
:::info
:mega: bal-devnet-2 targets to launch 04 Feb 2026.
([launched 2026-Feb-06 02:29:50 PM UTC](https://github.com/ethpandaops/bal-devnets/blob/master/network-configs/devnet-2/metadata/config.yaml#L27-L28))
:::


:::info
❗ EL clients: please add flags to enable/disable BAL optimizations batch/parallel io and parallel execution.
:::

## EIP List for bal-devnet-2

| Status | EIP | Title | Summary | PRs |
|--------|-----|-------|---------|-----|
| 🆙 | [EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log | All ETH transfers emit a log for unified tracking | [Respec topic & emitting addr + add LOG2 for SELFDESTRUCT @ self](https://github.com/ethereum/EIPs/pull/9003), [Clarify SELFDESTRUCT LOG2 events](https://github.com/ethereum/EIPs/pull/11127) |
| 🆕 | [EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds | Exclude refunds from block gas accounting to prevent limit circumvention | |
| 🆕 | [EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode | New opcode (0x4b) returning the current slot number | |
| 🆙 | [EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists | Enforced block access lists enabling parallel validation and executionless state updates | [spurious entry detection](https://github.com/ethereum/EIPs/pull/10990), [gas validation](https://github.com/ethereum/EIPs/pull/11066),  |
| 🆙 | [EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE | New instructions for stack manipulation beyond the 16-item limit | included WITHOUT [Switch ecoding to push-postfix](https://github.com/ethereum/EIPs/pull/11094) |


**Key:**
- 🆙 EIP has updated!
- 🆕 New EIP added.


### Implementation tracker EL

## Execution Layer Client Support

| EIP | Geth | Besu | Reth | Nethermind | Erigon | Nimbus-EL |
|-----|:----:|:----:|:----:|:----------:|:------:|:---------:|
| 7928 (BAL) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7708 (ETH Logs) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7778 (Gas Refunds) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| 7843 (SLOTNUM) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| 8024 (SWAPN/DUPN) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |



### Implementation tracker CL
| Feature/EIP | Lodestar | Lighthouse | Prysm |
| ----------- | -------- | ---------- | ----- |
| **EIP-7928** | ✅ | ✅ | bal-devnet-1 |
| **EIP-7843** | ✅ | ✅ | ❌ |


### Test Releases

**Consensus Specs:** [`v1.6.1`](https://github.com/ethereum/consensus-specs/releases/tag/v1.6.1) :heavy_check_mark:  Same as [bal-devnet-1](https://notes.ethereum.org/@ethpandaops/bal-devnet-1)

**Execution Specs:**  EEST Block-Level Access Lists (BAL) [v5.1.0](https://github.com/ethereum/execution-spec-tests/releases/tag/bal%40v5.1.0) :heavy_check_mark: :new:



### Spec versions required & Open PRs
**EIP udpates included**

- [EIP-7708 PR#9003 Respec topic & emitting addr](https://github.com/ethereum/EIPs/pull/9003) Merged :heavy_check_mark:
- [EIP-7708 PR#11127 Clarify selfdestruct event](https://github.com/ethereum/EIPs/pull/11127) Merged :heavy_check_mark:
- [EIP-7843 PR#11083 consistent naming, engine api](https://github.com/ethereum/EIPs/pull/11083) Merged :heavy_check_mark:
- [EIP-7928 PR#10990 clarifications](https://github.com/ethereum/EIPs/pull/10990) Merged :heavy_check_mark:, [more clarifications](https://github.com/ethereum/EIPs/pull/11066) Merged :heavy_check_mark:
- [EIP-7928 PR#10895 System contract access clarification](https://github.com/ethereum/EIPs/pull/10895) Merged :heavy_check_mark:
- [EIP-7928 PR#10940 BAL retention period WSP](https://github.com/ethereum/EIPs/pull/10940) Merged :heavy_check_mark:
- [EIP-7708 PR#11187 Clarify CALL to self does not emit transfer log](https://github.com/ethereum/EIPs/pull/11187) Merged :heavy_check_mark: :new:
- [EIP-7708 PR#11190 clarify order of burn logs and coinbase transfer logs](https://github.com/ethereum/EIPs/pull/11190) Merged :heavy_check_mark: :new:
- [EIP-7708 PR#11127 Clarify selfdestruct event](https://github.com/ethereum/EIPs/pull/11127) Merged :heavy_check_mark: :new:
- [EIP-7778 PR#11191 Revert back to not having a receipt field](https://github.com/ethereum/EIPs/pull/11191) Merged :heavy_check_mark: :new:
- [EIP-7778 PR#11138 clarify that user gas accounting remains unchanged](https://github.com/ethereum/EIPs/pull/11138) Merged :heavy_check_mark: :new:

**Consensus Specs**

Currently, No CL spec changes required.

CLs need to implement [EIP-7843 PR #731 engine_forkchoiceUpdatedV4](https://github.com/ethereum/execution-apis/pull/731)

**Execution Specs**

- [EIP-7708 PR#2023 ETH Transfer Emit](https://github.com/ethereum/execution-specs/pull/2023) Merged :heavy_check_mark: 
- [EIP-7778 PR#1401 Block Gas Accounting](https://github.com/ethereum/execution-specs/pull/1401) Merged :heavy_check_mark: 
- [EIP-7843 PR#2007 SlotNum Opcode](https://github.com/ethereum/execution-specs/pull/2007) Merged :heavy_check_mark: 
- [EIP-7928 PR#1719 feat(specs,tests): Implement EIP-7928 Block-Level Access Lists](https://github.com/ethereum/execution-specs/pull/1719) with: [PR#1917 feat(specs): EIP-7928 move bal from payload](https://github.com/ethereum/execution-specs/pull/1917) Merged :heavy_check_mark: 
- [EIP-8024 PR#2021 feat(src): EIP-8024 tests added](https://github.com/ethereum/execution-specs/pull/2021) Merged :heavy_check_mark:

**Execution APIs**

[Amsterdam Engine API spec](https://github.com/ethereum/execution-apis/blob/main/src/engine/amsterdam.md)

- [EIP-7843 PR #731 engine_forkchoiceUpdatedV4](https://github.com/ethereum/execution-apis/pull/731) Merged :heavy_check_mark:
- [EIP-7928 PR #691: ExecutionPayloadV4](https://github.com/ethereum/execution-apis/pull/691) Merged :heavy_check_mark: 
- [EIP-7928 PR#726: Add EIP-7928 Block-level Access Lists JSON RPC methods](https://github.com/ethereum/execution-apis/pull/726) Open :exclamation: 
- [EIP-7928 PR#727 engine: define EIP-7928 API methods](https://github.com/ethereum/execution-apis/pull/727) Merged :heavy_check_mark: 

<!-- **devp2p**
- [EIP-7928 GetBlockAccessLists, BlockAccessLists p2p](https://github.com/ethereum/devp2p/pull/264) Open :exclamation: -->

**Other**

- [bal-devnet-2 STEEL Milestone tracker](https://github.com/ethereum/execution-specs/milestone/32) :heavy_check_mark:
- [EIP-7928 Issue#1938 Test vector metadata consideration](https://github.com/ethereum/execution-specs/issues/1938) Open :exclamation:


## Client BAL features

| EL Client   | Exec Par. | Batch IO (required) | State Par. |
|-------------|:---------:|:--------:|:----------:|
| Geth        | ✅        | ✅       | ✅         |
| Nethermind  | ❓        | ❓       | ✅         |
| Erigon      | ✅        | ❓       | ❓         |
| Besu        | ✅        | ✅       | ✅         |
| Reth        | ✅        | ❓       | ❓         |

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
```
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
    disable_bal_batch_io: bool,
```

```
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



---

## Local testing

Kurtosis example:

```yaml=
participants:
  - cl_type: lighthouse
    cl_image: ethpandaops/lighthouse:bal-devnet-2
    el_type: geth
    el_image: ethpandaops/geth:bal-devnet-2
    el_extra_params: ["--history.state=0", "--gcmode=archive", "--syncmode=full"]
    count: 1
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-2
    el_type: geth
    el_image: ethpandaops/geth:bal-devnet-2
    el_extra_params: ["--history.state=0", "--gcmode=archive", "--syncmode=full"]
    count: 2
global_log_level: 'debug'
network_params:
  genesis_delay: 20
  fulu_fork_epoch: 0
  gloas_fork_epoch: 1
  seconds_per_slot: 6
snooper_enabled: true
dora_params:
  image: ethpandaops/dora:eip7928-support
ethereum_genesis_generator_params:
  image: ethpandaops/ethereum-genesis-generator:master
additional_services:
  - dora
port_publisher:
  additional_services:
    enabled: true
    public_port_start: 64400
```



## Metrics

https://notes.ethereum.org/@ethpandaops/bal-otel