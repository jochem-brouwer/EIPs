# ISSUE 2144: All Core Devs - Testing (ACDT) #86, July 6, 2026
Created: 2026-06-29T18:04:52Z by jtraglia

### UTC Date & Time

[July 06, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jul-6-2026/2pm)

### Agenda

#### Devnet status updates

* Updates on `glamsterdam-devnet-6`
* Plans for `glamsterdam-devnet-7`

#### Spec updates

* New specification releases asap, hopefully today or tomorrow.
  * https://github.com/ethereum/execution-specs/issues/3034
  * consensus-specs v1.7.0-alpha.12

#### Discussions

* Reminder: SSZ Engine API breakout call 2 is this Friday:
  * https://github.com/ethereum/pm/issues/2145

##### EL breakout

* https://github.com/ethereum/pm/issues/2144#issuecomment-4889177960
  * Led by @nerolation.
  * Clarifying the stipend check in EIP-7928 and https://github.com/ethereum/EIPs/pull/11854
* https://github.com/ethereum/pm/issues/2144#issuecomment-4891741495
  * Led by @barnabasbusa.
  * Discuss two open PRs.
  * https://github.com/ethereum/EIPs/pull/11844
  * https://github.com/ethereum/sys-asm/pull/49

##### CL breakout

* Open PRs to merge before making the release.
  * Led by @jtraglia.
  * https://github.com/ethereum/consensus-specs/pull/4630 
  * https://github.com/ethereum/consensus-specs/pull/5414
  * https://github.com/ethereum/consensus-specs/pull/5429
  * https://github.com/ethereum/consensus-specs/pull/5422 (maybe)
  * https://github.com/ethereum/consensus-specs/pull/5355 (maybe)

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

--- COMMENT by github-actions[bot] at 2026-06-29T18:05:36Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260706%2F20260707&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2386%2C+July+6%2C+2026&dates=20260706T140000Z%2F20260706T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2144)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28890)

--- COMMENT by nerolation at 2026-07-06T04:47:35Z ---
As discussed on ACDE, we agreed on clarifying the stipend check in EIP-7928 and I merged [this PR](https://github.com/ethereum/EIPs/pull/11854). I'd like to give a heads-up. This will be needed as of devnet-7.

--- COMMENT by barnabasbusa at 2026-07-06T10:29:38Z ---
A few items tracked for `glamsterdam-devnet-7` that are not on the agenda yet:

**CL breakout — release candidates:**
- https://github.com/ethereum/consensus-specs/pull/5355 — approved, and it is the fix for https://github.com/ethereum/consensus-specs/issues/5399 (gloas attestations with `data.index == 1` always failing payload matching). If 5414/5429/5422 are going into alpha.12, this should probably be on the list too.

**EL breakout:**
- https://github.com/ethereum/EIPs/pull/11844 — move state-dependent charges to runtime; the remaining open EIPs PR in devnet-7 scope.

**Devnet-7 genesis:**
- https://github.com/ethereum/sys-asm/pull/49 — builder_deposits: disable contract through system call. Merge state should be settled before the devnet-7 genesis bytecode is cut.
