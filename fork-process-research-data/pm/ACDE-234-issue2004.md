# ISSUE 2004: All Core Devs - Execution (ACDE) #234, April 9, 2026
Created: 2026-04-04T00:01:26Z by nixorokish

### UTC Date & Time

[April 09, 2026, 14:00 UTC](https://savvytime.com/converter/utc/apr-9-2026/2pm)

### Agenda

Glamsterdam
- Devnet updates
- [SFI definition](https://github.com/ethereum/EIPs/pull/11475) + [Glamsterdam SFIs](https://github.com/ethereum/EIPs/pull/11399)
- Engine API changes: https://github.com/ethereum/pm/issues/2004#issuecomment-4204427871

Hegotá
- Decisions from [previous call](https://forkcast.org/calls/acde/233#t=5871): Frame txs CFI'd as a non-headliner, but broadly will consider it a placeholder to decide on an AA direction
- Non-headliner proposals open - no end date right now, but will give at least 2 weeks notice. Expect some progress / suggestions to come out of interop
- Antonio Sanso / 2 min on AA: https://github.com/ethereum/pm/issues/2004#issuecomment-4205882075
- Ben Adams / alt AA proposal: https://github.com/ethereum/pm/issues/2004#issuecomment-4214209463
 
Misc
- Sina / History pruning targets: https://github.com/ethereum/pm/issues/2004#issuecomment-4213706943
- Chase / Request for EL devs to comment on adding a new RPC method: https://github.com/ethereum/pm/issues/2004#issuecomment-4200750948
- Barnabas / Bring SSZ to the engine API: https://github.com/ethereum/pm/issues/2004#issuecomment-4214487667

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

--- COMMENT by github-actions[bot] at 2026-04-04T00:02:26Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260409%2F20260410&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Execution+%28ACDE%29+%23234%2C+April+9%2C+2026&dates=20260409T140000Z%2F20260409T153000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F85451723466%3Fpwd%3DRgAVD0sO4OPFAU3UIqPflda1z05cRT.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2004)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28142)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=Gb07G2uckdg)

--- COMMENT by MysticRyuujin at 2026-04-07T16:49:52Z ---
It would be cool if we could get some of the EL devs to comment on this PR spec:
https://github.com/ethereum/execution-apis/pull/755

The idea being that `eth_capabilities` will return the effective pruning policy of your nodes.

We're also looking at an `admin_capabilities`, that could expose more client specific settings and configuration, but that would be optional and not part of the "spec" -  the part we need client agreement on would be the spec of `eth_capabilities` only

--- COMMENT by mkalinin at 2026-04-08T07:04:55Z ---
I would like to quickly touch the following Engine API changes:
* https://github.com/ethereum/execution-apis/pull/760 — related to bootstrapping with a non-finalized checkpoint
* https://github.com/ethereum/execution-apis/pull/770 — related to Glamsterdam
* Potential deprecation of “safe” block tag and addition of the two in replacement: “justified”, “confirmed”. Engine API `fcU` can be equipped with `justifiedBlockHash` and `confirmedBlockHash` in replacement to `safeBlockHash`

--- COMMENT by asanso at 2026-04-08T11:24:07Z ---
I would like a 2-Minute Plea for Native Account Abstraction in H*

--- COMMENT by akashkshirsagar31 at 2026-04-09T02:09:32Z ---
X Stream: https://x.com/i/broadcasts/1qJDzPlWmgQKV

--- COMMENT by s1na at 2026-04-09T11:16:20Z ---
If there was any time left I'd like to bring up history pruning targets. We'd like to get a sense of which configurations are already possible on the clients and what we can agree on wrt future pruning targets.

--- COMMENT by benaadams at 2026-04-09T12:30:56Z ---
I'd like to mention an alternative AA proposal (probably after @asanso ) 

Is ERC-8221: Wallet Title Deeds; that I believe solves most of the properties of AA that have been mentioned so far and doesn't need 4337 or any other specialist changes. It handles key rotation, but it doesn't handle alternative signing schemes (like PK) which it would need to be handled at the tx level

https://github.com/ethereum/ERCs/pull/1658

--- COMMENT by barnabasbusa at 2026-04-09T13:13:34Z ---
I'd like to raise https://github.com/ethereum/execution-apis/pull/764, optionally bringing ssz to the engine api. 

--- COMMENT by pedrouid at 2026-04-09T15:19:04Z ---
@nixorokish as mentioned on the call I would like to lead the AA breakout and would love to know about next steps... feel free to contact me via email (which was shared on Zoom) or Telegram at @pedrouid

--- COMMENT by github-actions[bot] at 2026-04-10T18:26:20Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
