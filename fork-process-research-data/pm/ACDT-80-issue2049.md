# ISSUE 2049: All Core Devs - Testing (ACDT) #80, May 18, 2026
Created: 2026-05-11T15:30:06Z by danceratopz

### UTC Date & Time

[May 18, 2026, 14:00 UTC](https://savvytime.com/converter/utc/may-18-2026/2pm)

### Agenda

#### Glamsterdam

##### Fork Scoping

Prep discussion for a fast resolution in [ACDE #237](https://github.com/ethereum/pm/issues/2044) on Thurs:

- Update 7904 [ethereum/EIPs#11540](https://github.com/ethereum/EIPs/pull/11540), in draft by @Giulio2002:
  - Raise max contract code size from 24KB to 64kB.
  - Raise max EIP-3860 initcode size from 48kB to 128kB.
  - Implications for Glamsterdam's repricing?

- Anything else?
  - `SELFDESTRUCT` removal, is the consensus that this is too late?
  - 8246 Selfdestruct gas burn is CFI'd; coming up in devnet scoping below...
  - Heads up: [bal-devnet-6 EIPs were SFI'd](https://github.com/ethereum/EIPs/pull/11399) last week and STEEL is in the process of merging this into `forks/amsterdam`, tracker [ethereum/execution-specs#2846](https://github.com/ethereum/execution-specs/issues/2846).

##### Devnet Status / Readiness / Scheduling

- `bal-devnet-7`:
  - Spec at [notes.ethereum.org/@ethpandaops/bal-devnet-7](https://notes.ethereum.org/@ethpandaops/bal-devnet-7).
    - eth/70 and eth/71 required.
    - 8037 `cost_per_state_byte` bumped to 1530.
  - Latest current EELS release: [tests-bal@v7.1.1](https://github.com/ethereum/execution-specs/releases/tag/tests-bal%40v7.1.1).
    - Results available at [https://hive.ethpandaops.io/#/group/bal-quick](https://hive.ethpandaops.io/#/group/bal-quick).
  - Imminent release: tests-bal@v7.2.0.
    - Will include new 8037 and 7981 (thanks @chfast) test cases @spencer-tb.
  - Client updates? Branches? / hive results @qu0b and client teams.

- Status regarding moving benchmarking to `bal-devnet-7`.

- `glamsterdam-devnet-4`: `bal-devnet-7` + `targetGasLimit` on `PayloadAttributesV4` + consensus-specs `v1.7.0-alpha.8` See [notes.ethereum.org/@ethpandaops/glamsterdam-devnet-4](https://notes.ethereum.org/@ethpandaops/glamsterdam-devnet-4).
- Scoping `glamsterdam-devnet-5`:
  - Few topics discussed in [Repricing #7](https://forkcast.org/calls/price/007):
    - Include EIP-8038 and EIP-2780?
    - RPC-vs-tracer?
    - EIP-7904 inclusion?
  - Approximate timing?
  - [EIP-8246 Remove SELFDESTRUCT Burn](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-8246.md): [CFI'd](https://github.com/ethereum/EIPs/commit/e88ce5971abb73ba526a6e3653313a82ca3f2ebe) and already required by [EIP-7708 ETH transfers emit a log](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7708.md) @chfast.
    - EELS PR from @LouisTsai-Csie: [ethereum/execution-specs#2842](https://github.com/ethereum/execution-specs/pull/2842).

##### Other Topics

- [Produce block v4 with payload#580](https://github.com/ethereum/beacon-APIs/pull/580) @barnabasbusa 
-  eth/71 devp2p [caps/eth.md: define block-level access list (BAL)#264](https://github.com/ethereum/devp2p/pull/264).
- Which REST-SSZ Engine-API proposal @marioevz 
  - https://github.com/ethereum/execution-apis/pull/793
  - https://github.com/ethereum/execution-apis/pull/764


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

--- COMMENT by github-actions[bot] at 2026-05-11T15:31:32Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260518%2F20260519&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2380%2C+May+18%2C+2026&dates=20260518T140000Z%2F20260518T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2049)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28498)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=AHm0HzNRjUQ)

--- COMMENT by marioevz at 2026-05-14T14:50:54Z ---
Brought up during ACD-C, might be worth discussing in ACD-T to get a decision.

Which of these two REST-SSZ Engine-API proposals to go with:
- https://github.com/ethereum/execution-apis/pull/793
- https://github.com/ethereum/execution-apis/pull/764

--- COMMENT by Giulio2002 at 2026-05-14T18:03:27Z ---
Brought up last ACDT but we did not make to talk about it: https://github.com/ethereum/EIPs/pull/11540

--- COMMENT by barnabasbusa at 2026-05-18T05:56:41Z ---
Should discuss https://github.com/ethereum/beacon-APIs/pull/580

--- COMMENT by chfast at 2026-05-18T08:42:22Z ---
Are we moving on with https://eips.ethereum.org/EIPS/eip-8246 and updated https://eips.ethereum.org/EIPS/eip-7708 ?

--- COMMENT by nerolation at 2026-05-18T10:47:54Z ---
I'd like to bring up the eth/71 devp2p PR, ask for some reviews and merging it:
https://github.com/ethereum/devp2p/pull/264

--- COMMENT by danceratopz at 2026-05-19T08:44:12Z ---
Summary on forkcast:
https://forkcast.org/calls/acdt/080
