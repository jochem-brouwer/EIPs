# ISSUE 1901: All Core Devs - Testing (ACDT) #68, February 2, 2026
Created: 2026-01-27T17:42:21Z by parithosh

### UTC Date & Time

[February 02, 2026, 14:00 UTC](https://savvytime.com/converter/utc/feb-2-2026/2pm)

### Agenda

Fusaka:
  - partial cell proofs 
  - blob-devnet-0 
      - `MIN_EPOCHS_FOR_DATA_COLUMN_SIDECARS_REQUESTS` field is not being respected by supernodes, and they still hold up to 18d worth of blob data - incorrect

Gas limit testing: 
  - eth/70 updates
  - benchmarking updates - benchmarkoor demo next week

Glamsterdam: 
  - [bal-devnet-2](https://notes.ethereum.org/@ethpandaops/bal-devnet-2)
    - [Add EIP-7928 Block-level Access Lists JSON RPC methods](https://github.com/ethereum/execution-apis/pull/726)
    - [engine: define EIP-7928 API methods](https://github.com/ethereum/execution-apis/pull/727)
    - [Known blockers](https://discord.com/channels/1359927674746835211/1428002540661899274/1467669238884864194):
      - Besu — Remove gasSpent from Receipt RLP Trie Encoding 
      - Nethermind — Remove gasSpent from Receipt RLP Trie Encoding
      - Nethermind — engine_getPayloadV6 Missing blobGasUsed Field 
      - Nimbus-EL — Gloas Opcodes Activated at Fulu Instead of Gloas    
      - Geth — 6 BAL State Transition Bugs (PR https://github.com/ethereum/go-ethereum/pull/33735)
      - Reth - missing ETH logs, gas refunds, swapn/dupn EIPs 
      - Geth - missing ETH logs eip
      - Erigon - missing ETH logs, gas refunds, slotnum, swapn/dupn EIPs
  - [epbs-devnet-0](https://notes.ethereum.org/@ethpandaops/epbs-devnet-0)
    - https://github.com/ethereum/consensus-specs/pull/4843 
    - implementation updates?
  - epbs-devnet-1 spec release: https://github.com/ethereum/consensus-specs/issues/4858

Hegota
 - headliner proposal deadline approaching, only 2 more days left

### Call Series

All Core Devs - Testing

### Autopilot Mode

- [x] Use autopilot (recommended defaults for this call series)


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

60 minutes

### Occurrence Rate

bi-weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Display Zoom Link in Calendar Invite (Optional)

- [ ] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2026-01-27T17:44:20Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27607)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=ay6kY5oOIeE)

--- COMMENT by potuz at 2026-01-28T21:51:47Z ---
Not sure if I'll be in the call, but I'd like this PR and the linked issue discussed there, I got the impression that on Discord there was consensus and builders didn't seem particularly concerned by it. https://github.com/ethereum/consensus-specs/pull/4875

--- COMMENT by qu0b at 2026-02-02T12:52:04Z ---
## bal-devnet-2 Latest Kurtosis Testing Summary 01.02.26


**1. Besu** — Remove `gasSpent` from Receipt RLP Trie Encoding

---

**2. Nethermind** — Remove `gasSpent` from Receipt RLP Trie Encoding

---

**3. Nethermind** — `engine_getPayloadV6` Missing `blobGasUsed` Field

https://github.com/NethermindEth/nethermind/pull/10376

Nethermind's `engine_getPayloadV6` response omits the `blobGasUsed` field. CL clients (Lodestar, Lighthouse) reject the payload with "blobGasUsed missing for gloas >= deneb executionPayload". Nethermind can validate blocks from other ELs but cannot propose.

---

**4. Nimbus-EL** — Gloas Opcodes Activated at Fulu Instead of Gloas

Causes chain split under fuzz transactions.

Nimbus activates Gloas-fork opcodes (`SLOTNUM` `0x4b`, and likely `DUPN`/`SWAPN`/`EXCHANGE` from EIP-8024) at the Fulu fork instead of the Gloas fork. Pre-Gloas transactions using these opcodes succeed on Nimbus but fail on Geth/Besu/NM, causing `gasUsed` mismatch and chain splits.

Reproduce: Contract creation tx using `SLOTNUM` at block 7 (pre-Gloas):
- Geth: `status=0x0` (FAIL), error "invalid opcode: SLOTNUM"
- Nimbus: `status=0x1` (SUCCESS), `gasUsed=57671`, contract deployed

Fix needed: Gate `SLOTNUM` and EIP-8024 opcode activation on the Gloas fork timestamp, not Fulu.

Note: Without fuzz transactions, Nimbus works perfectly in multi-client networks (all basic tests pass). The bug only manifests when random bytecode hits Gloas opcodes pre-fork.

---

**5. Geth** — BAL State Transition Bugs

https://github.com/ethereum/go-ethereum/pull/33735

---

**6. Lighthouse** — `upgrade_to_gloas` hardcoded slot number 0 fix

https://github.com/sigp/lighthouse/pull/8726


--- COMMENT by github-actions[bot] at 2026-02-03T18:26:51Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
