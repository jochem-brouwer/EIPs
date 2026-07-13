# ISSUE 1970: All Core Devs - Execution (ACDE) #233, March 26, 2026
Created: 2026-03-15T03:02:37Z by nixorokish

### UTC Date & Time

Mar 26, 2026, 14:00 UTC

### Agenda

- Glamsterdam
  - devnet updates
  - [benchmarking progress presentation](https://github.com/ethereum/pm/issues/1970#issuecomment-4133120675) by @MariusVanDerWijden and @Butta [ [slides](https://docs.google.com/presentation/d/1ui9lPlbyxVnA36vB0EGk577XZhTRgriMDeWO0DfiTdg/edit?slide=id.p#slide=id.p) ]
  - [EIP-8070 progress](https://github.com/ethereum/pm/issues/1970#issuecomment-4126453649) by @healthykim 
- Hegotá
  - headliner selection
    - breakout summary
    - [frame tx implementation in EthereumJS](https://github.com/ethereum/pm/issues/1970#issuecomment-4116337595) by @holgerd77 
    - [frame tx alternatives](https://github.com/ethereum/pm/issues/1970#issuecomment-4112448215) by @Giulio2002 
- Misc.
  - [SSZ experiments](https://github.com/ethereum/pm/issues/1955#issuecomment-4023511921) by @Giulio2002
  - [CFI -> SFI process question](https://github.com/ethereum/pm/issues/1955#issuecomment-4032072273) by @spencer-tb 

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

--- COMMENT by github-actions[bot] at 2026-03-15T03:03:32Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27989)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=PP1mBd4FUtQ)

--- COMMENT by Giulio2002 at 2026-03-23T17:34:53Z ---
Want to discuss PQ+SchemedTx as alternative to FrameTxs: https://ethereum-magicians.org/t/frame-transactions-vs-schemedtransactions-for-post-quantum-ethereum/28056. also dont forget SSZ

--- COMMENT by holgerd77 at 2026-03-24T08:34:29Z ---
I've done a first vertical integration (so: some type of trame txs work end-to-end but not all yet) on Frame Transactions using TypeScript (EthereumJS), if someone wants to get an impression how a code base might look like, there are also some end-to-end usage examples in the VM folder, e.g. [here](https://github.com/feelyourprotocol/ethereumjs-monorepo/pull/1/changes#diff-5b83c769f06a039459309a245736ecf866f3ed700789666533efec72f8b6a81f).

Also created some "live-docs" along the way in https://eip-8141-docs.feelyourprotocol.org/. These also contain some gentle EIP intro which I think can be a valuable ressource if someone wants to "onboard" until Thursday.
(yes, AI, but not the usual AI slop, put roughly 50% of effort in the implementation and 50% in these docs)

--- COMMENT by dionysuzx at 2026-03-24T18:22:59Z ---
reminder, there is a frame tx breakout tomorrow for any interested parties: https://github.com/ethereum/pm/issues/1985

--- COMMENT by healthykim at 2026-03-25T13:04:16Z ---
I'd like to discuss EIP-8070 (sparse blobpool). Geth has developed a prototype for it and believes it should be SFIed (in the future). We want to hear opinions from other client teams on this. Related document: https://hackmd.io/hJf45UjXTqikV5GqD5dR-Q

--- COMMENT by abcoathup at 2026-03-26T02:16:13Z ---
## Post Quantum

Google set 2029 post quantum migration timeline.
https://blog.google/innovation-and-ai/technology/safety-security/cryptography-migration-timeline/




--- COMMENT by MariusVanDerWijden at 2026-03-26T09:42:15Z ---
@butta and I would like to take 10 min to give a short presentation about benchmarking and repricings if possible in order to get people up to speed with the latest progress.

--- COMMENT by abcoathup at 2026-03-26T11:19:55Z ---
> short presentation about benchmarking and repricings

@MariusVanDerWijden can you share a link to the slides (here or in the chat)



--- COMMENT by spencer-tb at 2026-03-26T12:37:50Z ---
SFI all bal-devnet-2 EIPs into Amsterdam? PR with more context: https://github.com/ethereum/EIPs/pull/11399

I tried to raise last ACDE but there was not enough time, we discussed this on ACDT 2 weeks ago in favor but wanted to confirm on ACDC/E, raised on ACDC last week but it was decided it wasn't the place, so now trying for ACDE here again!

--- COMMENT by akashkshirsagar31 at 2026-03-26T12:42:08Z ---
X Stream: https://x.com/i/broadcasts/1jGXgemDaaEKZ

--- COMMENT by Giulio2002 at 2026-03-26T12:46:24Z ---
@adietrichs @nixorokish forgot about binary ssz for engine api

--- COMMENT by adietrichs at 2026-03-26T13:56:49Z ---
> forgot about binary ssz for engine api

@Giulio2002 updated.

--- COMMENT by fselmo at 2026-03-26T15:31:14Z ---
Ran out of time again but I think the more time that passes without SFIing EIPs that were in previous devnets (bal-devnet-2 EIPS), with more EIPs being put on top of it for other "bal" devnets, the more risk there exists in our process to passively push things through because of sunken cost. We should find time to talk about devnet process, regardless of when we SFI these particular EIPs imo.

--- COMMENT by akashkshirsagar31 at 2026-03-26T22:05:55Z ---
ACDE 233 Audio Podcast: https://open.spotify.com/episode/4QIYGWCRKjBfkvJGlroZQN?si=0xbyusMKR7S9E3I3mgGKUg

--- COMMENT by github-actions[bot] at 2026-03-27T18:26:41Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
