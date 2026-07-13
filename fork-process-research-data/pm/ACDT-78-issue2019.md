# ISSUE 2019: All Core Devs - Testing (ACDT) #78, April 20, 2026
Created: 2026-04-13T22:01:23Z by marioevz

### UTC Date & Time

[April 20, 2026, 14:00 UTC](https://savvytime.com/converter/utc/apr-20-2026/2pm)

### Agenda

- blob-devnet-0
- bal devnets or [glamsterdam devnet](https://github.com/ethereum/pm/issues/2019#issuecomment-4280860309)
- epbs-devnet-1
- [engine: EL must support reorg to head's ancestor](https://github.com/ethereum/execution-apis/pull/770)
- debug RPC update

### Call Series

All Core Devs - Testing

### Autopilot Mode

- [x] Use autopilot (recommended defaults for this call series)


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

60 minutes

### Occurrence Rate

weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Display Zoom Link in Calendar Invite (Optional)

- [x] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2026-04-13T22:02:23Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260420%2F20260421&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2378%2C+April+20%2C+2026&dates=20260420T140000Z%2F20260420T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2019)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28229)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=ZvG3OkEt8_o)

--- COMMENT by parithosh at 2026-04-20T08:41:18Z ---
We will be skipping next weeks ACDT due to an event conflict. 

--- COMMENT by nerolation at 2026-04-20T09:19:18Z ---
Has been a quiet week for BALs (at least on the specs + tests side). Therefore I'd propose to use most of the time in the BAL breakout for 8037 discussions (cc @misilva73):

For the first part of the breakout (EIP_7928), that's the proposed agenda:
* Move BlockAccessIndex from `uint16` to `uint64`.
  * EIP update: https://github.com/ethereum/EIPs/pull/11535
  * Specs update: https://github.com/ethereum/execution-specs/pull/2713
* Discussion: BAL item ordering
* Benchmarking updates 


--- COMMENT by MariusVanDerWijden at 2026-04-20T12:47:05Z ---
I propose to go straight to glamsterdam-devnet-0 for ELs instead of going to BAL-devnet-4, since we can use non-epbs CL nodes for BAL testing if we feel like glamsterdam-devnet-0 is not reliable on CLs. (The only change for ELs to support full glamsterdam is to allow for reorging the head block)

--- COMMENT by akashkshirsagar31 at 2026-04-20T12:59:17Z ---
X Stream: https://x.com/i/broadcasts/1qGvvkBdeYwGB

--- COMMENT by github-actions[bot] at 2026-04-21T18:33:11Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
