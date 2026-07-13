# ISSUE 1883: All Core Devs - Execution (ACDE) #229, Jan 29, 2026

### UTC Date & Time

[January 29, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jan-29-2025/2pm)

### Agenda

- Housekeeping
  - Breakout Calls
    - [Post Quantum transaction signature (PQTS)](https://github.com/ethereum/pm/issues/1889)
    - [L1-zkEVM](https://github.com/ethereum/pm/issues/1900)
    - [Glamsterdam Repricings](https://github.com/ethereum/pm/issues/1883#issuecomment-3817541545)
- Glamsterdam
  - devnet-2 updates
  - BAL client optimizations (parallel execution, batch reads, parallel state root calculation, sync)
    - update by @nerolation
    - repricings context by @misilva73
  - CFI EIP priorities for future devnets
  - scoping: PFI EIPs without protocol changes
    - [EIP-7610: Revert creation in case of non-empty storage](https://eips.ethereum.org/EIPS/eip-7610)
    - [EIP-7872: Max blob flag for local builders](https://eips.ethereum.org/EIPS/eip-7872)
    - [EIP-7949: Genesis File Format](https://eips.ethereum.org/EIPS/eip-7949)
- New EIPs ([presentation](https://drive.google.com/file/d/1qbYfroCds8_DCpVfzM1qm2HKjYLDntu9/view?usp=sharing))
  - [EIP-8077: eth/XX - announce transactions with nonce](https://eips.ethereum.org/EIPS/eip-8077)
  - [EIP-8094: eth/vhash - Blob-Aware Mempool](https://eips.ethereum.org/EIPS/eip-8094)
- Hegota
  - Headliner Proposal Presentations
    - [Universal Enshrined Encrypted Mempool (EEM)](https://ethereum-magicians.org/t/hegota-headliner-proposal-eip-8105-universal-enshrined-encrypted-mempool-eem/27448) by @jannikluhn
    - [Frame Transactions](https://ethereum-magicians.org/t/hegota-headliner-proposal-frame-transaction/27618) by @lightclient and @fjl
      - [context](https://github.com/ethereum/pm/issues/1883#issuecomment-3815340368)
    - [SSZ execution blocks](https://ethereum-magicians.org/t/hegota-headliner-proposal-ssz-execution-blocks/27619) by @etan-status
    - see [next ACDC](https://github.com/ethereum/pm/issues/1907) for [FOCIL](https://ethereum-magicians.org/t/hegota-headliner-proposal-focil-eip-7805/27604)

### Call Series

All Core Devs - Execution


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

90 minutes

### Occurrence Rate

bi-weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Facilitator Emails (Optional)

_No response_

### Display Zoom Link in Calendar Invite (Optional)

- [ ] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>


===== COMMENTS =====

--- github-actions[bot] 2026-01-19T20:52:38Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27513)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=4t_YTAbrH4o)

--- nixorokish 2026-01-22T17:58:19Z ---
there are three remaining EL EIPs in 7773 PFIs (7610, 7872, 7949) - they're all non-protocol EIPs but it should be explicit whether we're going to DFI them or include

--- jannikluhn 2026-01-23T18:03:00Z ---
I'd like to apply to give a 2 minute overview over [EIP-8105](https://ethereum-magicians.org/t/hegota-headliner-proposal-eip-8105-universal-enshrined-encrypted-mempool-eem/27448) as a potential headliner for Hegotá, in accordance with the [scoping process](https://ethereum-magicians.org/t/eip-8081-hegota-network-upgrade-meta-thread/26876).

--- akashkshirsagar31 2026-01-26T08:52:29Z ---
X Stream: https://x.com/i/broadcasts/1BRJjgZqYVpxw

--- asanso 2026-01-27T08:16:35Z ---
I was wondering if it would be possible to have 1 minute in order to introduce the new Post Quantum transaction signature (PQTS) Breakout Room https://github.com/ethereum/pm/issues/1889

--- lightclient 2026-01-29T03:55:19Z ---
@fjl and I would like to introduce our headliner proposal for Hegota: [Frame Transactions](https://ethereum-magicians.org/t/hegota-headliner-proposal-frame-transaction/27618). You can read more about it in the link, but ultimately we need get serious about providing an off-ramp from ECDSA to PQ crypto algorithms. There is a lot of active work on figuring out *what* those crypto algorithms will be specifically, but also need to figure out how the protocol is going to integrate it. This is the perfect time to bring account abstraction to the protocol.

It's also important to note: this is a generalization of EIP-7701, which builds on years of experience from the ERC-4337 network. While the EIP is new, the ideas the EIP is constructed on are old and already live on the 4337 network.

--- abcoathup 2026-01-29T04:41:38Z ---
## Hegotá headliner candidates

| EIP | Eth Magicians proposal | ACD presentation |
|-----|-----------|---------------------|
| [EIP-7805](https://forkcast.org/eips/7805): FOCIL | [proposal](https://ethereum-magicians.org/t/hegota-headliner-proposal-focil-eip-7805/27604) | ACDE 229 (on agenda) |
| [EIP-8105](https://github.com/ethereum/EIPs/pull/10943/changes): Encrypted transaction pool | [proposal](https://ethereum-magicians.org/t/hegota-headliner-proposal-eip-8105-universal-enshrined-encrypted-mempool-eem/27448) | ACDE 229 (on agenda) |
| [EIP-8141](https://eips.ethereum.org/EIPS/eip-8141): Frame transaction | [proposal](https://ethereum-magicians.org/t/hegota-headliner-proposal-frame-transaction/27618) | ACDE 229 (on agenda) |
| [EIP-7807](https://eips.ethereum.org/EIPS/eip-7807): SSZ execution blocks | [proposal](https://ethereum-magicians.org/t/hegota-headliner-proposal-ssz-execution-blocks/27619) | ACDE 229 (on agenda) |

--- cskiraly 2026-01-29T09:02:01Z ---
This has been scheduled several times, but time always run out before I was able to introduce the following two EL networking EIPs.

I would like to quickly introduce them this time. They are not fork-specific (hence I wasn't proposing them on the Glamsterdam list) but provide mempool improvements that would be useful for scaling.

- [EIP-8077](https://eips.ethereum.org/EIPS/eip-8077) proposes to change the way we propagate transactions in the mempool, improving efficiency and preparing for a future where not everyone has the capacity to receive all transactions.
- [EIP-8094](https://eips.ethereum.org/EIPS/eip-8094) aims to make RBF (replace by fee) much more efficient, eliminating the cost of redistributing blob content if only the metadata (fees) of a transaction are updated.

--- soispoke 2026-01-29T11:14:29Z ---
Since this ACDE call is the last one before the headliner proposal deadline (https://ethereum-magicians.org/t/eip-8081-hegota-network-upgrade-meta-thread/26876), we would like to take a minute to propose/present FOCIL as a CL headliner if it's the right time for it, process wise.

It might make more sense to present it during the next ACDC call next week but in practice it would be after the deadline, so we wanted to make sure this wouldn't be an issue. 


--- etan-status 2026-01-29T11:33:08Z ---
Would like to give a brief overview of EIP-7807 SSZ execution blocks [headliner proposal](https://ethereum-magicians.org/t/hegota-headliner-proposal-ssz-execution-blocks/27619).

[7807-h.pdf](https://github.com/user-attachments/files/24941000/7807-h.pdf)

--- kevaundray 2026-01-29T12:48:55Z ---
Hi, would be great to have a minute to introduce a new breakout call: https://github.com/ethereum/pm/issues/1900

--- misilva73 2026-01-29T12:59:47Z ---
I would like to introduce the new breakout call for Glamsterdam Repricings. It will be a biweekly call, Wednesday, 14:00 UTC. The first call will be next week, Feb. 4th.

--- poojaranjan 2026-01-29T13:49:15Z ---
Several **Fusaka-related EIPs** are still waiting on author responses:
- [EIP-7723: Network Upgrade Inclusion Stages](https://eips.ethereum.org/EIPS/eip-7723) is currently blocking progress on [EIP-7607](https://eips.ethereum.org/EIPS/eip-7607). Ref. Open [PRs](https://github.com/ethereum/EIPs/pulls?q=is%3Apr+is%3Aopen+7723)
- [EIP-7594: PeerDAS - Peer Data Availability Sampling](https://eips.ethereum.org/EIPS/eip-7594) has pending items that need author follow-up. Ref. [PR](https://github.com/ethereum/EIPs/pulls?q=is%3Apr+is%3Aopen+7594)
- [EIP-7918: Blob base fee bounded by execution cost](https://eips.ethereum.org/EIPS/eip-7918) depends on EIP-7594. Sam Wilson has a [draft PR](https://github.com/ethereum/EIPs/pull/10935) prepared to help unblock this, but the author of EIP-7594 needs to clear the existing PR backlog before it can move forward.

Request authors to kindly closethem at the earliest. 

--- github-actions[bot] 2026-01-30T18:18:27Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
