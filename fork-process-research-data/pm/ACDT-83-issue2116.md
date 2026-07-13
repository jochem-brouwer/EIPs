# ISSUE 2116: All Core Devs - Testing (ACDT) #83, June 15, 2026
Created: 2026-06-10T08:23:13Z by danceratopz

### UTC Date & Time

[June 15, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jun-15-2026/2pm)

### Agenda

#### Shared EL-CL Topics

We will have breakouts today...

##### Glamsterdam

Prep for fast resolution at [ACDE #239](https://github.com/ethereum/pm/issues/2115).

**ePBS - EIP-8282 builder deposits (go / no-go?):**

- [EIP-8282 #11760](https://github.com/ethereum/EIPs/pull/11760) @jtraglia / @wemeetagain / @ensi321 + [consensus-specs#5359](https://github.com/ethereum/consensus-specs/pull/5359) - separate builder-deposit path (EIP-7685 request types 0x03 / 0x04), Engine API ExecutionRequests extension, new system contracts ([sys-asm#43](https://github.com/ethereum/sys-asm/pull/43)).
  - Deferred here by [ACDC #180](https://github.com/ethereum/pm/issues/2113) TBD: Include in Gloas? Prelim decision here in ACDT, ratify at the next ACDE.
  - Implementation underway: CL spec [#5359](https://github.com/ethereum/consensus-specs/pull/5359), system contracts [sys-asm#43](https://github.com/ethereum/sys-asm/pull/43), Lodestar PoC [lodestar#9507](https://github.com/ChainSafe/lodestar/pull/9507) @markolazic01.
  - vs [EIP-8254](https://eips.ethereum.org/EIPS/eip-8254) deposit cap + O(1) packing - a cap alone is insufficient (the work is in `is_pending_validator`) per @potuz @tersec @nerolation.
  - Fork-delay risk of a late change. Contract frozen? Who deploys / vanity address (@pk910)?

#### Other Topics

- SSZ Engine API (EL-side) @barnabasbusa - status + what is blocking @MariusVanDerWijden's [execution-apis#793](https://github.com/ethereum/execution-apis/pull/793) (engine Rest-SSZ spec); get client teams implementing. Earlier ask: needs a 3rd CL volunteer.

##### Devnet Status / Readiness / Scheduling

- [`bal-devnet-7`](https://notes.ethereum.org/@ethpandaops/bal-devnet-7) - the last EL-specific devnet; EL benchmarking.
  - New release today: [tests-bal@v7.3.2](https://github.com/ethereum/execution-specs/releases/tag/tests-bal@v7.3.2), includes forks <= Prague; no change to Amsterdam coverage.
<!--
  - BAL-divergence bugs: geth EIP-7702 re-delegation diverges the BAL hash + state root (miner charges 35,190 state-gas, validator 0); erigon split; besu cap. @spencer-tb + client teams.
  - Gap: EELS does payload re-execution only, no block-building tests - which is why this slipped. Add them? [execution-specs#2945](https://github.com/ethereum/execution-specs/pull/2945) rejection test on the BAL agenda (@raxhvl).
-->
- [`glamsterdam-devnet-5`](https://notes.ethereum.org/@ethpandaops/glamsterdam-devnet-5) - salvaged by @barnabasbusa (not abandoned): the bugs root-caused at [ACDC #180](https://github.com/ethereum/pm/issues/2113) (Prysm peering + Grandine fork-choice) were fixed and the net recovered. Still the ePBS / buildoor + BAL substrate.
- [`glamsterdam-devnet-6`](https://notes.ethereum.org/@ethpandaops/glamsterdam-devnet-6) - target for the larger EL EIP integration.
  - EL tracker here: https://github.com/ethereum/execution-specs/issues/2915

#### EL Breakout

**Gas Repricings:**

- [EIP-2780 rework #11645](https://github.com/ethereum/EIPs/pull/11645) @misilva73 - resource-based decomposition of the 21k base; would also resolve/supersede [#11735](https://github.com/ethereum/EIPs/pull/11735) @rakita.
  - Ratify #11645 as the canonical 2780? Sub-21k transfers now look infeasible - confirm the 21k floor. Two EIP-7702 double-charge edge cases (cold-access, new-account) to close.
- EIP-8037 (state-create gas, `bal-devnet-7` spec merged to EELS [#2901](https://github.com/ethereum/execution-specs/pull/2901)):
  - Strict block-gas inclusion check, EIP-literal vs EELS-subtract: land [execution-specs#2892](https://github.com/ethereum/execution-specs/pull/2892) @chfast.
  - EIP-7928 BAL conflict, @rjl493456442 to present the [hackmd](https://hackmd.io/@bFEBbZiVSAO0IURh9qzEFg/BJmFYqCeGl) proposal: charge account-creation in the parent frame + refund - ratify? CALL stays pre-charged (in-frame charging breaks the 2300-gas stipend).
  - Reservoir-based vs source-based refund - decide.
  - [EIP-8037 7702 overcharge #11778](https://github.com/ethereum/EIPs/pull/11778) @lu-pinto - CHANGES_REQUESTED by @jochem-brouwer, unresolved.
- [EIP-8038 initial numbers #11802](https://github.com/ethereum/EIPs/pull/11802) @misilva73 - first concrete state-access numbers (STORAGE_WRITE +257%, STORAGE_CLEAR_REFUND +160%, CREATE_ACCESS +57%).
- EIP-8038 heads up: Plan is to create a combined `eip-8038-2780` branch.
- [EIP-7904 -> Informational #11622](https://github.com/ethereum/EIPs/pull/11622) @misilva73 - no compute repricing needed.

Other topics:
- [EIP-7997 Arachnid factory #11783](https://github.com/ethereum/EIPs/pull/11783) @frangio - pivot to the canonical keyless factory at `0x4e59...4956C` (follows ACDT #82 Option 2). Heads up.

#### CL Breakout

**ePBS - fork choice (EIP-7732), @jihoonsong - [consensus-specs#5348](https://github.com/ethereum/consensus-specs/pull/5348) `Modify get_proposer_head for Gloas`**
- `get_proposer_head` takes/returns `ForkChoiceNode`; `is_parent_strong` now counts a PENDING parent's support ([#5305](https://github.com/ethereum/consensus-specs/issues/5305)) - both Gloas. Also drops `is_shuffling_stable` (Fulu proposer lookahead; not Gloas-specific).
    - Review + interop implications for the ePBS devnet?

Other topics:
- QUIC vs mplex (CL networking) @barnabasbusa - aim to decide in the coming weeks whether to drop mplex for beacon p2p. Teku shipped QUIC in its latest stable release; status fro Nimbus? (ACDC #180 had removal at ~Gloas + 2 months; Barnabas wants the decision sooner.)

- Builder API / buildoor readiness (gates ePBS + 8282 testing): prysm Builder API + auth working @terencechain; ([beacon-APIs#588](https://github.com/ethereum/beacon-APIs/pull/588)) @nflaig.

- Staked Builder API [builder-specs#138](https://github.com/ethereum/builder-specs/pull/138) merged. Heads up.

Last call:
- ACDT # 82 Agenda: https://github.com/ethereum/pm/issues/2103.
- [ACDT # 82 on forkcast](https://forkcast.org/calls/acdt/082) (with summary).

**Please comment on this issue with other topics!**

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

- [ ] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2026-06-10T08:24:14Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260615%2F20260616&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2383%2C+June+15%2C+2026&dates=20260615T140000Z%2F20260615T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2116)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28759)

--- COMMENT by jihoonsong at 2026-06-13T14:36:44Z ---
I would like to discuss `get_proposer_head` changes introduced in this [PR](https://github.com/ethereum/consensus-specs/pull/5348). It does three things:

1. It changes `get_proposer_head` signature to receive and return `ForkChoiceNode` and adds a missing specification to optionally use it when building a block.

2. It modifies `is_parent_strong` to count support for a PENDING parent. See [#5305](https://github.com/ethereum/consensus-specs/issues/5305) for a relevant discussion.

3. It removes `is_shuffling_stable` since Fulu as the proposer lookahead allows us to deprecate it.

FYI, 1 and 2 are related to Gloas while 3 is not.

--- COMMENT by rjl493456442 at 2026-06-15T08:02:31Z ---
I would like to discuss this EIP-8037 change, proposal https://hackmd.io/@bFEBbZiVSAO0IURh9qzEFg/BJmFYqCeGl

--- COMMENT by nflaig at 2026-06-15T09:06:38Z ---
We would like to talk about EIP-8282 again and it's inclusion in Glamsterdam
- https://github.com/ethereum/EIPs/pull/11760 (eip itself)
- https://github.com/ethereum/consensus-specs/pull/5359 (spec change)
- https://github.com/ethereum/sys-asm/pull/43 (system contracts)
- https://github.com/ChainSafe/lodestar/pull/9507 (lodestar poc)

cc @wemeetagain @jtraglia @ensi321 @markolazic01



--- COMMENT by barnabasbusa at 2026-06-15T10:57:19Z ---
QUIC or not to QUIC? We should aim to make a decision whether to drop mplex for beacons in the coming weeks. 
Teku just merged in their quic implementation in the latest stable release. 
Right now the only CL client holding out is nimbus afaik. Could we get an update on what's the hold up for them? 

--- COMMENT by barnabasbusa at 2026-06-15T11:01:19Z ---
Can we get an update on sszing the engine API? 
What's the hold up on merging in Marius's PR, and get client teams to take a look at implementing it? 

--- COMMENT by danceratopz at 2026-06-16T06:06:28Z ---
Summary on forkcast:
https://forkcast.org/calls/acdt/083/
