# glamsterdam-devnet-7 spec
:::info
:mega: glamsterdam-devnet-7 targets to launch on the middle of July 2026.
:::

:::info
8282 introduces 2 new system contracts that can be found here:

| Contract | Req | type | Address | 
|----------|---------|------|--------------------------------------------|
| :exclamation: NEW | deposit | 0x03 | 0x0000bff46984e3725691fa540a8c7589300d8282 | 
| same as devnet-6 | exit | 0x04 | 0x000064d678505ad48f8ccb093bc65613800e8282 |
:::

## EIP List for glamsterdam-devnet-7

| EIP | Title |Status
|--------|-----|:-----:|
|[EIP-2780](https://eips.ethereum.org/EIPS/eip-2780) | Reduce intrinsic transaction gas |
|[EIP-7610](https://eips.ethereum.org/EIPS/eip-7610) | Revert creation in case of non-empty storage
|[EIP-7688](https://eips.ethereum.org/EIPS/eip-7688) | Forward compatible consensus data structures | :new:
|[EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log |
|[EIP-7732](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7732.md) | Enshrined Proposer-Builder Separation |
|[EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds |
|[EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode |
| [EIP-7904](https://eips.ethereum.org/EIPS/eip-7904) | Compute Gas Cost Analysis (informational only) | ℹ️ |
|[EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists | 
|[EIP-7954](https://eips.ethereum.org/EIPS/eip-7954) | Increase Maximum Contract Size (**→ 64 KiB**) | 
|[EIP-7975](https://eips.ethereum.org/EIPS/eip-7975) | eth/70 - partial block receipt lists |
|[EIP-7976](https://eips.ethereum.org/EIPS/eip-7976) | Increase Calldata Floor Cost (64/64) |
|[EIP-7981](https://eips.ethereum.org/EIPS/eip-7981) | Increase Access List Cost |
|[EIP-7997](https://eips.ethereum.org/EIPS/eip-7997) | Deterministic Factory Predeploy | 
|[EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE
|[EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) | State Creation Gas — CPSB=1530, **source-based refunds** |
|[EIP-8038](https://eips.ethereum.org/EIPS/eip-8038) | State-access gas cost update | 
|[EIP-8045](https://eips.ethereum.org/EIPS/eip-8045) | Exclude slashed validators from proposing |
|[EIP-8061](https://eips.ethereum.org/EIPS/eip-8061) | Increase exit and consolidation churn |
|[EIP-8070](https://eips.ethereum.org/EIPS/eip-8070) | eth/72 - Sparse Blobpool | :sparkles: 
|[EIP-8159](https://eips.ethereum.org/EIPS/eip-8159) | eth/71 - Block Access List Exchange | 
|[EIP-8246](https://eips.ethereum.org/EIPS/eip-8246) | Remove SELFDESTRUCT Burn | 
|[EIP-8282](https://github.com/ethereum/EIPs/pull/11760) | Add builder execution requests |

**Key:**
- :up: EIP has updated
- :new: New EIP added
- :exclamation: scope changed (now required)
- :sparkles: Optional (nice to have)

### Specs & Tests

**Consensus Layer**

- [x] [v1.7.0-alpha.12](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.12)

**Execution Layer**

- [x] [glamsterdam-devnet@v7.2.0](https://github.com/ethereum/execution-specs/releases/tag/tests-glamsterdam-devnet%40v7.2.0)

### Open PRs

**EIPs** (open / under debate)

- [ ] [PR-11902 - exclude the recipient when a runtime halt precedes](https://github.com/ethereum/EIPs/pull/11902)
- [x] [PR-11844 - move state-dependent charges to runtime](https://github.com/ethereum/EIPs/pull/11844)
- [x] [PR-11854 - Update EIP-7928: SSTORE access cost check](https://github.com/ethereum/EIPs/pull/11854)
- [x] [PR-11891 - clarify 2780](https://github.com/ethereum/EIPs/pull/11891)
- [x] [PR-EIP-2780: align with recent EIP-2780 changes](https://github.com/ethereum/EIPs/pull/11906)
- [x] [PR-11908 apply the EIP-7976 calldata floor to block-level gas accounting](https://github.com/ethereum/EIPs/pull/11908)

**Builder Specs**

**Beacon API**

- [x] [PR-580 - Produce block v4 with payload](https://github.com/ethereum/beacon-APIs/pull/580)
- [ ] [PR-606 - feat: extend /eth/v1/node/peers with peer scoring and disconnect reasons](https://github.com/ethereum/beacon-APIs/pull/606)
- [x] [PR-608 - gloas proposer preference apis](https://github.com/ethereum/beacon-APIs/pull/608)
- [x] [PR-621 - Simplify payload attributes event](https://github.com/ethereum/beacon-APIs/pull/621)

**Consensus Specs**

- [x] [PR-4630 - Forward compatible consensus data structures (EIP-7688)](https://github.com/ethereum/consensus-specs/pull/4630)
- [x] [PR-5355 - Require imported payload for `index == 1` attestation gossip](https://github.com/ethereum/consensus-specs/pull/5355)
- [x] [PR-5374 - Replace `get_dependent_root` with `get_shuffling_dependent_root`](https://github.com/ethereum/consensus-specs/pull/5374)
- [x] [PR-5383 - Use `MAX_REQUEST_PAYLOADS` in `EnvelopesByRange`](https://github.com/ethereum/consensus-specs/pull/5383)
- [x] [PR-5384 - Only reset builder withdrawal epoch if its balance has been swept](https://github.com/ethereum/consensus-specs/pull/5384)
- [x] [PR-5414 - Set payload deadline to 6 seconds into the slot](https://github.com/ethereum/consensus-specs/pull/5414)
- [x] [PR-5416 - Set `BUILDER_WITHDRAWAL_PREFIX` to 0xB0](https://github.com/ethereum/consensus-specs/pull/5416)
- [x] [PR-5420 - Reduce `MAX_BUILDER_DEPOSIT_REQUESTS_PER_PAYLOAD` to 64](https://github.com/ethereum/consensus-specs/pull/5420)
- [x] [PR-5426 - Reduce `MIN_BUILDER_WITHDRAWABILITY_DELAY` to 64 epochs](https://github.com/ethereum/consensus-specs/pull/5426)
- [x] [PR-5429 - Ignore instead of reject on preferences](https://github.com/ethereum/consensus-specs/pull/5429)
- [x] [PR-5436 - Remove `MAX_DEPOSIT_REQUESTS_PER_PAYLOAD` in Gloas](https://github.com/ethereum/consensus-specs/pull/5436)
- [x] [PR-5439 - Restrict builder deposits to payload builders](https://github.com/ethereum/consensus-specs/pull/5439)

**Execution APIs**

**Networking**

**Execution Specs**

- [x] [PR-3064 move SSTORE access-cost check before read](https://github.com/ethereum/execution-specs/pull/3064)

**System Contracts**

- [x] [PR-49 - builder_deposits: add logic to disable contract through system call](https://github.com/ethereum/sys-asm/pull/49)
- [x] [PR-50 - builder_deposits: reduce TARGET_PER_BLOCK, MAX_PER_BLOCK](https://github.com/ethereum/sys-asm/pull/50)

## Local testing

Kurtosis example: 

```
participants_matrix:
  el:
    - el_type: geth
      el_image: ethpandaops/geth:glamsterdam-devnet-7
    - el_type: ethrex
      el_image: ethpandaops/ethrex:glamsterdam-devnet-7
    - el_type: nethermind
      el_image: ethpandaops/nethermind:glamsterdam-devnet-7
  cl:
    - cl_type: lighthouse
      cl_image: ethpandaops/lighthouse:glamsterdam-devnet-7
    - cl_type: lodestar
      cl_image: ethpandaops/lodestar:glamsterdam-devnet-7
    - cl_type: prysm
      cl_image: ethpandaops/prysm-beacon-chain:glamsterdam-devnet-7-minimal

network_params:
  preset: minimal
  gloas_fork_epoch: 1
  # (deploy_eip8282_contracts: true) at the new addresses:
  #   deposit: 0x0000bFF46984e3725691FA540a8C7589300D8282
  #   exit:    0x000064D678505ad48F8cCb093BC65613800E8282

additional_services:
  - assertoor
  - spamoor
  - dora

assertoor_params:
  run_stability_check: false
  run_block_proposal_check: false
  tests:
    - { file: "https://raw.githubusercontent.com/ethpandaops/assertoor/refs/heads/master/playbooks/gloas-dev/builder-lifecycle.yaml" }
    - { file: "https://raw.githubusercontent.com/ethpandaops/assertoor/refs/heads/master/playbooks/gloas-dev/builder-deposit.yaml" }

dora_params:
  image: ethpandaops/dora:master-latest

port_publisher:
  additional_services:
    enabled: true

global_log_level: debug
```

---

## Metrics

https://notes.ethereum.org/@ethpandaops/bal-otel

Previous devnet spec sheets for reference: https://notes.ethereum.org/@ethpandaops/glamsterdam-devnet-6
