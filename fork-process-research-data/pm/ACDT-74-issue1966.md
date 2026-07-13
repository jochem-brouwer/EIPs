# ISSUE 1966: All Core Devs - Testing (ACDT) #74, March 16, 2026
Created: 2026-03-10T14:58:12Z by parithosh

### UTC Date & Time

[March 16, 2026, 14:00 UTC](https://savvytime.com/converter/utc/mar-16-2026/2pm)

### Agenda

Note: This issue is to gain feedback on if there are any emergency topics to discuss. If no compelling topics are proposed in this issue, we would propose to cancel the call in favor of a breakout room (TBD at ACD). 
Update: Call not cancelled.

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

--- COMMENT by github-actions[bot] at 2026-03-12T13:47:22Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27971)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=Sr4osj2u4-U)

--- COMMENT by spencer-tb at 2026-03-12T15:39:18Z ---
Forwarding from ACDE's last call as we didn't get to it!

Can we SFI all the EIPs included on the devnets following: https://eips.ethereum.org/EIPS/eip-7723
Otherwise should we change the devnet/SFI section to align with the process currently? Note SFI'd EIPs can still be DFI'd.

PR to update for the latter: https://github.com/ethereum/EIPs/pull/11399

Thanks! 🫡

--- COMMENT by nerolation at 2026-03-16T08:44:58Z ---
Agree with @spencer-tb - let's SFI some of our CFI'd EIPs where we're certain that they'll be shipped.

We must be intentional about SFI decissions though (**it should not become "the process" that included-in-a-devnet means = SFI**, otherwise we must define in which order we put EIPs on a devent (e.g. based on [client prios](https://forkcast.org/priority), or [community feedback](https://x.com/EFprotocol/status/2031056150427242892)), which isn''t great neither). 

I think we can be a bit pragmatic with *when* things land on devnets but the **SFI decission should be explicit**. As spencer notes, on-a-devnet can still be DFI'd.

--- COMMENT by parithosh at 2026-03-16T10:55:03Z ---
I think we should keep SFI decisions out of ACDT unless explicitly mentioned on ACD. The risk I see of SFI-ing whatever is on devnets is that we then make the term SFI less strong, an EIP is added to devnets mostly based on convenience of sequencing and testing - usually not based on importance. If there's an "easy" EIP that we add in and SFI, but it interacts poorly later with a more important EIP then we have to undo the SFI for no real gain or visibility into what ACD considers more or less important. 

I think the better process is for ACD to get an update on EIPs that have gone in, once we are sure there are no missing interactions we can SFI them in batches. If there's some oversight on my part for why SFI-ing early makes sense, then please do bring it up, ill add it as an agenda item on the call for today.

--- COMMENT by nerolation at 2026-03-16T11:19:20Z ---
For the EIP-7928 (BALs) part of the breakout, I'd like to discuss the following agenda items:
* BAL optimization status (focus on I/O benchmarking)
* [eth/71](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-8159.md) implementation status 
* [snap/2](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-8189.md) intro + proposal from @mirgee to **not reuse* message IDs from snap/1 but instead define new ones
  * see: https://github.com/ethereum/EIPs/pull/11391#discussion_r2939292215

--- COMMENT by nflaig at 2026-03-16T11:29:20Z ---
Would like to get these PRs merged (epbs related)
- https://github.com/ethereum/beacon-APIs/pull/586
- https://github.com/ethereum/beacon-APIs/pull/587
- https://github.com/ethereum/beacon-APIs/pull/588

No need to discuss, just for people to take a look when they have time

--- COMMENT by akashkshirsagar31 at 2026-03-16T13:04:29Z ---
X Stream: https://x.com/i/broadcasts/1NGaraDlnVlJj

--- COMMENT by github-actions[bot] at 2026-03-17T18:28:08Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
