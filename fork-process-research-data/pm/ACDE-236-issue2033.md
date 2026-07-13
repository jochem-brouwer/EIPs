# ISSUE 2033: All Core Devs - Execution (ACDE) #236, May 7, 2026
Created: 2026-04-26T10:59:08Z by nixorokish

### UTC Date & Time

[May 07, 2026, 14:00 UTC](https://savvytime.com/converter/utc/may-7-2026/2pm)

### Agenda

Glamsterdam
- [Svalbard](https://blog.ethereum.org/2026/05/02/soldogn-interop-recap) progress + devnet updates (@barnabasbusa)
  - [bal-devnet updates & gas accounting ask](https://github.com/ethereum/pm/issues/2033#issuecomment-4397074196) (@qu0b)
- EIPs [to SFI?](https://github.com/ethereum/pm/issues/2033#issuecomment-4378026309) @danceratopz (+ nixo [SFI definition changes](https://github.com/ethereum/EIPs/pull/11475))
  - EIPs 7708, 7778, 7843, 7954, 7976, 7981, 8024, 8037 → SFI
- EIP-8070 [changes](https://github.com/ethereum/pm/issues/2033#issuecomment-4395749928) (@kamilsa) 
- [Clarify min-reorg depth without resync](https://github.com/ethereum/pm/issues/2033#issuecomment-4396370342) (@nerolation)
- EIP-8254: [Cap deposit requests per block](https://github.com/ethereum/pm/issues/2033#issuecomment-4396854129): with gas limit increase & without cap, a block with >8192 deposits produces a payload that CL clients fail to decode
- Propose EIP-8246: [Remove SELFDESTRUCT burn](https://github.com/ethereum/pm/issues/2033#issuecomment-4370105186) (@chfast)
- Can we [remove SELFDESTRUCT](https://ethereum-magicians.org/t/can-we-completely-remove-selfdestruct/28464)?

Hegotá
- Propose EIP-7709: [Read BLOCKHASH from storage and update cost](https://github.com/ethereum/pm/issues/2033#issuecomment-4353547307) (@jsign) [[ slides ]](https://docs.google.com/presentation/d/1raEGijf92cUBeCGeTzQusItvIVUAjRcLzzlXtgL9jcU/edit?usp=sharing)
- Propose EIP-8253: [Remove pre-Spurious-Dragon accounts](https://github.com/ethereum/pm/issues/2033#issuecomment-4392380260) (@jochem-brouwer) [[ slides ]](https://github.com/user-attachments/files/28059625/EIP-8253_.Remove.pre-SpuriousDragon.accounts.pdf)
- AA update (@lightclient)

Misc
- Engine API v2 [schema fix](https://github.com/ethereum/pm/issues/2033#issuecomment-4369079002) (@bomanaps)
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

--- COMMENT by github-actions[bot] at 2026-04-26T11:00:05Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260507%2F20260508&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Execution+%28ACDE%29+%23236%2C+May+7%2C+2026&dates=20260507T140000Z%2F20260507T153000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F85451723466%3Fpwd%3DRgAVD0sO4OPFAU3UIqPflda1z05cRT.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2033)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28353)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=19Hi3SbT6tc)

--- COMMENT by jsign at 2026-04-30T14:57:53Z ---
I would like to propose [EIP-7709](https://eips.ethereum.org/EIPS/eip-7709) for Hegota:
- The current EIP is a bit stale; see [this PR](https://github.com/ethereum/EIPs/pull/11587), which proposes a refresh (under review).
- To help navigate the proposal consideration, I wrote a [document](https://hackmd.io/@jsign/EIP-7709-Hegota-proposal) with a deeper dive into many angles around it.

--- COMMENT by bomanaps at 2026-05-04T07:33:42Z ---
This pr https://github.com/ethereum/execution-apis/pull/781 is a tiny schema-only fix (V2 result becomes oneOf [array, null], matching what osaka.md already mandates and how V3 is already encoded), so flagging it for ACDE since engine API changes can't be discussed on the RPC call would appreciate a quick approval from core devs to move it forward.

--- COMMENT by chfast at 2026-05-04T09:58:12Z ---
I would like to propose [EIP-8246: Remove SELFDESTRUCT Burn](https://github.com/ethereum/EIPs/pull/11590) for Glamsterdam.

--- COMMENT by danceratopz at 2026-05-05T09:27:13Z ---
Once again, STEEL would like to discuss which EIPs can be SFId for Glamsterdam 😆 

Edit: Here's a suggestion as a basis for discussion:
https://github.com/ethereum/EIPs/pull/11399/changes


--- COMMENT by jochem-brouwer at 2026-05-06T21:38:56Z ---
I want to propose [EIP-8253: Remove pre-Spurious-Dragon accounts](https://github.com/ethereum/EIPs/pull/11605) for Hegota (although PR is big the idea is simple, should not need much time to explain this one) as alternative, or follow-up for [EIP-7610: Revert creation in case of non-empty storage](https://eips.ethereum.org/EIPS/eip-7610) 

--- COMMENT by kamilsa at 2026-05-07T09:10:07Z ---
Now that https://github.com/ethereum/execution-apis/pull/774 is merged, we need to reflect corresponding changes in the EIP-8070 (Sparse blobpool): https://github.com/ethereum/EIPs/pull/11444

I'd like to bring this PR to attention on the call 

--- COMMENT by chfast at 2026-05-07T10:29:19Z ---
[Can we completely remove SELFDESTRUCT?](https://ethereum-magicians.org/t/can-we-completely-remove-selfdestruct/28464)

--- COMMENT by nerolation at 2026-05-07T10:37:47Z ---
I'd like to discuss the min-reorg depth that ELs should support without triggering re-sync.
Based on discussions during interop, we put it into an EIP and want to further discuss it. 
https://github.com/ethereum/EIPs/pull/11601

This is something we should have already clarified before and doesn't necessarily need a hardfork for.


--- COMMENT by akashkshirsagar31 at 2026-05-07T11:41:00Z ---
X Stream: https://x.com/i/broadcasts/1AKEmOyVmnlKL

--- COMMENT by barnabasbusa at 2026-05-07T11:52:29Z ---
Would like to propose [EIP-8254](https://github.com/ethereum/EIPs/pull/11607) for glamsterdam. 

This EIP is currently only considers the fact that the EL needs to limit the number of deposits we can include in a slot. 
The exact number of what this limit should be needs to be discussed. With 200M gas we could exceed the 8192 limit. 
However, having such a high limit seems excessive anyways. Based on historical data we didn't have a single slot with over 1000 deposits in them. 

```sh
slots with >251 deposits

Slot        Deposits
13509034    335
13210333    692
13210332    400
12357246    328
12258228    648
12151075    383
12151074    400
12148580    470
```

696 is the max txs we can fit in a single tx with the current tx cap of 16.7M gas. 

I would actually like to propose lowering the 8192 -> 512 in gloas and enforcing this limit on the EL side as well. Since genesis we only had 2 slots that have exceeded this. 

With 512 deposits we still have ~$1M worth of deposits at worst case (1ETH deposits) or $32M worth of deposits on avg case. 

--- COMMENT by qu0b at 2026-05-07T12:25:50Z ---
- `bal-devnet-6` Multiple bugs found tracked here: https://github.com/ethereum/execution-specs/issues/2804. Spec clarifications: https://github.com/ethereum/EIPs/pull/11611/
- `bal-devnet-7` end of next week, which will include `bal-devnet-3` optimizations and `bal-devnet-6` fixes and constant updates https://github.com/ethereum/EIPs/pull/11616. This should be the last EL only devnet. 
- Ask to clients: Should we add more gas accounting information to the transaction receipts or how should we make it easier to debug gas accounting differences between clients. Bens proposal would be `debug_getBlockReceipts`. 

response could look something like:
```json
[...
  {                                                                                                                                                                                                                                                  
    "...": "all standard receipt fields preserved",                                                                                                                                                                                                
    "gasUsed": "...",                                                                                                                                                                                                                                
    "cumulativeGasUsed": "...",                            
                                                                                                                                                                                                                                                     
    "regularGasUsed":             "...",                                                                                                                                                                                                             
                                                          
    "stateGasCharged":            "...",                                                                                                                                                                                                             
    "stateGasRefunded":           "...",                                                                                                                                                                                                           
                                                                                                                                                                                                                                                     
    "cumulativeRegularGasUsed":   "...",                       
    "cumulativeStateGasCharged":  "...",                                                                                                                                                                                                             
    "cumulativeStateGasRefunded": "..."                                                                                                                                                                                                            
  }
...]
```

--- COMMENT by nflaig at 2026-05-07T14:05:53Z ---
maybe can get a temp check from EL devs on https://github.com/ethereum/execution-apis/pull/608 (can discuss async on discord [here](https://discord.com/channels/595666850260713488/688075293562503241/1501977234078961774))

--- COMMENT by github-actions[bot] at 2026-05-08T18:40:34Z ---
This meeting occurred more than 24 hours ago. Closing automatically.

--- COMMENT by jochem-brouwer at 2026-05-20T13:24:14Z ---
Slides used for the EIP-8253 presentation

[EIP-8253_ Remove pre-SpuriousDragon accounts.pdf](https://github.com/user-attachments/files/28059625/EIP-8253_.Remove.pre-SpuriousDragon.accounts.pdf)
