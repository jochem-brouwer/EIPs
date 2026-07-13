# ISSUE 2038: All Core Devs - Testing (ACDT) #79, May 11, 2026
Created: 2026-05-05T09:09:31Z by parithosh

### UTC Date & Time

[May 11, 2026, 14:00 UTC](https://savvytime.com/converter/utc/may-11-2026/2pm)

### Agenda

### Agenda

Agenda
- Summary of interop event
- glamsterdam devnet updates
- spec glamsterdam next devnet
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

--- COMMENT by github-actions[bot] at 2026-05-05T09:10:50Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260511%2F20260512&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2379%2C+May+11%2C+2026&dates=20260511T140000Z%2F20260511T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2038)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28438)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=2MouT9NiGS8)

--- COMMENT by healthykim at 2026-05-05T13:19:50Z ---
I want to talk about including sparse blobpool (EIP-8070) on the next glamsterdam devnet

--- COMMENT by Giulio2002 at 2026-05-07T14:03:55Z ---
I want to get https://github.com/ethereum/EIPs/pull/11540 merged for the next glam devnet

--- COMMENT by marioevz at 2026-05-08T13:25:38Z ---
We should discuss adding https://eips.ethereum.org/EIPS/eip-8246 to this next devnet. It was discussed and generally well received during last ACD-E, and greatly improves EIP-7708.

--- COMMENT by barnabasbusa at 2026-05-08T15:00:20Z ---
we should discuss this: https://github.com/ethereum/consensus-specs/pull/5223

--- COMMENT by barnabasbusa at 2026-05-08T15:03:36Z ---
> I want to talk about including sparse blobpool (EIP-8070) on the next glamsterdam devnet

I think this will have to wait potentially post glamsterdam unfortunately, as there are still many clients that are missing [partial cells implementation](https://eips.ethereum.org/EIPS/eip-8136)  on the CL side, which would be a pre-requisite for this EIP. 
Also if you can confirm that we need [EIP 8136](https://eips.ethereum.org/EIPS/eip-8136) for EIP 8070, then it would be good to mark it as a prerequisite in that EIP. 

--- COMMENT by healthykim at 2026-05-11T08:11:14Z ---
> Also if you can confirm that we need [EIP 8136](https://eips.ethereum.org/EIPS/eip-8136) for EIP 8070, then it would be good to mark it as a prerequisite in that EIP.

Technically EIP 8136 is not required for EIP 8070. This is almost EL only changes with two engine API updates. So it should be fine regardless partial cells implementation

--- COMMENT by nerolation at 2026-05-11T08:19:27Z ---
I want to quickly bring up this PR:
https://github.com/ethereum/execution-apis/pull/794

It changes the error code, fixes the uint64 -> uint32 for BAL's `BlockAccessIndex`, and introduces a new rpc method under the debug namespace, `debug_getRawBlockAccessList`, returning the RLP-encoded BAL.

--- COMMENT by misilva73 at 2026-05-11T12:07:54Z ---
I would like to discuss the following two PR's to EIP-8037:

- [Update EIP-8037: Update parameters and rationale section EIPs#11616](https://github.com/ethereum/EIPs/pull/11616)
- [Update EIP-8037: Fix bugs from bal-devnet-6 EIPs#11611](https://github.com/ethereum/EIPs/pull/11611)

After we agree on this, we can finalize the bal-devnet-7 spec.

--- COMMENT by barnabasbusa at 2026-05-11T12:09:55Z ---
We should also discuss https://github.com/ethereum/consensus-specs/pull/5224

--- COMMENT by nflaig at 2026-05-11T12:19:31Z ---
https://github.com/ethereum/execution-apis/pull/608 any EL devs opposed to having the CL decide which gas limit to use for local blocks? The tl;dr why this is useful is 1) only requires users to configure the gas limit on the CL side and 2) allows us to use vanilla ELs as builders, see [comment](https://github.com/ethereum/execution-apis/pull/608#issuecomment-3798868091)

**Edit:** updated in https://github.com/ethereum/execution-apis/pull/796

--- COMMENT by barnabasbusa at 2026-05-11T12:48:11Z ---
Would any clients oppose to set a max limit for the desried gas limit? e.g if we aim to hit 200m for glamsterdam but we do find some issues and we see that we could use 175M comfortably, then we should probably have an check where the user can override the default gas limit up till a max cap of 175 or whatever we deem safe? Similar idea to [EIP-3756.](https://eips.ethereum.org/EIPS/eip-3756). 

--- COMMENT by LukaszRozmej at 2026-05-11T13:15:13Z ---
Quick info from Nethermind: We merged [REST SSZ EngineAPI](https://github.com/ethereum/execution-apis/pull/764) with current spec into master. Our internal benchmarks show 5-10x improvement over JSON RPC. We are happy to collaborate with other CL's. Prysm currently has implementation of outdated spec.

--- COMMENT by nerolation at 2026-05-11T13:19:42Z ---
Almost forgot, we also wanted to discuss SFI'ing eth/70 and eth/71.

--- COMMENT by bharath-123 at 2026-05-11T13:48:59Z ---
In the consensus spec, we check if bid.gas_limit == pref.gas_limit here: https://github.com/ethereum/consensus-specs/blob/master/specs/gloas/p2p-interface.md?plain=1#L350

This is not right and we might want to revisit this. We either update the check to consider the parent gas limit or we evaluate whether we want to have such checks in CL at all

--- COMMENT by qu0b at 2026-05-11T13:49:27Z ---
Initial bal-devnet-7 spec sheet https://notes.ethereum.org/@ethpandaops/bal-devnet-7 

--- COMMENT by github-actions[bot] at 2026-05-12T18:47:11Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
