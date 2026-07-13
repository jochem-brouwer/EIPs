# ISSUE 2015: All Core Devs - Execution (ACDE) #235, April 23, 2026
Created: 2026-04-11T17:17:55Z by nixorokish

### UTC Date & Time

[April 23, 2026, 14:00 UTC](https://savvytime.com/converter/utc/apr-23-2026/2pm)

### Agenda

- Glamsterdam
  - devnet updates
  - Engine API change: https://github.com/ethereum/execution-apis/pull/770 or https://github.com/ethereum/execution-apis/pull/786 by @mkalinin / @qu0b
  - [EIP-8237](https://github.com/ethereum/EIPs/pull/11557) by @potuz
  - [EIP-8037 testing concerns](https://github.com/ethereum/pm/issues/2015#issuecomment-4305210461) by @marioevz
  - [mascot last call](https://github.com/ethereum/pm/issues/2015#issuecomment-4302071363) by @abcoathup
- Hegotá
  - [EIP-8163](https://eips.ethereum.org/EIPS/eip-8163) by @pdobacz 
  - [EIP-7979](https://eips.ethereum.org/EIPS/eip-7979) by @gcolvin, see [comment](https://github.com/ethereum/pm/issues/2015#issuecomment-4301212656) for context
- Misc
  - [Engine API performance for zkEVM use cases](https://github.com/ethereum/execution-apis/pull/773) by @developeruche
  - https://github.com/ethereum/execution-apis/pull/755 and https://github.com/ethereum/execution-apis/pull/774 by @bomanaps 

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

--- COMMENT by github-actions[bot] at 2026-04-11T17:19:02Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260423%2F20260424&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Execution+%28ACDE%29+%23235%2C+April+23%2C+2026&dates=20260423T140000Z%2F20260423T153000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F85451723466%3Fpwd%3DRgAVD0sO4OPFAU3UIqPflda1z05cRT.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2015)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28203)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=eSN27EDNdp8)

--- COMMENT by developeruche at 2026-04-20T15:51:26Z ---
Would love to get some EL dev's comment on this;

https://github.com/ethereum/execution-apis/pull/773

--- COMMENT by pdobacz at 2026-04-22T09:27:47Z ---
I would like to quickly mention and get feedback on [EIP-8163: Reserve `EXTENSION (0xae)` opcode](https://eips.ethereum.org/EIPS/eip-8163)

--- COMMENT by qu0b at 2026-04-22T11:38:06Z ---
Would be good to discuss https://github.com/ethereum/execution-apis/pull/786 or https://github.com/ethereum/execution-apis/pull/770 further so we can have the necessary changes merged

--- COMMENT by gcolvin at 2026-04-23T01:59:00Z ---
I'd like to move this to Proposed for Inclusion:
[EIP-7979: Call and Return Opcodes for the EVM](https://eips.ethereum.org/EIPS/eip-7979)
Most of the motivation for this I've moved to a broader Informational document:
[EIP-8173: Foundations of EVM Control Flow](https://eips.ethereum.org/EIPS/eip-8173)

PS.  For those who eventually want a more comprehensive solution there is a follow-on, [EIP-8013](https://eips.ethereum.org/EIPS/eip-8013), a version of EOF Functions without the code sections and other overhead.   


--- COMMENT by bomanaps at 2026-04-23T05:49:38Z ---
I would also love to add this to the agenda  nothing biggy, but for EL clients to please take a look at this PR that proposes a new eth_capabilities method and drop their  feedback on it  https://github.com/ethereum/execution-apis/pull/755 , and also this https://github.com/ethereum/execution-apis/pull/774 thank you 

--- COMMENT by abcoathup at 2026-04-23T06:03:16Z ---
## Mascot needed for Glamsterdam upgrade
Last call for suggestions.

Suggestions so far: 
🐜 Ant, 🦫 Beaver, 🐝 Bee, 🦩 Flamingo, 🐹 Hamster, 🐩 Poodle

https://ethereum-magicians.org/t/mascot-needed-for-glamsterdam-upgrade/26008

cc: @eviljordan (Mascot wrestler)



--- COMMENT by EvilJordan at 2026-04-23T06:05:02Z ---
~~The headliner is FOCIL, no? Then these are _super_ in play: 🦖🦕~~
HEGOTA!

--- COMMENT by abcoathup at 2026-04-23T07:19:45Z ---
## Mascot needed for Hegotá upgrade

> 🦖🦕

https://ethereum-magicians.org/t/mascot-needed-for-hegota-upgrade/28327

--- COMMENT by potuz at 2026-04-23T11:34:40Z ---
If there's time I'd like to discuss EIP-8237 https://ethereum-magicians.org/t/eip-8237-independent-cl-el-sync/28331

--- COMMENT by akashkshirsagar31 at 2026-04-23T12:43:49Z ---
X Stream: https://x.com/i/broadcasts/1qKVmQVZPElxB

--- COMMENT by petertdavies at 2026-04-23T13:38:34Z ---
STEEL would like to request a temperature check on EIP-8037.

We think that EIP-8037 is presenting significant testing and implementation challenges and are concerned that it may not be shippable without major changes. We are concerned that continuing on the current path will delay the fork. Are other teams also struggling with as much as STEEL?

--- COMMENT by marioevz at 2026-04-23T14:21:45Z ---
Wanted to list the testing concerns specifically here for us to be able to go through them in the call:
- Derivation of the state gas from the block gas limit is breaking test determinism because we now have to modify every test to account for account creation. Around 30% of tests needed to be updated for us to be able to fill them.
- Tests that required a specific gas limit to test an edge case are now in particular more difficult to achieve because of how the compute gas and state gas are conflated via the reservoir (pulling from the reservoir during opcode execution makes it difficult to predict if we could intentionally verify an Out-of-gas situation for testing purposes).
- Number of edge cases due to refunding mechanisms are not yet sufficiently covered by our tests and we need more time to fully cover these.
- The benchmark tests are in particular difficult to fill due to tests already relying on a specific block gas limit to setup and then a different gas limit during test execution.

--- COMMENT by github-actions[bot] at 2026-04-24T18:22:37Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
