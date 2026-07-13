# ISSUE 1931: All Core Devs - Execution (ACDE) #231, Feb 26, 2026
Created: 2026-02-17T00:21:36Z by nixorokish

### UTC Date & Time

[February 26, 2026, 14:00 UTC](https://savvytime.com/converter/utc/feb-26-2026/2pm)

### Agenda

- Glamsterdam
  - devnet updates
- Misc.
  - [EraE status](https://notes.ethereum.org/j65MUa-KTB-hZ2kDrijS9A) by @s1na
  - [`txpool` namespace standardization](https://github.com/ethereum/pm/issues/1931#issuecomment-3961057441) by @bomanaps
- Hegotá
  - headliner updates
    - [EIP-8105 withdrawal in favor of LUCID](https://ethereum-magicians.org/t/hegota-headliner-proposal-eip-8105-universal-enshrined-encrypted-mempool-eem/27448/3)
  - headliner discussion
    - [Erigon position](https://github.com/ethereum/pm/issues/1931#issuecomment-3961108850)
    - [Besu position](https://hackmd.io/@YwTR7izNSrCEQYKWdzdy_Q/Syutq86OZe)
    - [Geth position](https://notes.ethereum.org/@lightclient/h-is-for-hardness)
    - [Nimbus EL position](https://github.com/ethereum/pm/issues/1931#issuecomment-3966735519)

### Call Series

All Core Devs - Execution

### Autopilot Mode

- [x] Use autopilot (recommended defaults for this call series)


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

90 minutes

### Occurrence Rate

bi-weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Display Zoom Link in Calendar Invite (Optional)

- [x] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2026-02-17T00:22:39Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27707)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=cAP3_gwT6-E)

--- COMMENT by s1na at 2026-02-23T10:21:59Z ---
I'd like to bring up EraE integration into the clients. This is the successor format to era1 which is suitable for pre and post-merge history. Overview and relevant links (spec, geth implementation and sample exports) can be found here: https://notes.ethereum.org/j65MUa-KTB-hZ2kDrijS9A

--- COMMENT by bomanaps at 2026-02-25T18:07:32Z ---
I'd like to bring the stalled txpool standardization work from PR https://github.com/ethereum/execution-apis/pull/353 and opened a new PR https://github.com/ethereum/execution-apis/pull/758 to move this forward. I found that Geth, Erigon, Nethermind, and Reth already ship txpool_content and txpool_status with compatible responses, txpool_contentFrom is supported by Geth and Erigon, and txpool_inspect is supported by Geth and Nethermind so I documented what exists the new txpool_transactions and txpool_statistics methods with filtering capabilities from the original https://github.com/ethereum/execution-apis/pull/353 proposal will go through the EIP process based on Felix's feedback from our last RPC standards call. Would love input from client teams, especially Besu on whether they'd consider aliasing the standard names.

--- COMMENT by yperbasis at 2026-02-25T18:15:30Z ---
Erigon's position on Hegota EL headliners: we support [EIP-8141](https://eips.ethereum.org/EIPS/eip-8141): Frame Transaction as the EL headliner. We believe that [LUCID](https://ethresear.ch/t/lucid-encrypted-mempool-with-distributed-payload-propagation/24042) or something similar is crucial for Ethereum to prevent front running, but are fine with delaying it to the I* hardfork to make sure it's fully aligned with the proposals that affect slot times and how transactions are mapped to slots (e.g. [quick slots](https://ethereum-magicians.org/t/the-case-for-quick-slots-in-hegota/27708), [payload chunking](https://github.com/ethereum/EIPs/pull/10900), [block in blobs](https://eips.ethereum.org/EIPS/eip-8142)).

--- COMMENT by daniellehrner at 2026-02-26T11:06:22Z ---
Besu is supporting LUCID as Hegota EL headliner: https://hackmd.io/@YwTR7izNSrCEQYKWdzdy_Q/Syutq86OZe

We think it not only complements FOCIL very well, but also restores trustless transaction inclusion to the public mempool.

--- COMMENT by akashkshirsagar31 at 2026-02-26T12:15:07Z ---
X Stream: https://x.com/i/broadcasts/1qGvvkvbykOGB

--- COMMENT by tersec at 2026-02-26T13:44:50Z ---
The Nimbus team supports either EIP-8141, preferably with appropriate use of SSZ, or EIP-7807. In particular, EIP-8141 introduces a new transaction type and formats, and it would better align with the Ethereum roadmap to introduce these as SSZ than transition them later. Similarly, EIP-7807 aligns the full Ethereum stack responsible for interacting with execution blocks, payloads, or envelopes to use a single consistent, verifiable, and efficient representation.

Otherwise, Nimbus finds EIP-8141 as presented worth supporting.

--- COMMENT by lightclient at 2026-02-26T13:55:11Z ---
Geth prefers EIP-8141 as headliner for Hegotá. Full reasoning here: https://notes.ethereum.org/@lightclient/h-is-for-hardness


--- COMMENT by danceratopz at 2026-02-26T14:03:31Z ---
Just a quick head's up for client teams: Future excution-specs fixture releases will follow a different directory layout (fixture formats themselves haven't changed). Please read more in [announcment in the Eth R&D #el-testing channel](https://discord.com/channels/595666850260713488/753271902520213625/1476579508030013662).

But the TLDR is:
Previously, fixture JSON files accumulated test cases for every target fork in a single file, meaning each file grew with every new fork. Fixtures are now split into per-fork sub-directories, keeping file sizes bounded and letting you point your runner at exactly the fork you need to test.

And the layout is:
```
blockchain_tests_engine/
├── for_paris/
│   ├── paris/
│   │   ├── eip7610_create_collision/
│   │   ├── revert_in_create/
│   │   └── security/
│   └── shanghai/
│       └── eip3651_warm_coinbase/
└── for_shanghai/
    ├── paris/
    │   ├── eip7610_create_collision/
    │   └── ...
    └── shanghai/
        ├── eip3651_warm_coinbase/
        ├── eip3855_push0/
        ├── eip3860_initcode/
        └── eip4895_withdrawals/
```

--- COMMENT by akashkshirsagar31 at 2026-02-26T21:33:26Z ---
Audio Podcast Version: https://open.spotify.com/episode/3kIUuU6CiKVCJ2XvmNFe8e?si=aeWtIxAcST-Czm0wveVorw

--- COMMENT by github-actions[bot] at 2026-02-27T18:17:34Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
