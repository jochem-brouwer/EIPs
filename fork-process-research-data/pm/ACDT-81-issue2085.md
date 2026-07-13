# ISSUE 2085: All Core Devs - Testing (ACDT) #81, June 1, 2026
Created: 2026-05-27T16:49:36Z by danceratopz

### UTC Date & Time

[June 01, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jun-1-2026/2pm)

### Agenda

#### Glamsterdam

Heads up: As repricing specs have stabilized, the fortnightly Wednesday Gas Repricing Breakout Calls held by @misilva73 have stopped; any required discussion will happen in ACDT.

##### Devnet Status and Benchmarking

- `bal-devnet-7`, [hive dashboard](https://hive.ethpandaops.io/#/group/bal), [dora](https://dora.bal-devnet-7.ethpandaops.io/):
  - Stable, 100% EVM fuzzing participation (reported in [Repricings #8](https://forkcast.org/calls/price/008)) @qu0b.
  - Clients:
    - `eth/70` (EIP-7975) and `eth/71` (EIP-8159) are mandatory on `bal-devnet-7` - is this work complete? @qu0b.
  - Final test release `tests-bal@v7.3.0` once 8037 lands in `forks/amsterdam`, tracker: [ethereum/execution-specs#2912](https://github.com/ethereum/execution-specs/issues/2912) @spencer-tb.

- Benchmarking / repricing status (@misilva73 + client teams):
  - State of client BAL optimizations in `bal-devnet-7` branches? Per-client round.
    - ethrex reports ~3x exec speed-up vs devnet-3?
    - Nethermind OOM affecting benchmarking results?
    - Besu (shipped AOT and an OOM fix).
    - Geth BAL opt branch this week?
  - How much of a priority is it to replace the `perf-devnet-3` snapshots with [ethereum/state-actor](https://github.com/ethereum/state-actor)? ([execution-specs#2916](https://github.com/ethereum/execution-specs/issues/2916)). This can be deferred to tomorrow's Gas Lighting call.

- `glamsterdam-devnet-4` not so healthy as of ~May 25::
  - Chain split ~slot 22800; prysm-nethermind-1 forked on EMPTY after missing the 22800 payload.
  - PR ([consensus-specs#5307](https://github.com/ethereum/consensus-specs/issues/5307)).
  - Recover the chain or abandon for devnet-5?

##### Spec Changes

- EIP-7928 heads up: [ethereum/EIPs#11750](https://github.com/ethereum/EIPs/pull/11750), (merged) by @raxhvl, flagged by @bharnett:
  - Each `SlotChanges` MUST contain >=1 `StorageChange` (no empty change sets); EELS test added.

- EIP-7928, @nerolation wants to discuss decoupling state-access validation from EIP-2929:  stack-depth and balance checks move into the pre-state phase, and cold-access warming plus BAL insertion are now paired with each post-state access-cost charge.
  - <https://github.com/ethereum/EIPs/compare/master...nerolation:EIPs:toni/state-access>
  - Related discussion Eth R&D: <https://discord.com/channels/595666850260713488/1506543443172921464/1509797201340137544>

- EIP-8037 heads up [ethereum/EIPs#11706](https://github.com/ethereum/EIPs/pull/11706) (merged) @misilva73.
  - Calldata floor accounting alignment & call-frame refill clarification

- EIP-8037, [ethereum/EIPs#11715](https://github.com/ethereum/EIPs/pull/11715), 8037 (open) by @rjl493456442:
  - Expand EIP-7702 authorization gas refunds (5-rule per-authorization scheme).
  - Client-team opinions before approving: do we want this added refund complexity / any attack vectors?

- EIP-2780, [ethereum/EIPs#11735](https://github.com/ethereum/EIPs/pull/11735), 2780 in draft by @rakita:
  - Remove the self-transfer and precompile intrinsic-gas carve-outs; always charge `tx.to` at the cold rate.
  - Open: keep charging `TRANSFER_LOG_COST` for self-transfers when EIP-7708 emits no log? (@benaadams objects; @rakita agrees PR will change.)
  - Architectural gate (@gurukamath): one gas cost, or split **Code vs No-Code** accounts? Blocks the impl + numbers.

- [EIP-8246 Remove SELFDESTRUCT Burn](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-8246.md): Review, required by [EIP-7708](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7708.md) @chfast - in the devnet-5 set?

##### Glamsterdam spec next TODOs

1. Finalize gas repricings numbers (ongoing on bal-devnet-7).
2. CL stability; resolve [consensus-specs#5307](https://github.com/ethereum/consensus-specs/issues/5307).
3. Include new EL EIPS:
  i. EIP-8246 (remove self-destruct burn).
  ii. EIP-2780 (intrinsic gas) and EIP-8038 (state-access gas cost update), requires 1., but not 4.
4. Updated EL EIP: EIP-7954 (increase max contract size w/larger numbers).
5. EL EIP spec changes as above.
6. Any other CL changes?

  i. [EIP-8045: Exclude slashed validators from proposing](https://eips.ethereum.org/EIPS/eip-8045).
  ii. [EIP-7688: Forward compatible consensus data structures](https://eips.ethereum.org/EIPS/eip-7688) @etan-status?
  iii. ...

##### Options and scoping of `glamsterdam-devnet-5` OR next devnets

1. Clean `glamsterdam-devnet-4` spec relaunch. (CL ask from [ACDC #179](https://forkcast.org/calls/acdc/179): stability first, no scope increase for 2 weeks).
2. `glamsterdam-devnet-4` with `bal-devnet-7` (with or without some SFId EL EIP spec changes, details below).
3. As 2. but with `bal-devnet-7` with 2780/8038 placeholder numbers.
4. Launch two devnets simultaneously and merge into later devnet:
  i. `glamsterdam-devnet-5` for CL stability.
  ii. `bal-devnet-8` for close-to-final EL spec.
  iii. -> merge into `glamsterdam-devnet-6`.

- Timing of devnet launch(es)?

##### Other Topics

- SSZ Engine API: #764 closed for [execution-apis#793](https://github.com/ethereum/execution-apis/pull/793) @mariusvanderwijden ([ACDC #179](https://forkcast.org/calls/acdc/179)) - EL teams moving to 793?
- Heads up: EPF cohort-7 starting - [submit project ideas](https://github.com/eth-protocol-fellows/cohort-seven/blob/main/projects/project-ideas.md).
- Heads up: [ACDC #180](https://github.com/ethereum/pm/issues/2061) (June 11) trials an 11:00 UTC slot to allow easier participation from APAC devs.

**Please comment on this issue with other topics!**

Last call:
- ACDT # 80 Agenda: https://github.com/ethereum/pm/issues/2049.
- [ACDT # 80 on forkcast](https://forkcast.org/calls/acdt/080) (with summary).

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

--- COMMENT by github-actions[bot] at 2026-05-27T16:50:48Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260601%2F20260602&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2381%2C+June+1%2C+2026&dates=20260601T140000Z%2F20260601T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2085)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28643)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=MldFzAj_yx8)

--- COMMENT by nerolation at 2026-05-29T06:08:09Z ---
I want to discuss the following PR.  It decouples state-access validation from EIP-2929: stack-depth and balance checks move into the pre-state phase, and cold-access warming plus BAL insertion are now paired with each post-state access-cost charge.

https://github.com/ethereum/EIPs/compare/master...nerolation:EIPs:toni/state-access

Related discussion:
https://discord.com/channels/595666850260713488/1506543443172921464/1509797201340137544

--- COMMENT by akashkshirsagar31 at 2026-06-01T06:05:34Z ---
X Stream: https://x.com/i/broadcasts/1PKqrrPWaRQGb

--- COMMENT by misilva73 at 2026-06-01T11:41:28Z ---
For the glamsterdam-devnet-5 scoping discussion, a quick update on changes to EIP-8037:
- [Update EIP-8037: Calldata floor accounting alignment & call-frame refill clarification](https://github.com/ethereum/EIPs/pull/11706): merged 
- [Update EIP-8037: improve EIP-7702 authorization gas accounting in EIP-8037](https://github.com/ethereum/EIPs/pull/11715): still open; want to discuss inclusion in this call. 

--- COMMENT by qu0b at 2026-06-01T12:38:24Z ---
Would be great if clients could confirm work on eth/70 and eth/71 is done

--- COMMENT by etan-status at 2026-06-01T14:50:11Z ---
For 7688 (i.e., the proper fix for handling the deposits SSZ limit with 200M gas), Nimbus/Lodestar is ready.

--- COMMENT by danceratopz at 2026-06-02T06:55:09Z ---
Summary on forkcast:
https://forkcast.org/calls/acdt/081
