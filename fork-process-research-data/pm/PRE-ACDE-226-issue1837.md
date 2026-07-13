# ISSUE 1837: All Core Devs - Execution (ACDE) #226, Dec 18, 2025

### UTC Date & Time

[December 18, 2025, 14:00 UTC](https://savvytime.com/converter/utc/dec-18-2025/2pm)

### Agenda

- Announcements
  - [ACD holiday schedule](https://github.com/ethereum/pm/issues/1837#issuecomment-3656127221)
  - [zkEVM roadmap updates doc](https://github.com/eth-act/planning/pull/1) by @kevaundray 
- H-star
  - FOCIL decision summary
  - [Heka -> Heze name change](https://github.com/ethereum/pm/issues/1837#issuecomment-3656179646)
  - [portmanteau decision](https://ethereum-magicians.org/t/h-star-name-for-consensus-layer-upgrade-after-glamsterdam/24298/18)
- Glamsterdam
  - repricing update
  - scoping decisions
    - [updated CFI / DFI candidate EIPs](https://notes.ethereum.org/@ansgar/glamsterdam-el-pfi-eips)
    - [comment regarding EIP-8051 and related EIPs](https://github.com/ethereum/pm/issues/1837#issuecomment-3655914307) by @rdubois-crypto 
  - Open Questions
- Other
  - [EIP-8077](https://eips.ethereum.org/EIPS/eip-8077) and [EIP-8094](https://eips.ethereum.org/EIPS/eip-8094) by @cskiraly 

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

--- github-actions[bot] 2025-12-08T16:22:12Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27004)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=_KGsKUeH77g)

--- rdubois-crypto 2025-12-15T14:24:05Z ---
**Inclusion of DILITHIUM44 and FALCON512.**

We would like to discuss inclusion of PQ signatures, both DILITHIUM44 and FALCON512.

(https://github.com/ethereum/EIPs/pull/10557 and https://github.com/ethereum/EIPs/pull/9411)

https://ethereum-magicians.org/t/eip-8051-ml-dsa-verification/25857

As well, inclusion of EIP 7851 is important, otherwise having a PQ smart accounts/precompile doesn't make sense for EIP-7702 (would require 4337).

The combination of those one signature + eoa deactivation EIPs would provide a kill switch in case of early rising of a Quantum attacker.



--- barnabasbusa 2025-12-15T15:03:01Z ---
Proposed ACD Holiday schedule: 
18dec - ACDE
22dec - ACDT Cancelled 
25dec - ACDC Cancelled
29dec - ACDT Cancelled
1jan - ACDE Cancelled
5jan - ACDT -> ACDE 
8jan - ACDC
12jan - ACDT

--- barnabasbusa 2025-12-15T15:11:50Z ---
On ACDT we decided to Heka -> Heze (Actual IAU) star, we had more votes on Heze than on the original poll, so think it makes sense to make the change now.

Would be good to have a Portmanteaus selected on this call, so we can have this topic closed once and for all. 

https://ethereum-magicians.org/t/h-star-name-for-consensus-layer-upgrade-after-glamsterdam/24298/

--- kevaundray 2025-12-16T00:28:20Z ---
Small note on roadmap items for zkEVM: https://github.com/eth-act/planning/pull/1

--- abcoathup 2025-12-16T02:24:39Z ---
## Glamsterdam scoping

| Upgrade                                                                              | Headliners | Core EIPs | Other EIPs | mainnet |
|----------------------------------------------------------------|-------------|------------|------------|-----------|
| [Dencun](https://eips.ethereum.org/EIPS/eip-7569)          | -                 | 9               | 0             | March 13, 2024 |
| [Pectra](https://eips.ethereum.org/EIPS/eip-7600)             | -                | 10              | 2              | May 07, 2025 |
| [Fusaka](https://eips.ethereum.org/EIPS/eip-7607)            | 1                | 8               | 4              | December 3, 2025 |
| [Glamsterdam](https://eips.ethereum.org/EIPS/eip-7773) | 2                | *(9 [CFI'd](https://forkcast.org/upgrade/glamsterdam#considered-for-inclusion) so far)*    | 0             | ? 2026 * |

* If we wanted Glamsterdam upgrade by June, then testnet release(s) would likely be required by [mid April](https://github.com/ethereum/pm/issues/1808#issuecomment-3544214756).  (Also see: [forkcast.org/schedule](https://forkcast.org/schedule/))



--- abcoathup 2025-12-16T02:52:13Z ---
## H-star upgrade naming

A name is needed before Fork focus and Headliner selection starts in January.  
Suggest using polls in the chat to come to a decision.

* Heka is **NOT** in International Astronomical Union (IAU) catalog of stars (as raised by @leobago).  
* Core Devs can choose between:
  * [Heka](https://en.wikipedia.org/wiki/Meissa) (chosen at ACDC [#168](https://forkcast.org/calls/acdc/168))
  * [Heze](https://ethereum-magicians.org/t/h-star-name-for-consensus-layer-upgrade-after-glamsterdam/24298/21) ([topped Eth Magicians poll](https://ethereum-magicians.org/t/h-star-name-for-consensus-layer-upgrade-after-glamsterdam/24298/16) of IAU stars (excluding Hadar?))

## Portmanteau
* Depending on the H-star name, Core Devs can choose the portmanteau:
  * [Heka + Bogotá](https://ethereum-magicians.org/t/portmanteau-for-heka-bogota-upgrade-after-glamsterdam/26400#p-64019-poll-2): Hektá & Hekotá leading the signaling
  * [Heze + Bogotá](https://ethereum-magicians.org/t/h-star-name-for-consensus-layer-upgrade-after-glamsterdam/24298/18): Hegotá & Hezotá leading the signaling



--- nixorokish 2025-12-17T17:08:30Z ---
would like sign-off on [h-star timeline](https://ethereum-magicians.org/t/eip-8081-heka-bogota-network-upgrade-meta-thread/26876) so i can blast it out to the ecosystem as "official". i suggest a 30-day window for non-headliner EIP proposals to make our "choosing between EIPs" a little more manageable next cycle

--- akashkshirsagar31 2025-12-17T20:52:30Z ---
X Stream: https://x.com/i/broadcasts/1BdGYZAzbwEJX

--- cskiraly 2025-12-17T21:18:26Z ---
I would have two EL networking EIPs to introduce quickly, if there is time. They are not fork-specific (hence I wasn't proposing them on the Glamsterdam list) but provide mempool improvements that would be useful for scaling.
- [EIP-8077](https://eips.ethereum.org/EIPS/eip-8077) proposes to change the way we propagate transactions in the mempool, improving efficiency and preparing for a future where not everyone has the capacity to receive all transactions.
- [EIP-8094](https://eips.ethereum.org/EIPS/eip-8094) aims to make RBF (replace by fee) much more efficient, eliminating the cost of redistributing blob content if only the metadata (fees) of a transaction are updated.

--- MariusVanDerWijden 2025-12-18T09:34:35Z ---
I would like to give an update on the repricing work. Its already on the schedule, but I think its time to tie down the scope for repricings for Glamsterdam

--- gkoumout 2025-12-18T10:00:54Z ---
# Lido’s view on contract code size EIPs for Glamsterdam 

**TL;DR:** Lido supports increasing Ethereum’s contract code size limit and favors EIP-7907 as a pragmatic, low-risk solution that provides an explicit and usable 48KB cap, while EIP-2926, though more future-oriented, introduces significant complexity and indirect constraints that make it less suitable for near-term inclusion.

**Why larger contracts are needed:** Ethereum’s current 24KB runtime bytecode limit (from EIP-170) is increasingly restrictive for modern applications. Lido already maintains several production contracts close to this ceiling, which forces architectural workarounds such as contract splitting, factories, or proxies. These patterns increase complexity, deployment risk, and long-term maintenance costs, making larger contracts a practical requirement.

**Why we need a specified max size:** In Pectra upgrade, EIP-7825 introduced a transaction size cap, implying a theoretical upper bound of ~83.8KB for contract code size to deploy in a transaction. However, contract creation transactions must also account for initcode execution, calldata, and other overhead, making this bound non-universal but dependent on tooling and deployment factors. This reinforces the need for a clear, protocol-defined maximum contract size that developers can reliably target.
What are the options: Two related proposals are considered for Glamsterdam: 

1. [EIP-2926](https://eips.ethereum.org/EIPS/eip-2926) removes the explicit cap by introducing chunked, Merkleized code storage.
2. [EIP-7907](https://eips.ethereum.org/EIPS/eip-7907) keeps the existing model and explicitly increases the maximum runtime bytecode size to 48KB. 

Both aim to enable larger contracts, but differ significantly in scope, complexity, and impact.

**EIP-2926:** It is an important and future-proof upgrade, at the cost of significantly higher complexity and increased code-deposit gas costs. Under post-Pectra constraints, gas economics reintroduce an implicit effective cap (~47KB), enforced by transaction limits rather than consensus rules. This creates uncertainty for UX, tooling, and compiler behavior, and likely requires heavy client changes and migration effort. 

**Why EIP-7907 is the pragmatic choice:** EIP-7907 directly increases the runtime bytecode limit to 48KB, providing immediate and predictable relief for applications. It introduces a universal, explicit maximum, fits comfortably within transaction limits, and leaves sufficient slack for calldata and initcode. Compared to EIP-2926, it is simpler, lower risk, and directly solves the problem many applications face today. From the application perspective, this clarity and determinism are critical, making EIP-7907 the proposal Lido is best positioned to support for Glamsterdam.



--- rakita 2025-12-18T10:31:35Z ---
Sharing [Reth view](https://docs.google.com/spreadsheets/d/16Mes5B_b5vkBApzesH6uEJWLv9lAo9aa8-zTY0FZJSw/edit?gid=1783871973#gid=1783871973) on Glamsterdam EIPs


--- Marchhill 2025-12-18T12:59:32Z ---
I wrote a [short FAQ / explainer](https://ethereum-magicians.org/t/conditional-transactions-eip-7793-for-glamsterdam-faq/27228) of Conditional Transactions (EIP-7793) and why I think they should be in Glamsterdam. It doesn't need to be on the agenda to be discussed but something for teams to review async

--- abcoathup 2025-12-19T02:46:29Z ---
@MariusVanDerWijden can you share your slides please


--- cskiraly 2025-12-19T08:56:32Z ---
> I would have two EL networking EIPs to introduce quickly, if there is time. They are not fork-specific (hence I wasn't proposing them on the Glamsterdam list) but provide mempool improvements that would be useful for scaling.
> 
> * [EIP-8077](https://eips.ethereum.org/EIPS/eip-8077) proposes to change the way we propagate transactions in the mempool, improving efficiency and preparing for a future where not everyone has the capacity to receive all transactions.
> * [EIP-8094](https://eips.ethereum.org/EIPS/eip-8094) aims to make RBF (replace by fee) much more efficient, eliminating the cost of redistributing blob content if only the metadata (fees) of a transaction are updated.

We were running out of time before I could present these EIPs. I will propose it for the agenda of the next ACDE again.

--- MariusVanDerWijden 2025-12-19T11:25:42Z ---
@abcoathup https://docs.google.com/presentation/d/1F7fa8telOWEQngIVZTbnemk3xNdzX6OsSqNozYNcDhg/edit?usp=sharing

--- nixorokish 2025-12-29T19:24:27Z ---
closed in lieu of #1854
