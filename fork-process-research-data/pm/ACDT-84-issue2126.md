# ISSUE 2126: All Core Devs - Testing (ACDT) #84, June 22, 2026
Created: 2026-06-16T14:36:20Z by jtraglia

### UTC Date & Time

[June 22, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jun-22-2026/2pm)

### Agenda

#### Devnet status updates

* `bal-devnet-7`
* `glamsterdam-devnet-5`
* `glamsterdam-devnet-6`

#### Spec updates

New releases:

* [tests-glamsterdam-devnet@v6.0.0](https://github.com/ethereum/execution-specs/releases/tag/tests-glamsterdam-devnet@v6.0.0)
* [consensus-specs v1.7.0-alpha.11](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.11)

#### Discussions

* https://github.com/ethereum/pm/issues/2126#issuecomment-4768081626
  * Proposal to include EIP-7688 in `glamsterdam-devnet-7`.
  * Led by @tersec.

> [!NOTE]
> This week's ACDC call will be at [14:00 UTC](https://savvytime.com/converter/utc/june-25-2026/2pm), which is different than last time.

> [!NOTE]
> There is an REST-SSZ Engine API breakout call on [June 26, 2026, 13:00 UTC](https://savvytime.com/converter/utc/jun-26-2026/1pm)!
> * https://github.com/ethereum/pm/issues/2132

#### EL breakout

* https://github.com/ethereum/pm/issues/2126#issuecomment-4766792425
  * A bug in EIP-8038.
  * Led by @misilva73.
* https://github.com/ethereum/pm/issues/2126#issuecomment-4767748307
  * Can we retire `bal-devnet-3`?
  * Led by @qu0b.
* https://github.com/ethereum/pm/issues/2126#issuecomment-4768232950
  * EIP-7928 BAL × 7702 warming decision.
  * Led by @qu0b.
* https://github.com/ethereum/pm/issues/2126#issuecomment-4768515382
  * Question about closing https://github.com/ethereum/EIPs/pull/11778.
  * Led by @lu-pinto.

#### CL breakout

* https://github.com/ethereum/pm/issues/2126#issuecomment-4750367809
  * What do we think about these two beacon-apis changes? ([580](https://github.com/ethereum/beacon-APIs/pull/580) & [608](https://github.com/ethereum/beacon-APIs/pull/608))
  * Led by @bharath-123.

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

--- COMMENT by github-actions[bot] at 2026-06-16T14:37:25Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260622%2F20260623&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2384%2C+June+22%2C+2026&dates=20260622T140000Z%2F20260622T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2126)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28807)

--- COMMENT by bharath-123 at 2026-06-19T09:37:11Z ---
https://github.com/ethereum/beacon-APIs/pull/580 want to understand if there are any blockers with merging this PR. It would be great to have the standard stateless envelope publishing api merged to main so that all clients can implement it.
And also:  https://github.com/ethereum/beacon-APIs/pull/608

--- COMMENT by misilva73 at 2026-06-22T09:22:57Z ---
I would like to bring a bug in 8038 raised by @Helkomine in [eth magicians](https://ethereum-magicians.org/t/eip-8038-state-access-gas-cost-update/25693/19).  I update the EIP [here](https://github.com/ethereum/EIPs/pull/11823), but this will likely require a change in specs. It would also be nice to add testing for this edge case.

--- COMMENT by qu0b at 2026-06-22T11:24:47Z ---
Anyone still using bal-devnet-3 or can we retire it?

--- COMMENT by tersec at 2026-06-22T12:06:55Z ---
I would like to discuss inclusion of [EIP-7688](https://eips.ethereum.org/EIPS/eip-7688) in `devnet-7`.

--- COMMENT by qu0b at 2026-06-22T12:23:20Z ---
EIP-7928 BAL × 7702 warming decision

--- COMMENT by lu-pinto at 2026-06-22T12:57:00Z ---
Can we merge this PR about 8037 and EIP-7702 interaction? https://github.com/ethereum/EIPs/pull/11778 ?

--- COMMENT by RazorClient at 2026-06-22T13:26:33Z ---
SSZ engine api calls kicking out this week friday 
https://github.com/ethereum/pm/issues/2132
