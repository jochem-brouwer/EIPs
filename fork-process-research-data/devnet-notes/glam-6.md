# glamsterdam-devnet-6 spec
:::info
:mega: glamsterdam-devnet-6 targets to launch on the 26th June 2026.
:::

:::info
8282 introduces 2 new system contracts that can be found here:

| Contract | Req | type | Address |
|----------|---------|------|--------------------------------------------|
| Builder | deposit | 0x03 | 0x0000884d2AA32eAa155F59A2f24eFa73D9008282 | 
| Builder | exit | 0x04 | 0x000014574A74c805590AFF9499fc7A690f008282 |
:::

:::info
:checkered_flag: **What's necessary (EL).** devnet-6 adds the post-bal-devnet-7 EL work:
- **New:** EIP-2780 · EIP-8038 (repricing), EIP-7997, EIP-8246, EIP-8070 (optional), EIP-8282
- **Changed:** EIP-7954 → 64 KiB, EIP-8037 → source-based refunds, EIP-7928 (BAL × 7702 warming **still decided keeping status quo**)
:::

## EIP List for glamsterdam-devnet-6

| EIP | Title |Status
|--------|-----|-------|
|[EIP-2780](https://eips.ethereum.org/EIPS/eip-2780) | Reduce intrinsic transaction gas | :new:
|[EIP-7610](https://eips.ethereum.org/EIPS/eip-7610) | Revert creation in case of non-empty storage
|[EIP-7708](https://eips.ethereum.org/EIPS/eip-7708) | ETH transfers emit a log |
|[EIP-7732](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7732.md) | Enshrined Proposer-Builder Separation | :up:
|[EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) | Block Gas Accounting without Refunds |
|[EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) | SLOTNUM opcode |
|[EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) | Block-Level Access Lists | :up: ¹
|[EIP-7954](https://eips.ethereum.org/EIPS/eip-7954) | Increase Maximum Contract Size (**→ 64 KiB**) | :up:
|[EIP-7975](https://eips.ethereum.org/EIPS/eip-7975) | eth/70 - partial block receipt lists |
|[EIP-7976](https://eips.ethereum.org/EIPS/eip-7976) | Increase Calldata Floor Cost (64/64) |
|[EIP-7981](https://eips.ethereum.org/EIPS/eip-7981) | Increase Access List Cost |
|[EIP-7997](https://eips.ethereum.org/EIPS/eip-7997) | Deterministic Factory Predeploy | :new: 
|[EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) | Backward compatible SWAPN, DUPN, EXCHANGE
|[EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) | State Creation Gas — CPSB=1530, **source-based refunds** | :up:
|[EIP-8038](https://eips.ethereum.org/EIPS/eip-8038) | State-access gas cost update | :new:
|[EIP-8045](https://eips.ethereum.org/EIPS/eip-8045) | Exclude slashed validators from proposing |
|[EIP-8061](https://eips.ethereum.org/EIPS/eip-8061) | Increase exit and consolidation churn |
|[EIP-8070](https://eips.ethereum.org/EIPS/eip-8070) | eth/72 - Sparse Blobpool | :new: optional
|[EIP-8159](https://eips.ethereum.org/EIPS/eip-8159) | eth/71 - Block Access List Exchange | 
|[EIP-8246](https://eips.ethereum.org/EIPS/eip-8246) | Remove SELFDESTRUCT Burn | :new:
|[EIP-8282](https://github.com/ethereum/EIPs/pull/11760) | Add builder execution requests | :new:


**Key:**
- :up: EIP has updated!
- :new: New EIP added.
- :exclamation: scope changed (now required)

¹ EIP-7928: spec refinements merged

### Test Releases

**Consensus Specs:** [v1.7.0-alpha.11](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.11)  :heavy_check_mark: 


**Execution Specs (EELS):** [glamsterdam-devnet@v6.1.1](https://github.com/ethereum/execution-specs/releases/tag/tests-glamsterdam-devnet%40v6.1.1) :heavy_check_mark: 

### Spec versions required & Open PRs

**EIPs** (open / under debate)
- [#11645](https://github.com/ethereum/EIPs/pull/11645) EIP-2780 resource-based rework (canonical, supersedes #11735) Merged :heavy_check_mark: 
- [#11760](https://github.com/ethereum/EIPs/pull/11760) EIP-8282 add builder execution requests (Draft) :exclamation: 
- [#11807](https://github.com/ethereum/EIPs/pull/11807) EIP-8037 refund model — source-based Merged :heavy_check_mark: 
- [#11802](https://github.com/ethereum/EIPs/pull/11802) EIP-8038 preliminary gas numbers Merged :heavy_check_mark: 
- [#11783](https://github.com/ethereum/EIPs/pull/11783) EIP-7997 switch to Arachnid factory Merged :heavy_check_mark:
- [#11540](https://github.com/ethereum/EIPs/pull/11540) EIP-7954 raise contract code size to 64 KiB Merged :heavy_check_mark: 
- [#11626](https://github.com/ethereum/EIPs/pull/11626) EIP-8246 remove SELFDESTRUCT burn (→ Review) Merged :heavy_check_mark: 
- [#11444](https://github.com/ethereum/EIPs/pull/11444) EIP-8070 sparse blobpool update Merged :heavy_check_mark: 
- [#11817](https://github.com/ethereum/EIPs/pull/11817) EIP-8007 repricing summary for 2780/7904/8037/8038 Merged :heavy_check_mark: 
- [#11718](https://github.com/ethereum/EIPs/pull/11718) [#11717](https://github.com/ethereum/EIPs/pull/11717) [#11750](https://github.com/ethereum/EIPs/pull/11750) [#11699](https://github.com/ethereum/EIPs/pull/11699) EIP-7928 clarifications (EXTCODECOPY GAS_COPY, CREATE inclusion, no-op storage, 7702 edge case) Merged :heavy_check_mark: 
- [#11823](https://github.com/ethereum/EIPs/pull/11823) reverse SSTORE clear refund to prevent net-profitable round trips :new: :heavy_check_mark:
- [#11834](https://github.com/ethereum/EIPs/pull/11834) EIP-2780: clarify OOG handling for top-level gas charges :new: :heavy_check_mark:

**Builder Specs**

**Beacon API**
- [#580 - Produce block v4 with payload](https://github.com/ethereum/beacon-APIs/pull/580) Open :exclamation: 
- [#608 - gloas proposer preference apis](https://github.com/ethereum/beacon-APIs/pull/608) Open :exclamation: 

**Consensus Specs**

- [#5335 - Clarify `should_build_on_full` semantic](https://github.com/ethereum/consensus-specs/pull/5335) Merged :heavy_check_mark:
- [#5336 - Clarify `should_extend_payload`'s note and add a slot assert](https://github.com/ethereum/consensus-specs/pull/5336) Merged :heavy_check_mark:
- [#5348 - Modify `get_proposer_head` for Gloas](https://github.com/ethereum/consensus-specs/pull/5348) Merged :heavy_check_mark:
- [#5359 - Add builder execution requests (EIP-8282)](https://github.com/ethereum/consensus-specs/pull/5359) Merged :heavy_check_mark:
- [#5360 - Reject bids with invalid `prev_randao` during gossip validation](https://github.com/ethereum/consensus-specs/pull/5360) Merged :heavy_check_mark:
- [#5364 - Pass only signed bid to `process_execution_payload_bid`](https://github.com/ethereum/consensus-specs/pull/5364) Merged :heavy_check_mark:
- [#5365 - Only clear builder payment if the slashed validator is the proposer](https://github.com/ethereum/consensus-specs/pull/5365) Merged :heavy_check_mark:
- [#5373 - Reset withdrawable epoch when depositing to an exited builder](https://github.com/ethereum/consensus-specs/pull/5373) Merged :heavy_check_mark: 
- [#5377 - Introduce `PAYLOAD_BUILDER_VERSION`](https://github.com/ethereum/consensus-specs/pull/5377) Merged :heavy_check_mark: 

**Execution APIs**

**Networking**
- [#264 - eth/71 Block Access List exchange](https://github.com/ethereum/devp2p/pull/264) Merged :heavy_check_mark:

**Execution Specs**
- [#3017 - EIP-2780 implement resource-based intrinsic gas](https://github.com/ethereum/execution-specs/pull/3017) Merged :heavy_check_mark:
- [#2999 - EIP-8037 source-based refunds](https://github.com/ethereum/execution-specs/pull/2999) Merged :heavy_check_mark:
- [#2901 - EIP-8037 merge to `forks/amsterdam`](https://github.com/ethereum/execution-specs/pull/2901) Merged :heavy_check_mark:
- [#2987 - EIP-7954 raise max code size to 64 KiB](https://github.com/ethereum/execution-specs/pull/2987) Merged :heavy_check_mark:
- [#2972 - EIP-8038 state-access gas cost update](https://github.com/ethereum/execution-specs/pull/2972) Merged :heavy_check_mark:
- [#2842 - EIP-8246 remove SELFDESTRUCT burn](https://github.com/ethereum/execution-specs/pull/2842) Merged :heavy_check_mark:
- [#2980](https://github.com/ethereum/execution-specs/pull/2980) [#2985](https://github.com/ethereum/execution-specs/pull/2985) [#2967](https://github.com/ethereum/execution-specs/pull/2967) EIP-7928 test/spec clarifications Merged :heavy_check_mark:
- [#2948 - EIP-8070 sparse blobpool](https://github.com/ethereum/execution-specs/pull/2948) Open :exclamation:
- [#2900](https://github.com/ethereum/execution-specs/pull/2900) EIP-7702 delegate-warming (ACDE #239): defer warming + BAL-add of target/delegate until depth/balance checks pass; 3 proposals, clients leaning Option 1 (status quo) over this PR (Option 3) *KEEPING STATUS QUO* :exclamation:
- [#2990 - feat(spec,tests): Implement EIP-8282](https://github.com/ethereum/execution-specs/pull/2990) Open :exclamation:


**Other**


## Per-EIP EL changes (with merged PRs)

New EIPs:

| EIP | Change | EIPs PR | EELS PR |
|-----|--------|---------|---------|
| EIP-2780 | Reduce intrinsic tx gas, reworked resource-based | [#11645](https://github.com/ethereum/EIPs/pull/11645) **merged** | [#3017](https://github.com/ethereum/execution-specs/pull/3017) **merged** |
| EIP-7997 | Deterministic CREATE2 factory predeploy, switched to Arachnid | [#11783](https://github.com/ethereum/EIPs/pull/11783) **merged** | execute support [#2944](https://github.com/ethereum/execution-specs/pull/2944) **merged** |
| EIP-8038 | State-access repricing: STORAGE_WRITE / COLD_STORAGE_ACCESS / COLD_ACCOUNT_ACCESS / EXTCODESIZE / EXTCODECOPY; `requires` 7904+8037 | [#11802](https://github.com/ethereum/EIPs/pull/11802), [#11696](https://github.com/ethereum/EIPs/pull/11696) **merged** | [#2972](https://github.com/ethereum/execution-specs/pull/2972) **merged**, fixtures [#3019](https://github.com/ethereum/execution-specs/pull/3019) |
| EIP-8070 | eth/72 Sparse Blobpool (optional) | [#11444](https://github.com/ethereum/EIPs/pull/11444) **merged** | [#2948](https://github.com/ethereum/execution-specs/pull/2948) **open** |
| EIP-8246 | Remove SELFDESTRUCT burn | [#11626](https://github.com/ethereum/EIPs/pull/11626) (→ Review) | [#2842](https://github.com/ethereum/execution-specs/pull/2842) **merged** |
| EIP-8282 | Builder Execution Requests (ePBS); 2 system contracts (Builder deposit `0x03`, exit `0x04`) | [#11760](https://github.com/ethereum/EIPs/pull/11760) **open** | [#2990](https://github.com/ethereum/execution-specs/pull/2990) **draft** |

Changed EIPs:

| EIP | Change | EIPs PR | EELS PR |
|-----|--------|---------|---------|
| EIP-7954 | Max contract size → **64 KiB** | [#11540](https://github.com/ethereum/EIPs/pull/11540) **merged** | [#2987](https://github.com/ethereum/execution-specs/pull/2987) **merged**; jumpdest tests [#2993](https://github.com/ethereum/execution-specs/pull/2993), [#2998](https://github.com/ethereum/execution-specs/pull/2998) |
| EIP-8037 | Refund model → **source-based** (resolve 7928 conflict / route refills to origin); 7702 auth gas accounting; calldata-floor alignment; merged to `forks/amsterdam` | [#11807](https://github.com/ethereum/EIPs/pull/11807), [#11759](https://github.com/ethereum/EIPs/pull/11759), [#11715](https://github.com/ethereum/EIPs/pull/11715), [#11706](https://github.com/ethereum/EIPs/pull/11706) **merged** | [#2999](https://github.com/ethereum/execution-specs/pull/2999), [#2901](https://github.com/ethereum/execution-specs/pull/2901) (→`forks/amsterdam`), [#2892](https://github.com/ethereum/execution-specs/pull/2892), [#2956](https://github.com/ethereum/execution-specs/pull/2956), [#2978](https://github.com/ethereum/execution-specs/pull/2978), [#3002](https://github.com/ethereum/execution-specs/pull/3002), [#3011](https://github.com/ethereum/execution-specs/pull/3011) **merged** |
| EIP-7928 | GAS_COPY in EXTCODECOPY; CREATE inclusion + storage no-op clarifications; disallow empty change set; 7702 insufficient-gas edge case. Open: 7702 delegate warming | [#11718](https://github.com/ethereum/EIPs/pull/11718), [#11717](https://github.com/ethereum/EIPs/pull/11717), [#11750](https://github.com/ethereum/EIPs/pull/11750), [#11699](https://github.com/ethereum/EIPs/pull/11699) **merged** | [#2980](https://github.com/ethereum/execution-specs/pull/2980), [#2985](https://github.com/ethereum/execution-specs/pull/2985), [#2967](https://github.com/ethereum/execution-specs/pull/2967), [#2945](https://github.com/ethereum/execution-specs/pull/2945) **merged**; warming [#2900](https://github.com/ethereum/execution-specs/pull/2900) **open** |



## Local testing

Kurtosis example:
```yaml=
participants_matrix:
  el:
    - el_type: ethrex
      el_image: ethpandaops/ethrex:glamsterdam-devnet-6
  cl:
    - cl_type: lodestar
      cl_image: ethpandaops/lodestar:glamsterdam-devnet-6
    - cl_type: teku
      cl_image: ethpandaops/teku:glamsterdam-devnet-6

network_params:
  gloas_fork_epoch: 1
  withdrawal_type: "0x01"
  validator_balance: 40000
  genesis_gaslimit: 150000000
  gas_limit: 150000000

additional_services:
   - assertoor
   - spamoor
   - dora

assertoor_params:
  run_stability_check: false
  run_block_proposal_check: false
  tests:
    - { file: "https://raw.githubusercontent.com/ethpandaops/assertoor/refs/heads/master/playbooks/gloas-dev/builder-lifecycle.yaml" }

dora_params:
  image: ethpandaops/dora:glamsterdam-devnet-6
port_publisher:
  additional_services:
    enabled: true



global_log_level: debug
```

---

## Metrics

https://notes.ethereum.org/@ethpandaops/bal-otel

Previous devnet spec sheets for reference: https://notes.ethereum.org/@ethpandaops/glamsterdam-devnet-5
