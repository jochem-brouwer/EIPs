# ISSUE 1842: All Core Devs - Testing (ACDT) #64, December 15, 2025
Created: 2025-12-12T20:09:46Z by barnabasbusa

### UTC Date & Time

[December 15, 2025, 14:00 UTC](https://savvytime.com/converter/utc/dec-15-2025/3pm)

### Agenda

Fusaka:
- mainnet BPO 1 success 🚀  

Glamsterdam:
- bal-devnet-0/1 updates
- epbs-devnet-0 update

XXM gas topic: 
- Wen 80M gas?

BPO3 preparations: 

Partial responses interop:
- prysm branch
- lighthouse branch
- geth open pr
- nethermind open pr 
- others? 

Max blobs flag:
- nethermind ✅ 
- reth ✅ 
- others ?

H* discussion topics:
- Rename Heka -> Heze (see eth magician [discussion](https://ethereum-magicians.org/t/h-star-name-for-consensus-layer-upgrade-after-glamsterdam/24298/16?u=bbusa) 
- Decide on final name by next ACD?

Holiday schedule:
- No ACDT during holidays
- Last ACDT today/22nd of Dec - do we need another call?
- ACDE instead of ACDT on 5th of Jan @adietrichs 
- Next ACDT 12th of Jan

### Call Series

All Core Devs - Testing


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

60 minutes

### Occurrence Rate

weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Facilitator Emails (Optional)

_No response_

### Display Zoom Link in Calendar Invite (Optional)

- [x] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2025-12-12T20:10:46Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27137)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=JbHnZnkl2Mc)

--- COMMENT by canepat at 2025-12-14T21:02:31Z ---
Tullio from Erigon here, if there's time I would like to give ~10min presentation on RPC testing.

--- COMMENT by leobago at 2025-12-15T09:41:01Z ---
Quick report on H-star decision and portmanteau. 


--- COMMENT by qu0b at 2025-12-15T12:14:06Z ---
[Updated spec](https://notes.ethereum.org/@ethpandaops/bal-devnet-1)

- Client teams should switch branches to bal-devnet-1 and give us a heads up
- Spec 2.0 release includes the type definition changes https://github.com/ethereum/execution-specs/pull/1912 https://github.com/ethereum/execution-specs/pull/1912. We still need to update some references to the old format in the execution-specs esp. regarding RLP encoding.
- The 2.0 execution spec tests release is still tested under bal-devnet-0, but we will switch to bal-devnet-1 once clients have created their branches.
- If possible add a [BAL debug endpoint](https://notes.ethereum.org/@ethpandaops/bal-devnet-1#Besu-debug-endpoint)
- Test your client within kurtosis testnets using the [evm-fuzz](https://notes.ethereum.org/@ethpandaops/bal-devnet-1#Working-configs) spammer
- The following [test](https://github.com/ethereum/execution-specs/pull/1846) regarding self destruct OOG has affected multiple clients that are working on updates. Improved and included [test](https://github.com/ethereum/execution-specs/blob/eips/amsterdam/eip-7928/tests/amsterdam/eip7928_block_level_access_lists/test_block_access_lists_opcodes.py#L1893-L1977)


--- COMMENT by jtraglia at 2025-12-15T13:56:15Z ---
For ePBS, it would be good to discuss the following:

* https://github.com/ethereum/consensus-specs/pull/4777
* https://github.com/ethereum/consensus-specs/pull/4788

Also, please review the following PRs. I want to merge them today.

* https://github.com/ethereum/consensus-specs/pull/4765
* https://github.com/ethereum/consensus-specs/pull/4766

--- COMMENT by abcoathup at 2025-12-16T00:48:44Z ---
> ~10min presentation on RPC testing.

@canepat please share a link to your slides



--- COMMENT by canepat at 2025-12-16T15:44:45Z ---




> > ~10min presentation on RPC testing.
> 
> [@canepat](https://github.com/canepat) please share a link to your slides

Here are the slides:

[EL RPC Standard Testing.pdf](https://github.com/user-attachments/files/24194946/EL.RPC.Standard.Testing.pdf)
