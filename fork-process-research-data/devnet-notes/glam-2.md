# glamsterdam-devnet-2 spec
:::info
:mega: glamsterdam-devnet-2 targets to launch on the 30th of April 2026.
:::




## EIP List for glamsterdam-devnet-2

| EIP | Title |Status
|--------|-----|-------|
|[EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log |
|[EIP-7732](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7732.md) | Enshrined Proposer-Builder Separation | :up:
|[EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds |
|[EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode |
|[EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists | :up:
|[EIP-7954](https://eips.ethereum.org/EIPS/eip-7954) | Increase Maximum Contract Size |
|[EIP-7975](https://eips.ethereum.org/EIPS/eip-7975) | eth/70 - partial block receipt lists | optional
|[EIP-7976](https://eips.ethereum.org/EIPS/eip-7976) | Increase Calldata Floor Cost (64/64) |
|[EIP-7981](https://eips.ethereum.org/EIPS/eip-7981) | Increase Access List Cost |
|[EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE
|[EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) | State Creation Gas Cost Increase | :up:
|[EIP-8061](https://eips.ethereum.org/EIPS/eip-8061) | Increase exit and consolidation churn | :new:
|[EIP-8159](https://eips.ethereum.org/EIPS/eip-8159) | eth/71 - Block Access List Exchange | optional



**Key:**
- :up: EIP has updated!
- :new: New EIP added.


### Implementation tracker EL

## Execution Layer Client Support
Here you go:



### Test Releases

**Consensus Specs:** [v1.7.0-alpha.7](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.7)  :heavy_check_mark: 


**Execution Specs:** [bal@v5.6.1](https://github.com/ethereum/execution-spec-tests/releases/tag/bal@v5.6.1) or [bal@v5.7.0](https://github.com/ethereum/execution-spec-tests/releases/tag/bal@v5.7.0) pending release :exclamation: pending devnet3 or 4 scoping


### Spec versions required & Open PRs

**EIPs**
- [PR-11476 - Update EIP-8037: Refund state gas on all frame failures including top level](https://github.com/ethereum/EIPs/pull/11476) Merged :heavy_check_mark:

**Builder Specs**
- [PR-138 - Staked Builder API for Glamsterdam](https://github.com/ethereum/builder-specs/pull/138) Open :exclamation:

**Consensus Specs** (new consensus-breaking PRs since `v1.7.0-alpha.5`)
- [PR-4769 - Set `blob_data_available` in `PayloadAttestationMessage`](https://github.com/ethereum/consensus-specs/pull/4769) Merged :heavy_check_mark:
- [PR-5061 - Increase exit and consolidation churn (EIP-8061)](https://github.com/ethereum/consensus-specs/pull/5061) Merged :heavy_check_mark:
- [PR-5135 - Remove incorrect anchor seed for payload votes](https://github.com/ethereum/consensus-specs/pull/5135) Merged :heavy_check_mark:
- [PR-5152 - Add `parent_beacon_block_root` to `ExecutionPayloadEnvelope`](https://github.com/ethereum/consensus-specs/pull/5152) Merged :heavy_check_mark:
- [PR-5164 - Change proposer preference validator index check to ignore](https://github.com/ethereum/consensus-specs/pull/5164) Merged :heavy_check_mark:
- [PR-5172 - Fix genesis state in Gloas](https://github.com/ethereum/consensus-specs/pull/5172) Merged :heavy_check_mark:
- [PR-5177 - Change minimal `PTC_SIZE` to 16 validators](https://github.com/ethereum/consensus-specs/pull/5177) Merged :heavy_check_mark:
- [PR-5190 - Add checkpoint root to proposer preferences](https://github.com/ethereum/consensus-specs/pull/5190) Merged :heavy_check_mark:
- [PR-5191 - Fix slot check in proposer preferences gossip](https://github.com/ethereum/consensus-specs/pull/5191) Merged :heavy_check_mark:
- [PR-5196 - Use dependent root for proposer preferences](https://github.com/ethereum/consensus-specs/pull/5196) Merged :heavy_check_mark:


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
    - el_type: geth
      el_image: ethpandaops/geth:bal-devnet-6
  cl:
    - cl_type: prysm
      cl_image: ethpandaops/prysm-beacon-chain:glamsterdam-devnet-3-minimal
    - cl_type: lodestar
      cl_image: ethpandaops/lodestar:glamsterdam-devnet-2
    - cl_type: lighthouse
      cl_image: ethpandaops/lighthouse:glamsterdam-devnet-3
    - cl_type: teku
      cl_image: ethpandaops/teku:glamsterdam-devnet-2
    - cl_type: nimbus
      cl_image: ethpandaops/nimbus-eth2:glamsterdam-devnet-2-minimal
    - cl_type: grandine
      cl_image: ethpandaops/grandine:glamsterdam-devnet-2-minimal
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

```

---

Previous devnet spec can be found on [glamsterdam-devnet-0](https://notes.ethereum.org/@ethpandaops/glamsterdam-devnet-0)