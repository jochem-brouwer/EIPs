# ISSUE 2115: All Core Devs - Execution (ACDE) #239, June 18, 2026
Created: 2026-06-09T12:52:14Z by nixorokish

### UTC Date & Time

[June 18, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jun-18-2026/2pm)

Today's facilitator: @nixorokish 

### Agenda

- Glamsterdam
  - devnet updates [ @barnabasbusa ]
  - [EIP-8282 CFI needed](https://forkcast.org/calls/acdt/083/#t=1259) [ @wemeetagain or whoever from ACDT ]
  - [Add EIP-8189 to Glamsterdam networking section](https://github.com/ethereum/EIPs/pull/11792) [ @barnabasbusa ]
  - [EIP-7928 State-Access Restructuring](https://github.com/ethereum/execution-specs/pull/2900) [ @nerolation ]
  - Repricings update: 8037 status & 7904 announcement [ @misilva73 ]
- Hegotá 
  - PFI [EIP-8304](https://github.com/ethereum/EIPs/pull/11811): Trustless log & transaction index [ @zsfelfoldi ]
  - PFI [EIP-2488](https://eips.ethereum.org/EIPS/eip-2488): Deprecate the CALLCODE opcode & [EIP-7645](https://eips.ethereum.org/EIPS/eip-7645): Alias ORIGIN to SENDER [ @abc-123-c ]
- Misc
  - Mempool encryption EIPs [-8105](https://eips.ethereum.org/EIPS/eip-8105) and [-8184](https://eips.ethereum.org/EIPS/eip-8184) are [looking for feedback](https://github.com/ethereum/pm/issues/2115#issuecomment-4739713978)
  - [Mascot needed](https://ethereum-magicians.org/t/mascot-needed-for-glamsterdam-upgrade/26008) for Glamsterdam

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

--- COMMENT by github-actions[bot] at 2026-06-09T12:53:10Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260618%2F20260619&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Execution+%28ACDE%29+%23239%2C+June+18%2C+2026&dates=20260618T140000Z%2F20260618T153000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F85451723466%3Fpwd%3DRgAVD0sO4OPFAU3UIqPflda1z05cRT.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2115)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28751)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=Y1r-O9Vdl7I)

--- COMMENT by barnabasbusa at 2026-06-10T15:02:44Z ---
Would like to add an optional networking EIP to the Glamsterdam meta EIP: https://github.com/ethereum/EIPs/pull/11792

--- COMMENT by nerolation at 2026-06-15T15:04:56Z ---
I want to get to a decision about the following PRs:

EELS:
https://github.com/ethereum/execution-specs/pull/2900

And the EIP:
https://github.com/ethereum/EIPs/compare/master...nerolation:EIPs:toni/state-access

Background:
Restructure CALL/CALLCODE/DELEGATECALL/STATICCALL into pre-state gas → free depth/balance preconditions → post-state gas + warming + BAL insertion, per EIP-7928. The target's cold-access surcharge and accessed_addresses.add(...) are deferred until after preconditions pass; EIP-2929 cost↔warm pairing is preserved. generic_call's depth check is removed (callers handle it).

--- COMMENT by abcoathup at 2026-06-17T06:47:55Z ---
**Mascot needed for Glamsterdam upgrade**
🐜🦫🐝🦩🐹🐙🐻‍❄️🐩
Vote for your favorite on Eth Magicians

Top 3 currently: 🦩 42%, 🐻‍❄️ 25% & 🦫 19%

https://ethereum-magicians.org/t/mascot-needed-for-glamsterdam-upgrade/26008

--- COMMENT by zsfelfoldi at 2026-06-17T16:04:21Z ---
I would like to propose https://github.com/ethereum/EIPs/pull/11811 for Hegota.
This EIP is a result of the same “trustless log index” project as EIP-7745 but it is a new and different (much simpler and even more efficient) design.

--- COMMENT by abc-123-c at 2026-06-17T17:06:09Z ---
I would like to propose https://github.com/ethereum/EIPs/pull/11793 for Hegota.
Prioritize considering EIP-7645 first to support account abstraction.

--- COMMENT by akashkshirsagar31 at 2026-06-17T19:39:28Z ---
X Stream: https://x.com/i/broadcasts/1jxXggAkYlPJZ

--- COMMENT by zsfelfoldi at 2026-06-18T08:22:54Z ---
Update: the number of the EIP I'd like to present is 8304.

--- COMMENT by LoringHarkness at 2026-06-18T08:33:00Z ---
Announcement:
(I am not able to attend the call this week. But, if possible, please read for attendees.)

**Shutter and the "Encrypt The Mempool!" coalition is offering a 3 ETH bounty for feedback on EIPs [8105](https://eips.ethereum.org/EIPS/eip-8105) and [8184](https://eips.ethereum.org/EIPS/eip-8184).**

Suggested topics include:

- dealing with key withholding attacks
- leveraging FRAME transactions
- improving transaction execution quality
- and more!

If you have feedback, please share your feedback and submit a claim by **EOD July 30**.
If you just want to Encrypt The Mempool, please add funds (even 0.001 ETH) to the bounty pool.

https://poidh.xyz/mainnet/bounty/11

--- COMMENT by thechouhomespa at 2026-06-18T22:36:06Z ---
Shutter và liên minh "Encrypt The Mempool!" đang treo thưởng 3 ETH cho phản hồi về EIP 8105 và 8184 .

<img width="720" height="735" alt="Image" src="https://github.com/user-attachments/assets/ebcdc219-c448-4389-aa73-8af15d416ee4" />

--- COMMENT by github-actions[bot] at 2026-06-19T18:47:00Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
