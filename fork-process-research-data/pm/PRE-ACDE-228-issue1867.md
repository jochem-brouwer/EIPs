# ISSUE 1867: All Core Devs - Execution (ACDE) #228, Jan 15, 2026

### UTC Date & Time

[January 15, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jan-15-2026/2pm)

### Agenda

- Glamsterdam
  - devnets
    - devnet-2 scope
  - EIP clarifications
    - EIP-7778: Block Gas Accounting without Refunds
      - [Clarifications on `CumulativeGasUsed`, `gasUsedForPaying`, & `gasUsedForBlockLimit`](https://github.com/ethereum/pm/issues/1867#issuecomment-3750342394)
    - EIP-8024: Backward compatible SWAPN, DUPN, EXCHANGE
      - [Clarification on behavior when code ends before immediate operand](https://github.com/ethereum/pm/issues/1867#issuecomment-3750375521)
      - [Proposal for simpler version](https://github.com/ethereum/pm/issues/1867#issuecomment-3753841169)
    - EIP-7708: ETH transfers emit a log
      - [pending PR](https://github.com/ethereum/EIPs/pull/9003) with [open questions](https://github.com/ethereum/pm/issues/1867#issuecomment-3754028415)
  - Remaining Glamsterdam PFI decisions
    - [EL PFI'd EIPs status doc](https://notes.ethereum.org/@ansgar/glamsterdam-el-pfi-eips)
      - postponed last ACDE
        - EIP-8037: State Creation Gas Cost Increase
          - @MariusVanDerWijden will explain the recommended approach: [slides here](https://docs.google.com/presentation/d/1zRVR-tziS0Z0IACiDlcOPOUmkPdaBvjE-b10yH35iOY/edit?usp=sharing)
      - no time last ACDE
        - EIP-7793: Conditional Transactions
        - EIP-5920: PAY opcode
        - EIP-8051: Precompile for ML-DSA signature verification
      - delayed decision last ACDE
        - EIP-7971: Hard Limits for Transient Storage
        - EIP-8032: Size-Based Storage Gas Pricing
        - EIP-7907: Meter Contract Code Size And Increase Limit
          - [benchmarks & analysis](https://ethresear.ch/t/data-driven-analysis-on-eip-7907/23850) by @CPerezz 
        - EIP-7903: Remove Initcode Size Limit
      - EIPs without protocol changes
        - EIP-7610: Revert creation in case of non-empty storage
        - EIP-7872: Max blob flag for local builders
        - EIP-7949: Genesis File Format
- Misc
  - [Request for client review on execution API items](https://github.com/ethereum/pm/issues/1867#issuecomment-3742257592)

### Call Series

All Core Devs - Execution


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

90 minutes

### Occurrence Rate

bi-weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Facilitator Emails (Optional)

_No response_

### Display Zoom Link in Calendar Invite (Optional)

- [ ] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>


===== COMMENTS =====

--- github-actions[bot] 2026-01-07T21:24:15Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27400)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=SMC83TdqgLY)

--- abcoathup 2026-01-08T01:40:46Z ---
## Glamsterdam scoping

⚠️ Glamsterdam would have the most EIPs ever if all CFI'd EIPs are SFI'd into devnets.

@adietrichs's [categorized EIP shortlist](https://notes.ethereum.org/@ansgar/glamsterdam-el-pfi-eips)

11 [Proposed for Inclusion](https://forkcast.org/upgrade/glamsterdam/#proposed-for-inclusion) EIPs remain to be decided.

| Upgrade                                                                              | Headliners | Core EIPs | Other EIPs | mainnet |
|----------------------------------------------------------------|-------------|------------|------------|-----------|
| [Dencun](https://eips.ethereum.org/EIPS/eip-7569)          | -                 | 9               | 0             | March 13, 2024 |
| [Pectra](https://eips.ethereum.org/EIPS/eip-7600)             | -                | 10              | 2              | May 07, 2025 |
| [Fusaka](https://eips.ethereum.org/EIPS/eip-7607)            | 1                | 8               | 4              | December 3, 2025 |
| [Glamsterdam](https://eips.ethereum.org/EIPS/eip-7773) | 2                | *(15 [CFI'd](https://forkcast.org/upgrade/glamsterdam#considered-for-inclusion) so far)*    | 0             | ? 2026 * |

* If we wanted Glamsterdam upgrade by June, then testnet release(s) would likely be required by [mid April](https://github.com/ethereum/pm/issues/1808#issuecomment-3544214756).  (Also see: [forkcast.org/schedule](https://forkcast.org/schedule/))
* How will ACD decide which CFI'd EIPs are SFI'd? (@nixorokish ?)

--- akashkshirsagar31 2026-01-12T08:07:30Z ---
X Stream: https://x.com/i/broadcasts/1lDGLBpELZRxm

--- bomanaps 2026-01-13T06:37:10Z ---
#### Agenda Addition – Execution API Topics

Following up on our last call, I’d like to raise a few execution-API–related issues for broader ACDE input:

1. EIP-3155 (Call Tracer Standardization)
There is ongoing work and discussion around standardizing the call tracer output format via EIP-3155. This EIP was proposed over five years ago, and we’d like to gather renewed client feedback and build momentum toward alignment and adoption.
https://eips.ethereum.org/EIPS/eip-3155 https://github.com/ethereum/execution-apis/pull/728

2. Execution APIs – Issue #729
https://github.com/ethereum/execution-apis/issues/729
Encourage clients to comment on this issue to express support, concerns, or implementation constraints, so we can better understand client positions and determine next steps.

--- yperbasis 2026-01-14T16:13:58Z ---
As [discussed on Discord](https://discord.com/channels/595666850260713488/1460715235437580369), I think that [EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) requires a couple of changes/clarifications:

- `CumulativeGasUsed` in receipts should remain consistent with block gas accounting, not gas used for paying
- Clarify that `gasUsedForPaying = max(floorGas7623, gasUsed-refund)` and `gasUsedFoBlockLimit = max(floorGas7623, gasUsed)` 

--- yperbasis 2026-01-14T16:21:26Z ---
As [discussed on Discord](https://discord.com/channels/595666850260713488/1461001696183455930), [EIP-8024](https://eips.ethereum.org/EIPS/eip-8024) needs to clarify what happens when the code ends before an immediate operand. That can either lead to an execution failure or the value of the immediate operand can be postulated to be 0 in such case similar to `PUSH1`.

--- qu0b 2026-01-15T09:28:47Z ---
Clarification if we should include [EIP-7843](https://eips.ethereum.org/EIPS/eip-7843) in `bal-devnet-2` alongside the other EIPS: [8024, 7778 & 7708](https://notes.ethereum.org/@ethpandaops/bal-devnet-2) or if we should drop it as it requires CL changes.

--- rakita 2026-01-15T09:52:36Z ---
```
Not sure if this was talked about. We prefer EIP-8024 (SWAPN, DUPN, EXCHANGE) to be simpler and use previous version of SWAPN PUSH2 0x0000 format as it is more straightforward on both jumpdest analysis and impl side.

Was this discussed in ACDT?

Not a blocker, EIP-8024 is important to add and any version of EIP would work for us!
```
Have raised this in [discord](https://discord.com/channels/595666850260713488/745077610685661265/1461005687952769183) as people are not responding async and would like to discuss it on the call.

--- spencer-tb 2026-01-15T10:20:52Z ---
Could this EIP-7708 PR (https://github.com/ethereum/EIPs/pull/9003) get some more love - so we can finalize its spec for devnet-2? cc @etan-status
- Agreement on the log address & topic[0] value.
- Do we want to include fee payment logs on top of eth transfer logs.

--- CPerezz 2026-01-15T10:50:25Z ---
Maybe worth giving a TLDR on my report on 7907 prior to consider inclusion? 
See: https://ethresear.ch/t/data-driven-analysis-on-eip-7907

--- nixorokish 2026-01-19T20:51:57Z ---
closing in lieu of #1883 
