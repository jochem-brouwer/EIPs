# ISSUE 2076: All Core Devs - Execution (ACDE) #238, June 4, 2026
Created: 2026-05-22T20:41:45Z by nixorokish

### UTC Date & Time

[June 04, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jun-4-2026/2pm)

Today's facilitator: @adietrichs 

### Agenda

- Glamsterdam
  - devnet updates
    - bal-devnet-7
    - glamsterdam-devnet-5
    - glamsterdam-devnet-6
- Hegotá
  - EIP Proposals
    - [EIP-8131](https://eips.ethereum.org/EIPS/eip-8131) and [EIP-8279](https://github.com/nerolation/EIPs/blob/b49a74d151dde3e5bd4aa8f9aea39d0d0b23b408/EIPS/eip-8279.md) by @nerolation
    - [EIP-7979](https://eips.ethereum.org/EIPS/eip-7979) and [EIP-8173](https://eips.ethereum.org/EIPS/eip-8173) by @gcolvin 
      - see [comment](https://github.com/ethereum/pm/issues/2076#issuecomment-4566416227) for context
    - [EIP-8222](https://github.com/ethereum/EIPs/pull/11500) by @mmjahanara 
    - [EIP-7851](https://eips.ethereum.org/EIPS/eip-7851) and [EIP-8151](https://eips.ethereum.org/EIPS/eip-8151) by @lightclient 

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

--- COMMENT by github-actions[bot] at 2026-05-22T20:42:33Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260604%2F20260605&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Execution+%28ACDE%29+%23238%2C+June+4%2C+2026&dates=20260604T140000Z%2F20260604T153000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F85451723466%3Fpwd%3DRgAVD0sO4OPFAU3UIqPflda1z05cRT.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2076)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28596)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=FIiT1ZRsiDE)

--- COMMENT by nerolation at 2026-05-26T07:17:08Z ---
I would like to propose two data repricing [EIP-8131](https://eips.ethereum.org/EIPS/eip-8131) and [EIP-8279](https://github.com/nerolation/EIPs/blob/b49a74d151dde3e5bd4aa8f9aea39d0d0b23b408/EIPS/eip-8279.md) for Hegota.

--- COMMENT by gcolvin at 2026-05-28T17:02:29Z ---
I would like to further discuss EIP-7979 in the context of PFI/CFI and planning which subjects need to be discussed on a breakout call.

These are the relevant propals:
_[EIP-7979: Call and Return Opcodes for the EVM](https://github.com/gcolvin/EIPs/blob/359ce45328f9478ce058baa58d1ffbb5c0baf440/EIPS/eip-7979.md)_
_[EIP-8173: Foundations of EVM Control Flow](https://eips.ethereum.org/EIPS/eip-8173)_

These are some outside discussions of the issues:
_[The EVM Has No Subroutines. Here's Why That's a ZK Problem, and the Fix](https://tryethernal.com/blog/eip-7979-evm-subroutines-zk-control-flow)_
_[Static Analysis Deserves a Seat at the Table](https://www.certora.com/blog/static-analysis-eof-response)_

The main substantive change since the earlier proposal is that canonical EVM code for validation will be placed on the blockchain where clients can call it directly.  The intent is to make implementing the proposal as easy as possible.


--- COMMENT by mmjahanara at 2026-06-01T14:51:23Z ---
I would like to briefly present [EIP-8222](https://ethereum-magicians.org/t/eip-8222-lean-staking/28196) and gather some feedback. 

the goal of the EIP is to (a) separate execution layer identity of validators from their consensus layer identity (b) provide a native plausibly deniable method for private transfers. 

--- COMMENT by lightclient at 2026-06-03T21:40:13Z ---
Would like to propose [EIP-7851](https://eips.sh/eip/7851) and [EIP-8151](https://eips.sh/eip/8151) for Hegota.

The combination of these EIPs provide a complete, secure migration path from EOA -> full smart account. With 7851, we would introduce a new instruction which would allow EOAs to write a permanent delegation to their account. 8151 checks the recovered address from 7851 to determine if the account permanently revoked it's key and reverts if so.

--- COMMENT by github-actions[bot] at 2026-06-05T18:48:46Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
