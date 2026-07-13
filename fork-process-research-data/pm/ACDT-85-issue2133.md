# ISSUE 2133: All Core Devs - Testing (ACDT) #85, June 29, 2026
Created: 2026-06-22T15:40:06Z by marioevz

### UTC Date & Time

[June 29, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jun-29-2026/2pm)

### Agenda

#### Devnet status updates

- glamsterdam-devnet-6 status
- glamsterdam-devnet-7 planning

#### Spec updates

- Reduce builder deposit constants on EIP-8282: https://github.com/ethereum/sys-asm/pull/43#discussion_r3470524501, https://github.com/ethereum/EIPs/pull/11760#discussion_r3474446565 

#### Discussions

#### EL Breakout

- https://github.com/ethereum/EIPs/pull/11778

#### CL Breakout

- https://eips.ethereum.org/EIPS/eip-7688

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

--- COMMENT by github-actions[bot] at 2026-06-22T15:40:49Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260629%2F20260630&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2385%2C+June+29%2C+2026&dates=20260629T140000Z%2F20260629T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2133)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28840)

--- COMMENT by rakita at 2026-06-29T08:46:15Z ---
I would like to discuss the proposal to change EIP-2780 that I wrote about in Discord.

Current EIP-2780 have a new mechanism added in case where tx target is not existing account that needs to be created, additional gas is consumed that can cancel the value transfer but it does not invalidate the transaction.

There is additional rule where delegation is always charged as cold so we dont need to fetch the access list or auth list for intrinsic gas calc. 

The proposal is to unify this behaviour with how it behaves inside frame and leverage  mechnanism that could halt transaction (tx is still valid) even before executing.

So if this mechanism is propagated, it would mean:
* Intrinsic gas for authorization list covers: signature recovery and tx bytes.
  * Runtime gas should cover warm/cold load and state creation.
* Intrinsic gas if value is zero: should cover value change (We could even do runtime check for this).
  * Runtime gas should cover: cold loading of target, loading of delegation target (be that warm or cold), and target creationg (if it happens).
  * Intrinsic Tx create, would do similar (init code analysis charge, and warm accesses), and charge gas in rutime if account is not existing etc.

In essence, intrinsic gas represents the minimum gas required for tx to be included. Other charges are incurred at runtime, and these charges can't invalidate the tx.

--- COMMENT by github-actions[bot] at 2026-06-30T18:47:10Z ---
This meeting occurred more than 24 hours ago. Closing automatically.

--- COMMENT by danceratopz at 2026-07-01T03:32:10Z ---
Summary on forkcast: https://forkcast.org/calls/acdt/085/

Next ACDT: 
- https://github.com/ethereum/pm/issues/2144


