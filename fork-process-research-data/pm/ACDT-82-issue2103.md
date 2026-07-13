# ISSUE 2103: All Core Devs - Testing (ACDT) #82, June 8, 2026
Created: 2026-06-02T13:09:13Z by jtraglia

### UTC Date & Time

[June 08, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jun-8-2026/2pm)

### Agenda

#### Devnet status updates

* `bal-devnet-7`
* `glamsterdam-devnet-5`

#### EL spec updates

See the `glamsterdam-devnet-6` tracker:

* https://github.com/ethereum/execution-specs/issues/2915

#### CL spec updates

New releases:

* [beacon-apis v5.0.0-alpha.2](https://github.com/ethereum/beacon-APIs/releases/tag/v5.0.0-alpha.2)
* [consensus-specs v1.7.0-alpha.9](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.9)
* [consensus-specs v1.7.0-alpha.10](https://github.com/ethereum/consensus-specs/releases/tag/v1.7.0-alpha.10)

#### Discussions

* https://github.com/ethereum/pm/issues/2103#issuecomment-4602772906
  * Which devnet should we include 7688 in?
  * Led by @etan-status.
* https://github.com/ethereum/pm/issues/2103#issuecomment-4632595698
  * Our options we have for deploying a deterministic factory.
  * https://nerolation.github.io/hegota-eip-presentations/eip-7997.html
  * Led by @nerolation.
* https://github.com/ethereum/pm/issues/2103#issuecomment-4624490675
  * Clarify whether EIP-7997 factory insertion must be in BAL to avoid state root mismatches.
  * Led by @jochem-brouwer.
* https://github.com/ethereum/pm/issues/2103#issuecomment-4649213390
  * Clarifications on BAL and 8037 interaction.
  * Led by @raxhvl.
* https://github.com/ethereum/pm/issues/2103#issuecomment-4649326156
  * Decision on the EIP-7702 authorization gas accounting for EIP-8037.
  * Led by @misilva73.
* https://github.com/ethereum/pm/issues/2103#issuecomment-4649369744
  * Decision on syncing BAL and access list for 7702 delegation.
  * Led by @raxhvl.
* https://github.com/ethereum/pm/issues/2103#issuecomment-4649520098
  * Thoughts on how range sync should be used in Gloas.
  * Led by @nflaig.

#### Next steps

* Get all CL clients included in glamsterdam-devnet-5.
* Finalize scope (both layers) for glamsterdam-devnet-6.
* ...
* Profit?

#### Other

* Status update on SSZ Engine API work.
* This week's ACDC call (180) will be at 11:00 UTC, 3 hours earlier than before.

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

--- COMMENT by github-actions[bot] at 2026-06-02T13:10:08Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260608%2F20260609&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Testing+%28ACDT%29+%2382%2C+June+8%2C+2026&dates=20260608T140000Z%2F20260608T150000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F88479308162%3Fpwd%3D9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2103)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28681)

--- COMMENT by etan-status at 2026-06-02T13:18:28Z ---
what devnet should we include 7688 to unblock the deposit requests SSZ limit that gets hit on 200M gas?

- Tests + Kurtosis: https://github.com/ethereum/consensus-specs/pull/4630#issuecomment-4352744403

--- COMMENT by jochem-brouwer at 2026-06-04T17:14:38Z ---
[EIP-7997](https://eips.ethereum.org/EIPS/eip-7997) spec and tests, commented this in [discord](https://discord.com/channels/595666850260713488/688075293562503241/1512141314718961776) (slightly edited it here by removing unnecessary items):

> Hi all, since we include EIP-7997 in the next Glam devnet I feel like these points: https://ethereum-magicians.org/t/eip-7997-deterministic-factory-predeploy/24998/26?u=jochem-brouwer are not yet fixed in the current EIP spec. 
> The main point is that if we insert the factory in the chain at the fork block this should be included in the BAL. If it is not included then any client which implements a quick state root check by batch writing the BAL to state, they will not insert the factory address and thus yield invalid state root. 
> An alternative could be that this change MUST NOT be in the BAL and that this insertion in implicit, but I nevertheless feel this should be cleared up.
> If no client currently does the "quick state root check" via BAL then this problem could go unnoticed
There are no EEST tests yet but there is this issue: https://github.com/ethereum/execution-specs/issues/1988

Let's agenda [EIP-7997](https://eips.ethereum.org/EIPS/eip-7997) spec+tests on ACDT unless this is resolved on discord async (will then update this comment and leave a message we can remove this item)

--- COMMENT by nerolation at 2026-06-05T14:24:32Z ---
With regards to EIP-7997 and it's inclusion to a devnet, after discussing things with frangio, there's the following option:
* Instead of enshrining a factory at 0x12, we enshrine Arachnid at `0x4e59...`.

This would essentially mean we write the bytecode into the genesis file. 
**For Ethereum Mainnet, that's a no-op**, as the contract already exists. New chains profit from having the factory deployed independent from their gas schedule.

To keep things clean, we could swap 7997 with this one:  
https://github.com/nerolation/EIPs/blob/eaee46304d8c46c2c02fad963f7295d10396c6d8/EIPS/eip-8269.md

Or we change 7997 (though, the changes would be quite big).

We already handle it like that for upcoming devnets where we have Arachnid deployed via the gensis file:
https://github.com/ethpandaops/ethereum-genesis-generator/pull/294

The downsides of Arachnid [are known (h/t frangio)](https://discord.com/channels/595666850260713488/688075293562503241/1512444613783326852):

1) it doesn't forward revert data, so if the deployment reverts you need the trace to get the revert data
2) it returns the deployed address in 20 bytes rather than padded to 32. this makes it incompatible with standard solidity abi decoding for addresses




--- COMMENT by akashkshirsagar31 at 2026-06-08T11:08:03Z ---
X Stream: https://x.com/i/broadcasts/1OGwbbPBaOoKB

--- COMMENT by raxhvl at 2026-06-08T12:58:56Z ---
BAL and 8037 interaction needs clarification;  https://hackmd.io/@bFEBbZiVSAO0IURh9qzEFg/BJmFYqCeGl

--- COMMENT by misilva73 at 2026-06-08T13:12:31Z ---
Also, can we have a decision on the  EIP-7702 authorization gas accounting for EIP-8037?

PR: https://github.com/ethereum/EIPs/pull/11715

--- COMMENT by raxhvl at 2026-06-08T13:17:14Z ---
Decision on syncing BAL and access list for 7702 delegation: https://github.com/ethereum/execution-specs/pull/2900

--- COMMENT by jtraglia at 2026-06-08T13:35:03Z ---
Copied from Eth R&D discord [here](https://discord.com/channels/595666850260713488/1513383683090813129/1513500890810810538):

> @terence (and others) curious if you have some thoughts on how range sync should work. in my opinion, data columns and payload envelopes by range is just dysfunctional and really error prone, you always have this off-by-one issue and can't fetch data like this during non-finality as peers might have a different view, so you always rely on querying the same peer. how about we completely get rid of by range and instead always query columns/payloads by root? this way you already know the chain you try to sync (based on parent block hashes in blocks) and don't have to try and align different range requests with each other

was discussing this with @wemeetagain before and @twoeths | lodestar

--- COMMENT by terencechain at 2026-06-08T13:48:53Z ---
Any latest update on builder API integration into kurtosis, or another way to start testing locally before we have it on devnet?
Or just yolo it on devnet?

