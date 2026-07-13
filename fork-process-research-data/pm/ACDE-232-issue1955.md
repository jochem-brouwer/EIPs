# ISSUE 1955: All Core Devs - Execution (ACDE) #232, March 12, 2026
Created: 2026-03-02T14:33:39Z by nixorokish

### UTC Date & Time

[March 12, 2026, 14:00 UTC](https://savvytime.com/converter/utc/mar-12-2026/2pm)

### Agenda

- Glamsterdam
  - devnet updates
    - bal-devnet-2
    - bal-devnet-3
      - [EIP-8037 concerns](https://github.com/ethereum/pm/issues/1955#issuecomment-4045214383) by @yperbasis 
    - epbs-devnet-0
- Hegotá
  - headliner selection
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

--- COMMENT by github-actions[bot] at 2026-03-02T14:34:45Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=MDJjdTFiMWcxbXNjdW52N2QzcDBpbGczZ2cgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27880)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=3owz0r4Fv68)

--- COMMENT by Giulio2002 at 2026-03-09T12:47:15Z ---
I would like to add https://ethresear.ch/t/binary-ssz-transport-for-the-engine-api-an-initial-benchmark-on-a-live-network-with-kurtosis/24324 and https://github.com/ethereum/execution-apis/pull/764 to the agenda to discuss SSZ on the engine API and my experiments on implementing it in Prysm, Lighthouse, Lodestar, Teku, Erigon, Geth and Nethermind

https://eips.ethereum.org/EIPS/eip-8178

--- COMMENT by spencer-tb at 2026-03-10T14:57:54Z ---
Can we SFI all the EIPs included on the devnets following: https://eips.ethereum.org/EIPS/eip-7723
Otherwise should we change the devnet/SFI section to align with the process currently?

https://github.com/ethereum/EIPs/pull/11399

--- COMMENT by nerolation at 2026-03-12T08:40:19Z ---
> Can we SFI all the EIPs included on the devnets following: https://eips.ethereum.org/EIPS/eip-7723 Otherwise should we change the devnet/SFI section to align with the process currently?

Would this mean that the process of getting an EIP from CFI to SFI is based on an EIP being put on a devnet (which doesn't really follow any specified process), independently of [client priorizations](https://forkcast.org/priority) and [community feedback](https://x.com/EFprotocol/status/2031056150427242892)?
I interpreted it more as "EIPs can be put on a devnet but then still DFI'd", so we would, in another iteration, decide upon moving EIPs from CFI to SFI based on seeing they work on a devnet + ongoing support by community and devs.

--- COMMENT by yperbasis at 2026-03-12T09:25:45Z ---
We (Erigon) think that we should take out [EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) out of Glamsterdam or re-design it significantly. It adds the 4th dimension to the gas so that the whole gas logic becomes a horrible mess impossible to reason about. The four gas dimensions I'm talking about:

1. [EIP-4844](https://eips.ethereum.org/EIPS/eip-4844) introduces blob vs regular gas.
2. [EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) (BALs) introduces stateful vs non-stateful gas.
3. [EIP-7778](https://eips.ethereum.org/EIPS/eip-7778) introduces block accounting vs user refunds gas.
4. [EIP-8037](https://eips.ethereum.org/EIPS/eip-8037) introduces state vs regular gas.

--- COMMENT by misilva73 at 2026-03-12T11:13:43Z ---
@yperbasis , can you provide more context on which part of EIP-8037 you are concerned about? Is is the dynamic cost per byte (and increasing gas costs for state creation in general) or the multidimensional metering?

--- COMMENT by yperbasis at 2026-03-12T11:35:11Z ---
> [@yperbasis](https://github.com/yperbasis) , can you provide more context on which part of EIP-8037 you are concerned about? Is is the dynamic cost per byte (and increasing gas costs for state creation in general) or the multidimensional metering?

It's not just EIP-8037, which might want to do a reasonable thing on if you take it in isolation. The problem is that all the EIPs I mentioned (4844, 7928, 7778, 8037) add hacks to the concept of gas in Ethereum, and taken together turn it into a hairy beast with multiple dimensions and flavours, impossible to reason about. We should take a step back and design gas changes in the protocol holistically ­– otherwise there'll be a gazillion of corner cases and subtle bugs that'll haunt EL implementations.

--- COMMENT by akashkshirsagar31 at 2026-03-12T11:45:17Z ---
X Stream: https://x.com/i/broadcasts/1RJjpzDEELgKw

--- COMMENT by fjl at 2026-03-12T13:45:09Z ---
About EIP-8141 (Frame Transaction), here is what we have been busy with since the EIP was presented on ACD:

- We had a [breakout call](https://forkcast.org/calls/one-off-1954/001) on March 5, where people had a chance to voice their wider concerns about the purpose of the EIP.

- We have addressed backwards-compatibility concerns with the APPROVE opcode by changing it to not set an EVM return status other than zero or one.

- The EIP was [experimentally implemented](https://github.com/lightclient/execution-specs/pull/1) in execution-specs to enable internal testing.

- While working on account implementations for testing, we found that it is often necessary to pass the APPROVE scope into the account via the transaction. To facilitate this while also allowing accounts to cleanly sign over the sighash of the transaction, *approval scope bits* were [added to the frame `mode` field](https://github.com/ethereum/EIPs/pull/11401).

- On a related note, the transaction parameter access opcodes were [cleaned up a bit](https://github.com/ethereum/EIPs/pull/11400).

- Recently the concept of a *default account* was [introduced into the EIP](https://github.com/ethereum/EIPs/pull/11379). The precise semantics of the default account are still being discussed, so this is more of a proof-of-concept addition for now. The idea of the default account is allowing existing EOA users to immediately benefit from UX improvements such as gas sponsoring and native batching. It also creates a potential upgrade path to UX, where the protocol can adapt the behavior of the default account in future forks to ship improvements to existing accounts. 

- More generally, we have been collecting a lot of feedback from wallet implementers. From many people, we have heard the message that the Frame Transaction should provide native atomic batching, e.g. for ERC-20 `approve`+`transfer` flows. We are currently [prototyping an addition](https://github.com/ethereum/EIPs/pull/11395) to the EIP that enables this. The precise mechanism is still under discussion though, this is just to say we are working on an atomic batching facility and consider it a valuable addition to the EIP. 

- We are also currently working on a built-in upgrade path for ERC-4337 smart account which are already deployed, making them usable as EOAs right away.


--- COMMENT by github-actions[bot] at 2026-03-13T18:18:24Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
