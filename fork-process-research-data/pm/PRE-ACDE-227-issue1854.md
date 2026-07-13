# ISSUE 1854: All Core Devs - Execution (ACDE) #227, Jan 5, 2026 [OFF-CYCLE]

### UTC Date & Time

[January 05, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jan-5-2026/2pm)

### Agenda

- Housekeeping
  - [ACD process updates](https://github.com/ethereum/pm/issues/1854#issuecomment-3708564165) by @poojaranjan
- Glamsterdam
  - scoping decisions
    - [updated CFI / DFI candidate EIPs](https://notes.ethereum.org/@ansgar/glamsterdam-el-pfi-eips)
    - [comment from last ACDE regarding EIP-8051 and related EIPs](https://github.com/ethereum/pm/issues/1837#issuecomment-3655914307) by @rdubois-crypto 
- Other
  - [EIP-8077](https://eips.ethereum.org/EIPS/eip-8077) and [EIP-8094](https://eips.ethereum.org/EIPS/eip-8094) by @cskiraly

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

--- github-actions[bot] 2025-12-29T19:25:08Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27356)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=1B03r5t03bU)

--- abcoathup 2025-12-29T23:01:09Z ---
## Glamsterdam scoping

⚠️ Glamsterdam would have the most EIPs ever if all CFI'd EIPs are SFI'd into devnets.

| Upgrade                                                                              | Headliners | Core EIPs | Other EIPs | mainnet |
|----------------------------------------------------------------|-------------|------------|------------|-----------|
| [Dencun](https://eips.ethereum.org/EIPS/eip-7569)          | -                 | 9               | 0             | March 13, 2024 |
| [Pectra](https://eips.ethereum.org/EIPS/eip-7600)             | -                | 10              | 2              | May 07, 2025 |
| [Fusaka](https://eips.ethereum.org/EIPS/eip-7607)            | 1                | 8               | 4              | December 3, 2025 |
| [Glamsterdam](https://eips.ethereum.org/EIPS/eip-7773) | 2                | *(14 [CFI'd](https://forkcast.org/upgrade/glamsterdam#considered-for-inclusion) so far)*    | 0             | ? 2026 * |

* If we wanted Glamsterdam upgrade by June, then testnet release(s) would likely be required by [mid April](https://github.com/ethereum/pm/issues/1808#issuecomment-3544214756).  (Also see: [forkcast.org/schedule](https://forkcast.org/schedule/))

--- cskiraly 2026-01-02T11:28:15Z ---
Since time was running out in the last ACDE call before I was able to introduce the following two EL networking EIPs, I would like to quickly introduce them this time. They are not fork-specific (hence I wasn't proposing them on the Glamsterdam list) but provide mempool improvements that would be useful for scaling.

- [EIP-8077](https://eips.ethereum.org/EIPS/eip-8077) proposes to change the way we propagate transactions in the mempool, improving efficiency and preparing for a future where not everyone has the capacity to receive all transactions.
[EIP-8094](https://eips.ethereum.org/EIPS/eip-8094) aims to make RBF (replace by fee) much more efficient, eliminating the cost of redistributing blob content if only the metadata (fees) of a transaction are updated.

--- poojaranjan 2026-01-05T00:13:18Z ---
Happy New Year, everyone!

If time permits, I'd like to bring up 
- [Introduce ACD-G (All Core Devs – Governance)](https://ethereum-magicians.org/t/introduce-acd-g-all-core-devs-governance/27387) 
- [Add Optional “Upgrade” Field to Standards Track Core EIPs](https://ethereum-magicians.org/t/add-optional-upgrade-field-to-standards-track-core-eips/27388)
- [Best place to host "Call for Input"](https://github.com/ethcatherders/EIPIP/issues/397)
- Update EIP-7723 for clarity, ref [PR](https://github.com/ethereum/EIPs/pull/11006)

--- akashkshirsagar31 2026-01-05T01:35:25Z ---
X Stream: https://x.com/i/broadcasts/1ZkJzZmPozvJv

--- nixorokish 2026-01-07T21:23:47Z ---
closed in lieu of #1867 
