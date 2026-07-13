# glamsterdam-devnet-5 spec
:::info
:mega: glamsterdam-devnet-5 targets to launch on 4th of June 2026 and shut off on 25th of June 2026.
:::

:::info
:checkered_flag: **First `glamsterdam-` prefixed CL+EL devnet** combining `consensus-specs` **v1.7.0-alpha.10** with the **bal-devnet-7** EL feature set, plus the new `targetGasLimit` engine API field ([execution-apis#796](https://github.com/ethereum/execution-apis/pull/796) ↔ [consensus-specs#5235](https://github.com/ethereum/consensus-specs/pull/5235)).
:::

:::info
❗ **`targetGasLimit` on `PayloadAttributesV4`** — CL now passes the per-validator target gas limit to the EL on `engine_forkchoiceUpdatedV4`, replacing the static EL-side flag. CL also enforces `target_gas_limit` consistency with the block's `gas_limit` (CL PR-5236).
:::


## EIP List for glamsterdam-devnet-5

| EIP | Title |Status
|--------|-----|-------|
|[EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log |
|[EIP-7732](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7732.md) | Enshrined Proposer-Builder Separation | :up:
|[EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds |
|[EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode |
|[EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists |
|[EIP-7954](https://eips.ethereum.org/EIPS/eip-7954) | Increase Maximum Contract Size |
|[EIP-7975](https://eips.ethereum.org/EIPS/eip-7975) | eth/70 - partial block receipt lists |
|[EIP-7976](https://eips.ethereum.org/EIPS/eip-7976) | Increase Calldata Floor Cost (64/64) |
|[EIP-7981](https://eips.ethereum.org/EIPS/eip-7981) | Increase Access List Cost |
|[EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE
|[EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) | State Creation Gas — **CPSB=1530**, system-call gas bump, halt/revert + SELFDESTRUCT refunds |
|[EIP-8045](https://eips.ethereum.org/EIPS/eip-8045) | Exclude slashed validators from proposing | :new:
|[EIP-8061](https://eips.ethereum.org/EIPS/eip-8061) | Increase exit and consolidation churn |
|[EIP-8159](https://eips.ethereum.org/EIPS/eip-8159) | eth/71 - Block Access List Exchange | 
|[EIP-8189](https://eips.ethereum.org/EIPS/eip-8189) | snap/2 - BAL-Based State Healing | :new: optional



**Key:**
- :up: EIP has updated!
- :new: New EIP added.
- :exclamation: scope changed (now required)


### Implementation tracker EL


| EIP |               Geth | Besu | Reth | Nethermind | Erigon | Nimbus-EL | Ethrex |
|-----|              :----:|:----:|:----:|:----------:|:------:|:---------:|:------:|
| 7708 / 7778 / 7843 / 7928 / 7954 / 7976 / 7981 / 8024 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7975 (eth/70 — **required**)   | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 8037 (CPSB=1530 + #11611)      | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |
| 8159 (eth/71 — **required**)   | ✅  | ✅ | ✅  | ✅ | ✅ | ✅ | ✅  |
| 7732 ePBS                       | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| engine: `targetGasLimit` (#796) | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: | :hammer: |


### Implementation tracker CL
| Feature/EIP | Lodestar | Lighthouse | Prysm |
| ----------- | -------- | ---------- | ----- |
| **EIP-7928** | ✅ | ✅ | glamsterdam-devnet-0 |
| **alpha.9 rebase** | :hammer: | :hammer: | :hammer: |
| **`target_gas_limit` (#5235)** | :hammer: | :hammer: | :hammer: |
| **EIP-8045** | :hammer: | :hammer: | :hammer: |


### Test Releases

**Consensus Specs:** [v1.7.0-alpha.10](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.10)  :heavy_check_mark: 


**Execution Specs (EELS):** [tests-bal@v7.2.0](https://github.com/ethereum/execution-specs/releases/tag/tests-bal@v7.2.0) — branch [`devnets/bal/7`](https://github.com/ethereum/execution-specs/tree/devnets/bal/7) :heavy_check_mark:


### Spec versions required & Open PRs

**EIPs** (new since bal-devnet-7)

_None — all EIP updates were already incorporated into bal-devnet-7. See [bal-devnet-7](https://notes.ethereum.org/@ethpandaops/bal-devnet-7) for the historical EIP PR list._

**Builder Specs**

**Beacon API**
[PR#613](https://github.com/ethereum/beacon-APIs/pull/613) Pluralize gloas execution payload endpoint paths Open :exclamation: 

**Consensus Specs** (update PRs between `v1.7.0-alpha.8` and `v1.7.0-alpha.9`)

[PR#5300](https://github.com/ethereum/consensus-specs/pull/5300) Precompute head for `update_proposer_boost_root`
[PR#5302](https://github.com/ethereum/consensus-specs/pull/5302) Ensure bids are for a higher slot than their parent
[PR#5250](https://github.com/ethereum/consensus-specs/pull/5250) Embed `payload_status` into `head` fork choice check
[PR#5281](https://github.com/ethereum/consensus-specs/pull/5281) Ignore PTC attestations for empty assigned slots
[PR#5310](https://github.com/ethereum/consensus-specs/pull/5310) Use `should_build_on_full` for bid parent block hash
[PR#5309](https://github.com/ethereum/consensus-specs/pull/5309) Limit `should_build_on_full` checks to the previous slot

**Execution APIs**

**Networking**
- [EIP-7928 GetBlockAccessLists, BlockAccessLists p2p](https://github.com/ethereum/devp2p/pull/264) Open :exclamation:

**Execution Specs**

**Other**



## Local testing

Kurtosis example:
```yaml title="kurtosis.yaml"
participants_matrix:
  el:
    - el_type: ethrex
      el_image: ethpandaops/ethrex:glamsterdam-devnet-5
    - el_type: nethermind
      el_image: ethpandaops/nethermind:glamsterdam-devnet-5
  cl:
    - cl_type: prysm
      cl_image: ethpandaops/prysm-beacon-chain:glamsterdam-devnet-5
    - cl_type: lodestar
      cl_image: ethpandaops/lodestar:glamsterdam-devnet-5
    - cl_type: lighthouse
      cl_image: ethpandaops/lighthouse:glamsterdam-devnet-5

network_params:
  gloas_fork_epoch: 1
  withdrawal_type: "0x01"
  validator_balance: 40000
  genesis_gaslimit: 150000000
  gas_limit: 150000000

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

```

---


## Client BAL features

| EL Client   | Exec Par. | Batch IO (required) | State Par. |
|-------------|:---------:|:--------:|:----------:|
| Geth        | ✅        | ✅       | ✅         |
| Nethermind  | ✅        | ✅       | ✅         |
| Erigon      | ✅        | ✅       | ✅         |
| Besu        | ✅        | ✅       | ✅         |
| Reth        | ✅        | ✅       | ✅         |

Per-client BAL flag tables are unchanged from bal-devnet-6: https://notes.ethereum.org/@ethpandaops/bal-devnet-6#Feature-flags.

---

## Metrics

https://notes.ethereum.org/@ethpandaops/bal-otel

Previous devnet spec sheets for reference:
- CL: https://notes.ethereum.org/@ethpandaops/glamsterdam-devnet-2
- EL: https://notes.ethereum.org/@ethpandaops/bal-devnet-7
