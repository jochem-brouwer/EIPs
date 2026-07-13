# ISSUE 1921: All Core Devs - Execution (ACDE) #230, Feb 12, 2026
Created: 2026-02-10T15:27:11Z by nixorokish

### UTC Date & Time

[February 12, 2026, 14:00 UTC](https://savvytime.com/converter/utc/feb-12-2026/2pm)

### Agenda

- Glamsterdam
  - devnets
    - [bal-devnet-2](https://notes.ethereum.org/@ethpandaops/bal-devnet-2) update
    - EIP-8024 changes, see [comment](https://github.com/ethereum/pm/issues/1921#issuecomment-3887841880) by @frangio
      - https://github.com/ethereum/EIPs/pull/11094#issuecomment-3769076517
      - https://github.com/ethereum/EIPs/pull/11306
    - `eth_simulateV1` RPC support
    - bal-devnet-3 priorities
    - nft-devnet-10 (eth/70) update
  - scoping
    - PFI: [EIP-7975: eth/70 - partial block receipt lists](https://eips.ethereum.org/EIPS/eip-7975)
- Hegotá
  - headliner
    - proposal presentation: [LUCID encrypted mempool](https://ethereum-magicians.org/t/hegota-headliner-lucid-encrypted-mempool/27658) by @anderselowsson
    - call for client preferences
    - open discussion

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

--- COMMENT by github-actions[bot] at 2026-02-10T15:28:26Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27707)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=WzaE1fXWLPE)

--- COMMENT by anderselowsson at 2026-02-11T10:08:26Z ---
I'd like to present the [LUCID encrypted mempool](https://ethresear.ch/t/lucid-encrypted-mempool-with-distributed-payload-propagation/24042) design, a minimum viable version of which is [proposed as an EL headliner](https://ethereum-magicians.org/t/hegota-headliner-lucid-encrypted-mempool/27658) for H*. Julian and Justin F may also wish to say a few words.

--- COMMENT by frangio at 2026-02-11T23:43:24Z ---
I'd like to discuss EIP-8024 changes:
- https://github.com/ethereum/EIPs/pull/11094#issuecomment-3769076517
  - Needs final decision, I explain above why I oppose it
- https://github.com/ethereum/EIPs/pull/11306
  - Recent idea for small simplification

--- COMMENT by lightclient at 2026-02-12T05:55:22Z ---
In case any ELs were wanting to look at frame tx complexity or interested in trying to implement, I've got an implementation in EELS coming along with EEST tests so you check it out: https://github.com/lightclient/execution-specs/pull/1

*note: base in my repo because  I created a Bogota branch based on current state of Amsterdam, since Amsterdam is latest fork upstream.*

--- COMMENT by akashkshirsagar31 at 2026-02-12T13:06:31Z ---
X Stream: https://x.com/i/broadcasts/1PlJQOMbqpDKE

--- COMMENT by github-actions[bot] at 2026-02-13T18:21:57Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
