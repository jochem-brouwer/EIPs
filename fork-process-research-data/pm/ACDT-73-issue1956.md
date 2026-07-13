# ISSUE 1956: All Core Devs - Testing (ACDT) #73, March 9, 2026
Created: 2026-03-03T09:49:33Z by danceratopz

### UTC Date & Time

[March 09, 2026, 14:00 UTC](https://savvytime.com/converter/utc/mar-9-2026/2pm)

### Agenda

#### Fusaka

##### blob-devnet-0

[spec&config](https://notes.ethereum.org/@ethpandaops/blob-devnet-0) · [dora](https://dora.blob-devnet-0.ethpandaops.io) · [explorer](https://explorer.blob-devnet-0.ethpandaops.io) · [forkmon](https://forkmon.blob-devnet-0.ethpandaops.io)

- Engine API serialization bottleneck (Teku+Geth), SSZ engine API discussion: [execution-apis#764](https://github.com/ethereum/execution-apis/pull/764), [Eth R&D discussion](https://discord.com/channels/595666850260713488/745077610685661265/1478875962027544659).
- eth69 debugging resolved (was mainnet peers).

#### Glamsterdam

##### bal-devnet-2

[spec&config](https://notes.ethereum.org/@ethpandaops/bal-devnet-2) · [dora](https://dora.bal-devnet-2.ethpandaops.io) · [forkmon](https://forkmon.bal-devnet-2.ethpandaops.io)

- Winding down, some known Besu/Nimbus issues.

##### bal-devnet-3

[spec&config](https://notes.ethereum.org/@ethpandaops/bal-devnet-3) · not yet launched; explorers will be at `*.bal-devnet-3.ethpandaops.io`.

- Client readiness / [EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) cross-client bug hunt, @qu0b [go-ethereum#33972](https://github.com/ethereum/go-ethereum/pull/33972), [besu#9994](https://github.com/hyperledger/besu/pull/9994), [nimbus-eth1#4036](https://github.com/status-im/nimbus-eth1/pull/4036)
  - Increased EIP-8037 coverage, @benaadams [execution-specs#2426](https://github.com/ethereum/execution-specs/issues/2426).
  - -> New EL test release @spencer-tb [bal@v5.3.0](https://github.com/ethereum/execution-spec-tests/releases/tag/bal@v5.3.0).
    - New: Also includes all tests filled for Osaka (and use the [new directory layout](https://github.com/ethereum/pm/issues/1948#issuecomment-3971527094)).
- When launch? > 2026-03-11?

##### epbs-devnet-0

[spec&config](https://notes.ethereum.org/@ethpandaops/epbs-devnet-0) · [dora](https://dora.epbs-devnet-0.ethpandaops.io) · [explorer](https://explorer.epbs-devnet-0.ethpandaops.io) · [forkmon](https://forkmon.epbs-devnet-0.ethpandaops.io)

- Launch update, Prysm/LH/Lodestar running, sync challenges, [prysm#16449](https://github.com/OffchainLabs/prysm/pull/16449).
  - Blog entry on initial devnet-0 challenges https://www.potuz.net/posts/epbs-devnet-0/.
- Teku/nimbus not running on devnet yet, planned later.
- ePBS forcing the EL to trigger block production on older heads.
- The issue with PTC lookahead: [ethereum/consensus-specs#4979](https://github.com/ethereum/consensus-specs/pull/4979).

##### Other Glamsterdam

- snap/2: BAL-based state healing proposal (Toni/@nerolation, Gary/@rjl493456442) — [EIP draft](https://github.com/nerolation/EIPs/blob/toni/snap-2/EIPS/eip-9999.md).

#### Gas limit / eth/70 / eth/71

- eth/70 implementation update (carry forward).
- eth/71 (EIP-8159): Clarify response size cap (10 MiB → 2 MiB soft cap), [EIPs#11387](https://github.com/ethereum/EIPs/pull/11387) (@nerolation).

#### State bloat

##### perf-devnet-3

[dora](https://dora.perf-devnet-3.ethpandaops.io) · [explorer](https://explorer.perf-devnet-3.ethpandaops.io) · [forkmon](https://forkmon.perf-devnet-3.ethpandaops.io)

- Infrastructure issues, Nethermind/Reth/Erigon, snapshot timeline, [reth#22721](https://github.com/paradigmxyz/reth/pull/22721). Geth stable at correct height. 
- 50 GB ERC20 complete, EOA bloating in progress...
- Need all clients synced for snapshot.

#### Meta

- ACDT call format/cadence discussion (biweekly proposal) — [Eth R&D thread](https://discord.com/channels/595666850260713488/1478010793042903101/1478022347146662059).

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

--- COMMENT by github-actions[bot] at 2026-03-03T09:50:39Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27883)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=9MxhnFm3CFM)

--- COMMENT by nerolation at 2026-03-08T16:12:51Z ---
I'd like to quickly talk about snap/2: BAL-based State Healing, described [here](https://github.com/nerolation/EIPs/blob/toni/snap-2/EIPS/eip-9999.md).


--- COMMENT by nerolation at 2026-03-09T10:24:35Z ---
Based on feedback from Bosul, I've prepared a draft PR for eth/71, updating the cap of 10 MiB to having a soft cap of 2 MiB in consistency with other responses (e.g. blocks, receipts, headers):
https://github.com/ethereum/EIPs/pull/11387

--- COMMENT by akashkshirsagar31 at 2026-03-09T13:36:02Z ---
X Stream: https://x.com/i/broadcasts/1OxwblLVBpgJB

--- COMMENT by qu0b at 2026-03-09T13:51:35Z ---
- Deployed and syncing dedicated benchmarking nodes for bal-devnet-2 (geth besu): https://github.com/ethpandaops/bal-devnets/pull/29
- reth & nethermind & etherex update on bal benchmarking flags (prefetch, sequential, all optimizations)?
- What is the status on local BAL benchmarking e.g. [https://github.com/ethereum/execution-specs/pull/2033](https://github.com/ethereum/execution-specs/pull/2033/changes)
- bal-devnet-3 local testing ongoing

--- COMMENT by potuz at 2026-03-09T14:02:13Z ---
Want to add two topics of discussion
- ePBS forcing the EL to trigger block production on older heads
- The issue with PTC lookahead. 

--- COMMENT by github-actions[bot] at 2026-03-10T18:25:13Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
