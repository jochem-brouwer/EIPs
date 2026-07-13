# ISSUE 1769: All Core Devs - Testing (ACDT) #58, Oct 20, 2025

### UTC Date & Time

Oct 20, 2025, 14:00 UTC

### Agenda

#### Fusaka:
- Fusaka Holesky BPO fork updates
- Fusaka devnet status updates

#### Gas limit testing update:
- 60M gas limit on mainnet updates
- State test updates

#### Glamsterdam Testing Updates:
- BALer updates
- ePBS updates

#### Other Topics
- RPC updates

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

parithosh@ethereum.org

### Display Zoom Link in Calendar Invite (Optional)

- [x] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>


===== COMMENTS =====

--- github-actions[bot] 2025-10-17T14:40:15Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/25859)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=5qqcQaAet2o)

--- akashkshirsagar31 2025-10-20T09:23:49Z ---
X Stream: https://x.com/i/broadcasts/1MnGnPyNZVNxO

--- marcindsobczak 2025-10-20T13:04:29Z ---
In the context of [EIP-7823](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7823.md) and [EIP-7883](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7883.md) we have analyzed current mainnet blocks (which use the Pectra spec) against the Fusaka spec and found a failing pattern related to Account Abstraction contracts.

In Fusaka, EIP-7883 significantly increases the price of the MODEXP precompile. In some transactions calling the `EntryPoint v0.6.0 contract`, we are seeing the error message: `AA40 over verificationGasLimit`.

It's likely caused by underpriced user operations failing at the verification stage. We suspect that this won't be an issue after the actual hardfork, as user operations' gas limits will likely be estimated higher. Interestingly, we didn't notice any issues with contract versions `0.7.0` or `0.8.0` - only `v0.6.0` seems to be affected.

This was the only issue found across ~98k scanned blocks, appearing roughly once per hour.

--- qu0b 2025-10-20T13:20:50Z ---
Glamsterdam:
- Status of FOCIL regarding devnet 0
- Client status on BAL - besu, geth seem ready, nethermind nearly ready, reth no idea, erigon work in progress.
- Status of epbs
