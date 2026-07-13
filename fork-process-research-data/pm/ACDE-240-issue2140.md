# ISSUE 2140: All Core Devs - Execution (ACDE) #240, July 2, 2026
Created: 2026-06-25T14:09:31Z by nixorokish

### UTC Date & Time

[July 02, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jul-2-2026/2pm)

Today's facilitator: @adietrichs 

### Agenda

- Glamsterdam
  - devnet-7 spec
    - various updates needed, see [comment](https://github.com/ethereum/pm/issues/2140#issuecomment-4864378158) by @qu0b 
    - new deposit contract, see [comment](https://github.com/ethereum/pm/issues/2140#issuecomment-4865260387) by @barnabasbusa 
    - other open questions
  - mascot needed, see [comment](https://github.com/ethereum/pm/issues/2140#issuecomment-4805184883) by @abcoathup
- Hegotá
  - setting the EIP proposal deadline
  - proposed EIPs
    - [EIP-8250](https://eips.ethereum.org/EIPS/eip-8250) and [EIP-8272](https://eips.ethereum.org/EIPS/eip-8272), see [comment](https://github.com/ethereum/pm/issues/2140#issuecomment-4808863293) by @soispoke 
    - [EIP-7862](https://eips.ethereum.org/EIPS/eip-7862) and [EIP-8146](https://eips.ethereum.org/EIPS/eip-8146), see [comment](https://github.com/ethereum/pm/issues/2140#issuecomment-4824952022) by @nerolation 
    - [EIP-8298](https://eips.ethereum.org/EIPS/eip-8298) by @benaadams 
- Misc
  - [housekeeping items](https://github.com/ethereum/pm/issues/2140#issuecomment-4866293221) by @poojaranjan 

### Call Series

All Core Devs - Execution

### Autopilot Mode

- [x] Use autopilot (recommended defaults for this call series)


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

90 minutes

### Occurrence Rate

bi-weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Display Zoom Link in Calendar Invite (Optional)

- [x] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2026-06-25T14:10:32Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260702%2F20260703&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Execution+%28ACDE%29+%23240%2C+July+2%2C+2026&dates=20260702T140000Z%2F20260702T153000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F85451723466%3Fpwd%3DRgAVD0sO4OPFAU3UIqPflda1z05cRT.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2140)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28862)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=HT96XUWOVMs)

--- COMMENT by abcoathup at 2026-06-25T23:56:37Z ---
**Mascot needed for Glamsterdam upgrade**
🐜🦫🐝🦩🐹🐙🐻‍❄️🐩
Vote for your favorite on Eth Magicians.  **Voting closes July 8**

Top 3 currently: 🦩 36%, 🐻‍❄️ 35% & 🦫 19%

https://ethereum-magicians.org/t/mascot-needed-for-glamsterdam-upgrade/26008

[EIP-8066: Upgrade Mascots](https://eips.ethereum.org/EIPS/eip-8066) specifies mascot requirements, role of Mascot Wrestler (@eviljordan) & veto powers of the Mascot Wrestler & client teams.

--- COMMENT by soispoke at 2026-06-26T10:54:10Z ---
Would like to propose [EIP-8250](https://eips.sh/eip/8250) and [EIP-8272](https://eips.sh/eip/8272) for Hegota.

The combination of these EIPs with [EIP-8141](https://eips.sh/eip/8141), give us native, trustless, censorship resistant (via FOCIL [EIP-7805](https://eips.sh/eip/7805)) private protocol transactions on the L1. 

--- COMMENT by nerolation at 2026-06-28T05:18:05Z ---
I would like to propose [EIP-7862: Delayed State Root](https://eips.ethereum.org/EIPS/eip-7862) and [EIP-8146: Block Access List Sidecars](https://eips.ethereum.org/EIPS/eip-8146) for Hegota.

Delaying the state root inclusion (include pre-state root instead of post) simplifies building/proving.
Separating the BAL from the execution payload + having an earlier BAL deadline, makes the BAL to arrive earlier in the slot. EL clients can then start prefetching state + doing the post-state root calculation and only start parallel execution when the block arrived.

--- COMMENT by tersec at 2026-07-02T06:53:39Z ---
> I would like to propose [EIP-7862: Delayed State Root](https://eips.ethereum.org/EIPS/eip-7862) and [EIP-8146: Block Access List Sidecars](https://eips.ethereum.org/EIPS/eip-8146) for Hegota.

8146 is almost entirely a CL EIP though?

--- COMMENT by nerolation at 2026-07-02T07:19:54Z ---
yeah, I plan to also present it on ACDC but it does impact ELs as much as the CLs; Id say, very much a cross-layer proposal. E.g. ELs need to make sure they are capable of receiving the BAL early, leverage that and then be ready to parallelize transactions when the payload arrives.


--- COMMENT by qu0b at 2026-07-02T09:46:22Z ---

Updates needed for the devnet-7 spec:

1. EIP-8038 SSTORE access-cost check ("Sentry check OOG DoS") - check access cost before the storage read; EIP-7928's SSTORE text needs the matching clause
2. EIP-8037 authorization refunds - decide https://github.com/ethereum/EIPs/pull/11778 (source-based refunds for 7702 overcharges)
3. EIP-2780 open intrinsic-gas questions - delegated-recipient cold-access exception (https://github.com/ethereum/execution-specs/pull/3045); intrinsic-vs-runtime split for authorization charges

Land https://github.com/ethereum/EIPs/pull/11838 (7928 option-1 text) before the devnet-7 spec release
Glam overview EIP-7773 is missing 8282.


--- COMMENT by barnabasbusa at 2026-07-02T11:37:30Z ---
We should aim to finalize the system contract by the end of the call for 8282: 
https://github.com/ethereum/sys-asm/pull/50

Proposing new deposit contract: [0x00006AE84ed173D4394de5E28F9ED56b28008282](https://github.com/ethpandaops/ethereum-genesis-generator/commit/7cc307c718e04427c1bc75223518c31d05b23c75)
We can keep the exit contract as it was on devnet-6. 

Each contract deployment will require 0.25ETH. 

Updated our genesis generator with the new address, so if you are preparing a glamsterdam-devnet-7 branch, make sure you hardcode the new deposit contract address for that. 


--- COMMENT by benaadams at 2026-07-02T11:56:18Z ---
I'd like to propose [EIP-8298: SETCODEFROM Code Reuse Instruction](https://eips.ethereum.org/EIPS/eip-8298) for Hegotá

--- COMMENT by poojaranjan at 2026-07-02T13:32:17Z ---
Housekeeping items
- Update Glamsterdam Meta EIP-7773 - Ref: [PR](https://github.com/ethereum/EIPs/pull/11853)
- Required fixes for EIP-8282 - Ref: [PR](https://github.com/ethereum/EIPs/pull/11760)
- Promote Glamsterdam EIPs to Review

--- COMMENT by github-actions[bot] at 2026-07-03T18:34:58Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
