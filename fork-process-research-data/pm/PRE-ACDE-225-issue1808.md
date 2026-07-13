# ISSUE 1808: All Core Devs - Execution (ACDE) #225, Dec 4, 2025

### UTC Date & Time

[December 4, 2025, 14:00 UTC](https://savvytime.com/converter/utc/dec-04-2025/2pm)
RESCHEDULED from November 20th. From Ansgar:

> Hi everyone, after speaking with several core devs here in Buenos Aires, we decided to cancel ACDE this week.
> 
> Reasons:
> more core devs here than initially expected
> conflicting events for many of the people
> issues with finding reliable internet for calls
> 
> Overall the concern was that we would likely not have enough participation on the call to make CFI / DFI decisions.
> 
> Next ACDE will be Dec 4.

### Agenda

- Fusaka
  - [incidents following the fork](https://notes.ethereum.org/Qcl9AZy6SiWAoTCSwDUTJg)
  - current status
- Housekeeping (urgent)
  - ACD timings
    - [ACDC Dec 25: canceled](https://github.com/ethereum/pm/issues/1808#issuecomment-3599522113)
    - [ACDE Jan 01: keep?](https://github.com/ethereum/pm/issues/1808#issuecomment-3599394973)
  - [FOCIL: status for Heka / Bogota?](https://github.com/ethereum/pm/issues/1808#issuecomment-3599524331)
- Glamsterdam
  - [CFI / DFI candidate EIPs](https://notes.ethereum.org/@ansgar/glamsterdam-el-pfi-eips)
  - Open Questions
    - Repricing: Core / Gas
      - unbundle / regroup?
    - Repricing: Core / State Growth
      - which approach?
      - [writeup](https://notes.ethereum.org/@anderselowsson/3-paradigms-for-state-creation-repricing) by @anderselowsson
    - Contracts / Size
      - which approach?
    - Utility / Transaction & Block Features
      - [EIP-7745 updates](https://gist.github.com/zsfelfoldi/3f0f44c4ac0fb940907f9fa714d2334c)
    - Cryptography / PQ Precompiles
      - Glamsterdam the right time?
    - Other / Process
      - what to do with EIPs that don't change protocol?
- Housekeeping (not urgent)
  - [H-Star name](https://github.com/ethereum/pm/issues/1808#issuecomment-3605030181)
  - [minimum hard fork testnets & mainnet rollout timeline](https://github.com/ethereum/pm/issues/1808#issuecomment-3544214756)

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

--- github-actions[bot] 2025-11-11T04:38:08Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/26515)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=2KU93ZHf-ww)

--- gcolvin 2025-11-15T05:53:55Z ---
I beg you all to reconsider whether to consider [EIP-7979: Call and Return Opcodes for the EVM](https://eips.ethereum.org/EIPS/eip-7979) for inclusion.

Calls and returns are a very small, very safe ask.  Three operations that we have already implemented and tested at least once in most all of our clients.  The simple mechanism proposed here is used by all of the other EIPs that do this job, and has seen extensive use in industry since 1945.

Calls and returns are also a longstanding community ask that will make a big difference to a lot of people.  With them we can have one-pass streaming JITs from EVM to 8086, ARM, and RISC-V.  With them we can have automatic proofs of EVM contract correctness.  With them we can have the static control flow we need to allow for such tools and more.  Without them EVM bytecode is hopeless spaghetti, and any such [Static Analysis](https://www.certora.com/blog/static-analysis-eof-response) remains stymied.

Further, as a champion I am volunteering my time now, not in some nebulous future.  Almost none of our other VM experts remain to help me.  And to my knowledge nobody is offering to champion any later proposals.  So we can consider them now or never.  There is EVM R&D I have wanted to do for a long time now.  And I'm an ailing old man who can't keep this up much longer.  (Which might be good news if you wish this crotchety geezer would just go away.)  I need calls, returns, and static control flow to even begin my work, and I'm not the only one.

So can we please quit kicking this ancient can down the road and at least consider getting the EVM caught up to 1945 standards?

--- abcoathup 2025-11-17T23:03:39Z ---
Assuming we aim for two upgrades a year.

**Minimum mainnet timeline from testnet releases** 
(based on https://github.com/ethereum/pm/blob/master/processes/protocol-upgrade.md)

| Event | Min days | Total elapsed |
|-------|------------|---------------|
| testnet release(s) |   | 0  |
| 1st testnet upgrade |  14 | 14 |
| 2nd testnet upgrade | 14 | 28 |
| mainnet date set | 2 | 30 |
| mainnet client releases | 7 | 37 |
| mainnet upgrade | 30 | 67|

_Completely hypothetical mainnet targets (for illustration purposes only)_
**Glamsterdam**: June 24, 2026, testnet release(s) required by April 18, 2026 (at the latest).
**Heka-Bogotá**: December 9, 2026, testnet release(s) required by October 3, 2026 (at the latest). 
*Note 2026 is a Devcon year

--- abcoathup 2025-11-17T23:11:26Z ---
https://ethereum-magicians.org/t/glamsterdam-upgrade-non-headliner-scoping-execution-layer/26502

Draft shortlist of CFI/DFI Execution Layer EIPs based on client team writeup of preferences ([Erigon](https://github.com/erigontech/erigon/wiki/Glamsterdam-PFI-stand), [Geth](https://notes.ethereum.org/@fjl/geth-glamsterdam-eip-ranking), [Nethermind](https://x.com/URozmej/status/1986040895578296825), [Reth](https://hackmd.io/@jenpaff/S1bj9gqkbe) & [Besu](https://hackmd.io/@RoboCopsGoneMad/GlamTiers)) and discussions at [ACDC #224](https://ethereum-magicians.org/t/all-core-devs-execution-acde-224-november-6-2025/25950/)

Note:  @adietrichs is compiling an official shortlist

--- abcoathup 2025-11-19T00:26:38Z ---
### [EIP-8081](https://github.com/ethereum/EIPs/pull/10772/files): Hardfork Meta - Heka/Bogotá

We should define the Fork Focus and then converge towards Headliner(s), rather than skip ahead to choosing a headliner:
https://ethereum-magicians.org/t/community-consensus-fork-headliners-acd-working-groups/24088

Once Glamsterdam scoping is done (hopefully by December), we could then do the following:
1. Retro on Fusaka (and update any processes based on that)
2. Define Fork Focus for Heka/Bogotá 
3. Converge on Headliner(s) for Heka/Bogotá (hopefully by February to give enough time to ideally ship in 2026)

--- SirSpudlington 2025-11-20T13:20:02Z ---
I also have a request for feedback on [stripping the TX out of EIP-7932 to favour AA over native TX's](https://ethereum-magicians.org/t/open-question-to-the-core-devs-about-eip-7932/26636).

This does not need to be discussed on call, but if the stripped change is favoured a lot more by the core devs, it may increase the odds of it being included in Glamsterdam.

--- barnabasbusa 2025-11-28T14:52:48Z ---
Not really a consensus related question, but we should consider working on making the engine ssz compatible. Maybe this is for a future call, but would like to open this box today, if we get some time in the end.

--- rdubois-crypto 2025-12-01T14:03:25Z ---
[EIP-8081: MLDSA](https://github.com/ethereum/EIPs/pull/10557) and [EIP-7851: EOA deactivation](https://github.com/ethereum/EIPs/pull/9411)

We would like to discuss inclusion of EIP-8051: add MLDSA.

https://ethereum-magicians.org/t/eip-8051-ml-dsa-verification/25857

As well, inclusion of EIP 7851 is mandatory, otherwise having a PQ smart accounts/precompile doesn't make sense.

The combination of those 2 EIPs would provide a kill switch in case of early rising of a Quantum attacker.

--- nixorokish 2025-12-01T23:18:52Z ---
Little item to add: can we assume no ACDC on Dec 25th?
edit: also ACDE Jan 1st - keep or cancel?

--- ralexstokes 2025-12-02T00:08:14Z ---
> Little item to add: can we assume no ACDC on Dec 25th?

correct, lets cancel

--- ralexstokes 2025-12-02T00:09:22Z ---
we discussed FOCIL for inclusion in Heka / Bogota on https://github.com/ethereum/pm/issues/1812

there's interest to put in that fork, but we were undecided about CFI or SFI

we said we would get a temperature check on this week's call to gauge the best way to proceed

--- nixorokish 2025-12-03T04:21:09Z ---
It would also be nice to choose a name for H-star since we'll be discussing it more going forward :)
https://ethereum-magicians.org/t/portmanteau-for-heka-bogota-upgrade-after-glamsterdam/26400

--- abcoathup 2025-12-04T06:24:50Z ---
## Client diversity
Is it time to campaign large stakers (again) to improve client diversity?

<img width="926" height="756" alt="Image" src="https://github.com/user-attachments/assets/6075da9d-6640-412f-9358-38aab6e9c295" />

*From: https://clientdiversity.org/#distribution*


--- akashkshirsagar31 2025-12-04T07:28:29Z ---
X Stream: https://x.com/i/broadcasts/1djGXWdNXpEKZ

--- anderselowsson 2025-12-04T12:52:43Z ---
This post offers a brief overview of options for preserving scaling under state creation repricing related to EIP-8037:

https://notes.ethereum.org/@anderselowsson/3-paradigms-for-state-creation-repricing

In summary, there are two orthogonal pathways to preserve scaling.

A. Make adjustments to the protocol to allow for higher state growth.
B. Make adjustments to how the protocol processes state creation operations.

The post is focused on (B). Approaches can be grouped into three paradigms:

#### Repricing – Reprice state creation to shift resource consumption

**Pros:** Simplest solution, particularly the one-time change. The dynamic change is also fairly straightforward.
**Cons:** Does not structurally improve scaling because state creation gas will still count against the block's `gas_limit`. Does not target a specific long-run state growth, with a rather broad range of outcomes possible. Relies on estimated demand elasticity instead of observed usage.

Example: [EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) as is or [EIP-8073](https://github.com/ethereum/EIPs/pull/10667) but specified as [here](https://ethereum-magicians.org/t/eip-8037-state-creation-gas-cost-increase/25694/8).

#### Metering – Meter state creation to exempt it from the regular block gas limit

**Pros:** Facilitates much more scaling by tracking state creation gas separately from regular gas, such that state creation will not crowd out other resources. State creation metering can be reused for more elaborate solutions in the future.
**Cons:** Does not fully improve scaling, because the unified pricing can still tilt resource consumption: the single base fee implies that high demand for regular gas raises the cost for state creation, or vice versa, potentially leading to under-utilization. Does not fully target a specific long-run state growth, with a rather broad range of outcomes still possible. Relies on estimated demand elasticity instead of observed usage.

Example: [EIP-8011](https://eips.ethereum.org/EIPS/eip-8011), possibly specified as [here](https://notes.ethereum.org/@anderselowsson/State_metering).

#### Targeting – Meter and target state creation to exempt it from the regular block gas limit and achieve a specific state growth under full utilization

**Pros:** Facilitates the most scaling by tracking state creation gas separately from regular gas, such that state creation will not crowd out other resources, and pricing it separately for full utilization. Targets a specific desirable state growth. State creation metering can be reused for other solutions in the future.
**Cons:** Besides adding metering, the approach also adds three header fields (`state_bytes_created`, `state_bytes_cleared`, `excess_state_bytes`). This is the solution that has the highest complexity.

Example: [EIP-8075](https://eips.ethereum.org/EIPS/eip-8075).


--- pkieltyka 2025-12-04T13:04:24Z ---
hi everyone. congrats on the Fusaka upgrade.

I'd like to advocate for the inclusion of https://eips.ethereum.org/EIPS/eip-7708 in the next Ethereum upgrade to significantly simplify the ability to query the ETH value updates from all transactions with just a vanilla Ethereum node. 

The inclusion of 7708 offers many updates to developers, such as for transaction simulation or external offchain indexers to event source and aggregate ETH value transfers from log data.

Additional background: Indexers are an important adjacent infrastructure component that works on top of Ethereum chains for many purposes to offer apps and clients a simplified inverted index on state inside of blocks. Many indexers are written based on event sourcing / event aggregation of eth event logs, such that the events work as deltas to balance state (in the case of ERC20/721/1155/etc value transfers), and by replaying offchain all events from start block to head, you are able to calculate reliably the balance of any token by just replaying and persisting the transfer events. Many indexers do this, including TheGraph, Sequence Indexer, etc., and it works beautifully. We run the Sequence Indexer across 55 evm chains (many kinds, even alt-L1 evms), and it works reliably for years. However, when it comes to aggregating native ETH balances or computing transaction history of value transfers, unfortunately this has proven to be extremely difficult with the goal in mind to capture all ETH value transfers and state updates from contract calls. For ETH transfer data we've had to work around this issue by fetching debug and trace data, but its a significant lift and huge data set to process when simply having EIP-7708 where and ETH value state update would offer a delta event from any interaction, similar to how ERC20's perform. The inclusion of EIP-7708 would be a dream come true and significantly simplify the architectural demands of indexers that want to track ETH value updates from all transactions. 

The last point is, I'm not sure if its possible or realistic to offer 7708 events for all historic data as well, or just future blocks from the time of upgrade, but either is fine, as one can always compute a snapshot up to the point of upgrade and use it as the seed data. Of course, would be nice if could be updated from the start, but offchain datasets which essentially offer the same data and stored on IPFS/somewhere would work just as well without having to modify history. However, I defer to the amazing ethereum r&d team to make the best decision on implementation. 

--- taxmeifyoucan 2025-12-04T13:30:14Z ---
Quick summary of incidents that followed Fusaka: https://notes.ethereum.org/@MarioHavel/S1wW5tJyWx

--- nixorokish 2025-12-08T16:21:21Z ---
closing in lieu of #1837 
