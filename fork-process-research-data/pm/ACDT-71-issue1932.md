# ISSUE 1932: All Core Devs - Testing (ACDT) #71, February 23, 2026
Created: 2026-02-17T17:35:42Z by marioevz

### UTC Date & Time

[February 23, 2026, 14:00 UTC](https://savvytime.com/converter/utc/feb-23-2026/2pm)

### Agenda

### Fusaka:

- blob-devnet-0 updates
- partial cells / getBlobv3 implementation update

### Glamsterdam:

- bal-devnet-2 updates
- bal-devnet-3 readiness
  - EIP-8037 specs/tests status
- epbs-devnet-0 implementation update
  - client readiness check
- Adding [`testing_buildBlockV1`](https://github.com/ethereum/execution-apis/pull/747) for future devnets (4+)
  - Client teams readiness

### Gas limit:

- TBD

### State bloat:

- perf-devnet-2 updates


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

--- COMMENT by github-actions[bot] at 2026-02-17T17:36:47Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27758)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=w97pUWO0ABA)

--- COMMENT by parithosh at 2026-02-17T17:49:18Z ---
I'd like to bring up the inclusion of testing_buildBlockV1 (https://github.com/ethereum/execution-apis/pull/747) for bal-devnet-4 onwards (so not the next devnet, but the one after). cc @mysticRyuujin 


--- COMMENT by marioevz at 2026-02-23T12:51:34Z ---
@yperbasis via discord on Erigon status for devnet-2:
> We've finished our bal-devnet-2 implementation (kudos to Mark Holt) and pass all the tests more or less. We're working on a couple of remaining flaky tests and pesky races in parallelized execution.

--- COMMENT by spencer-tb at 2026-02-23T12:54:18Z ---
I want to briefly bring up some issues we've been having with the EELS testing framework for 8037. The EIP adds a new paradigm at a high level on the framework: gas costs are now dependent on the block gas limit via cost per state byte. This impacts not only the framework but a lot of tests >600 files. We're working on a good framework solution for this but if we don't get there by ACDE we might want to consider making the `cost_per_state_byte` a fork constant, this massively reduces he complexity on the framework side, and will enable us to get tests out.

Similary on 8037, how do we want to tackle tracing (originally brought up by SamWilsn). We have added `stateGas` and  `stateGasCost` to the EELS tracer alongside `gas` and `gasCost`. Should we update EIP-3155 with this?

--- COMMENT by MariusVanDerWijden at 2026-02-23T13:00:03Z ---
Making cost_per_state_byte a fork constant would kinda defeat the purpose of 8037 tbh.
The whole idea is to be able to change the gas limit independent of hardforks, otherwise we could just go with fixed costs. Maybe a way forward would be to postpone 8037 to devnet-4 to give testing teams more time to update their framework with this new paradigm. But I do think we will use this paradigm more and more in the future, so it makes sense to spend some time to create a good solution for it

I think we should update 3155 with the stateGas and stateGasCost.

--- COMMENT by akashkshirsagar31 at 2026-02-23T13:06:07Z ---
X Stream: https://x.com/i/broadcasts/1yxBeMMWnenJN

--- COMMENT by jtraglia at 2026-02-23T13:49:08Z ---
For consensus specifications, I would like to mention:

* EIP-7805 (FOCIL) specifications have been [promoted](https://github.com/ethereum/consensus-specs/pull/4942) to Heze specifications.

Request core devs review the following PRs *related to Gloas*:

* https://github.com/ethereum/consensus-specs/pull/4843
* https://github.com/ethereum/consensus-specs/pull/4892
* https://github.com/ethereum/consensus-specs/pull/4898
* https://github.com/ethereum/consensus-specs/pull/4918
* https://github.com/ethereum/consensus-specs/pull/4939

Request core devs review the following PRs *unrelated to Gloas*:

* https://github.com/ethereum/consensus-specs/pull/4814
* https://github.com/ethereum/consensus-specs/pull/4902
* https://github.com/ethereum/consensus-specs/pull/4926

PS: I do not intend to give an overview of these PRs during ACDT.

--- COMMENT by bomanaps at 2026-02-23T14:13:40Z ---
Just for more spotlight on this https://hackmd.io/@bomanaps/rJiHuPFOZl

--- COMMENT by github-actions[bot] at 2026-02-24T18:28:34Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
