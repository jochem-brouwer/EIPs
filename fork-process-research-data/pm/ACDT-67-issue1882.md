# ISSUE 1882: All Core Devs - Testing (ACDT) #67, January 26, 2026
Created: 2026-01-19T19:44:55Z by marioevz

### UTC Date & Time

[January 26, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jan-26-2026/2pm)

### Agenda

#### Glamsterdam

- bal-devnet-2 updates
  - [spec reference tests release](https://github.com/ethereum/execution-spec-tests/releases/tag/bal%40v4.0.0)
  - Progress update by @qu0b: https://github.com/ethereum/pm/issues/1882#issuecomment-3798678466
  - [EIP-7778 Discussion](https://github.com/ethereum/pm/issues/1882#issuecomment-3793080772)
- epbs-devnet-0 update
  - Standardized ePBS Beacon API: [Comment](https://github.com/ethereum/pm/issues/1882#issuecomment-3798965040), [PR Link](https://github.com/ethereum/beacon-APIs/pull/552)

#### Gas Benchmarking Updates

- Updates

#### Process

- [CFI -> Devnet Process Discussion](https://github.com/ethereum/pm/issues/1882#issuecomment-3791951548)

#### Others

- [BPO Meta EIPs](https://github.com/ethereum/pm/issues/1882#issuecomment-3795075740)

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

--- COMMENT by github-actions[bot] at 2026-01-19T19:45:44Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27511)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=EUhKZYGRjBw)

--- COMMENT by marioevz at 2026-01-23T19:25:44Z ---
I’d like to allocate some time during the call to discuss process, specifically the CFI -> Devnet process for future devnets, because there seems to be room for improvement.

A possible process could look as follows:
- EIPs proposed for inclusion in the next devnet should be posted to the ACD-T agenda ahead of time, to give client teams time to review, raise objections, or suggest alternatives.
- During the call, if there are no objections, the devnet plan moves forward.
- If issues are discovered only after implementation has started, an EIP can still be removed and deferred to a future devnet.
- Decisions around CFI -> SFI or CFI -> DFI will **_not_** be discussed in ACD-T and should remain within ACD-E, ACD-C.

--- COMMENT by fselmo at 2026-01-24T00:07:55Z ---
We need to reach consensus on EIP-7778 state for `bal-devnet-2` (spec freeze). EELS spec and tests implementations were finished and [bal@v4.0.0](https://github.com/ethereum/execution-spec-tests/releases/tag/bal%40v4.0.0) tests were released with support for all EIPs in the devnet spec. However, this includes the `gasSpent` field on transaction receipts, as that is the current spec. If this changes, it should be a quick update on our side and we can get a new test release out. But this complicates things as this is a consensus issue on receipts roots so no client can test against this latest test release.

Let's reach a consensus on a spec freeze for `bal-devnet-2`.

--- COMMENT by nerolation at 2026-01-24T03:09:42Z ---
Related discussion for 7778 here:

https://discord.com/channels/595666850260713488/1460715235437580369/1463837761982300288


--- COMMENT by poojaranjan at 2026-01-24T16:55:55Z ---
I’d like to surface the PRs below and am happy to answer any questions.

- [Add BPO Folder](https://github.com/ethereum/pm/pull/1894) to ethereum/pm repo
- [Add Hardfork Meta - BPO1](https://github.com/ethereum/EIPs/pull/11164)
- [Add Hardfork Meta - BPO2](https://github.com/ethereum/EIPs/pull/11165)

Feedback and suggestions on structure, fields, and long-term maintainability are very welcome.

--- COMMENT by akashkshirsagar31 at 2026-01-26T08:49:41Z ---
X Stream: https://x.com/i/broadcasts/1djxXWlRomyJZ

--- COMMENT by qu0b at 2026-01-26T09:41:47Z ---
1. I propose to discuss these BAL spec clarifications in the ACDT call instead of the BAL breakout call. I believe most of these will be decided swiftly, if there is need for further discussion we can move that to the breakout call:

- https://github.com/ethereum/execution-apis/pull/727
- https://github.com/ethereum/EIPs/pull/11066
- https://github.com/ethereum/execution-apis/pull/726

the following PR is also still open:

- https://github.com/ethereum/execution-apis/pull/731

2. Current `bal-devnet-2` implementation status as per my integration testing on bal-devnet-2 branches:

 EIP | Geth | Besu | Reth | Nethermind | Erigon | Nimbus-EL |
|-----|:----:|:----:|:----:|:----------:|:------:|:---------:|
| 7928 (BAL) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7708 (ETH Logs) | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| 7778 (Gas Refunds) | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| 7843 (SLOTNUM) | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| 8024 (SWAPN/DUPN) | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

Most kurtosis integration testing is currently blocked because apart from geth no EL client has merged the SLOTNUM EIP, which is a breaking change at the gloas fork.

3. Please add a flag to enable / disable BAL optimizations as this is currently a blocker for gas repricing.

4. lastly could the testing team give an update on how they have decided to get the BAL for their testing since its no longer in the block body ref: https://github.com/ethereum/execution-specs/issues/1938

5. Lets get `bal-devnet-2` up and running by Wednesday 28.01

--- COMMENT by nflaig at 2026-01-26T10:50:35Z ---
Would like to get more eyes on https://github.com/ethereum/beacon-APIs/pull/552, so we can merge it soon.

The [main concern](https://github.com/ethereum/beacon-APIs/pull/552#discussion_r2642624992) right now is that it makes the local block production flow stateful, meaning the beacon node which produced the `BeaconBlock` is the only one right now that would be able to produce the `ExecutionPayloadEnvelope` in the second step/api request.

There are 3 different paths forward that I can see
1) we keep spec as is, `SignedBeaconBlock` can still be published to all connected beacon node from validator client but `ExecutionPayloadEnvelope` can only be produced by beacon node that produced `BeaconBlock` and `SignedExecutionPayloadEnvelope` can also only be published via that node since blobs are no passed around and need to be cached
2) we still keep the 2 step process but allow in both publish steps to broadcast to all connected beacon nodes, this is almost same to 1) but instead of `ExecutionPayloadEnvelope` we would return `BlockContents` which also includes `blobs` and allows to publish `SignedExecutionPayloadEnvelope` via all connected beacon nodes
3) this is most similar to current flow, we return all data (`BeaconBlock`, `ExecutionPayloadEnvelope` and `blobs`) from `produceBlockV4` as proposed [here](https://github.com/shane-moore/beacon-APIs/pull/2). This allows to publish to any beacon node irrespective of which node produced the block but requires to pass all data around which especially with higher blob counts will become a bottleneck (see [previous discussion](https://github.com/ethereum/beacon-APIs/issues/519))

Happy to further clarify during the call but feedback can be provided async, ideally on the PR or #apis channel on discord

--- COMMENT by github-actions[bot] at 2026-01-28T15:42:41Z ---
This meeting occurred more than 24 hours ago. Closing automatically. Reopen if further discussion is needed.
