# ISSUE 2044: All Core Devs - Execution (ACDE) #237, May 21, 2026
Created: 2026-05-10T17:00:28Z by nixorokish

### UTC Date & Time

[May 21, 2026, 14:00 UTC](https://savvytime.com/converter/utc/may-21-2026/2pm)

Today's facilitator: @nixorokish 

### Agenda

- Glamsterdam
  - devnet updates (@barnabasbusa)
  - _(placeholder)_ EIP-7928 (BAL) [spec update](https://github.com/ethereum/EIPs/pull/11699/changes/780cc420f00de2ba21283b6c9a61fa95e978f7d6..daa60674d01258d86593bb20c130736ead879c77) - 7702 delegate inclusion on failed calls (@nerolation)
  - EIP-7904 update (@misilva73)
- Hegotá
  - [Propose](https://github.com/ethereum/pm/issues/2044#issuecomment-4418321118) EIP-8188: [State Tiering by Write Age](https://eips.ethereum.org/EIPS/eip-8188) (@weiihann) [[ slides ]](https://weiihann.github.io/20260521-eip8188-acd/)
  - [Propose]() EIP-8182: [Private ETH and ERC-20 Transfers](https://github.com/ethereum/pm/issues/2044#issuecomment-4435116340) (@RogerPodacter) [[ slides ]](https://github.com/0xFacet/eip-8182-reference-implementation/blob/main/EIP-8182-ACDE.pdf)
  - [Propose](https://github.com/ethereum/pm/issues/2044#issuecomment-4502437428) EIP-4758: [Deactivate SELFDESTRUCT](https://eips.ethereum.org/EIPS/eip-4758) (@petertdavies)
- Misc
  - [Client feedback on execution API cases](https://github.com/ethereum/pm/issues/2044#issuecomment-4501069405) (@bomanaps)

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

--- COMMENT by github-actions[bot] at 2026-05-10T17:01:24Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/85451723466?pwd=RgAVD0sO4OPFAU3UIqPflda1z05cRT.1)
✅ **Calendar**: [View](https://calendar.google.com/calendar/embed?src=c_upaofong8mgrmrkegn7ic7hk5s%40group.calendar.google.com&ctz=UTC&mode=AGENDA&dates=20260521%2F20260522&showTitle=1&showCalendars=0&showTabs=0&showPrint=0&showNav=0) | [Add to Calendar](https://www.google.com/calendar/render?action=TEMPLATE&text=All+Core+Devs+-+Execution+%28ACDE%29+%23237%2C+May+21%2C+2026&dates=20260521T140000Z%2F20260521T153000Z&details=Meeting%3A+https%3A%2F%2Fethereumfoundation.zoom.us%2Fj%2F85451723466%3Fpwd%3DRgAVD0sO4OPFAU3UIqPflda1z05cRT.1%0A%0AIssue%3A+https%3A%2F%2Fgithub.com%2Fethereum%2Fpm%2Fissues%2F2044)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/28485)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=EuUZbpMoIjM)

--- COMMENT by weiihann at 2026-05-11T07:11:26Z ---
I'd like to present and propose EIP-8188 for Hegota.
The specs: https://eips.ethereum.org/EIPS/eip-8188
The case: https://docs.fileverse.io/d/02001b770000#k=udWZ61QWdJASC3myxf5_0BU6yLMcsmHwqpXccRhhFh8

--- COMMENT by RogerPodacter at 2026-05-12T21:50:57Z ---
I'd like to present [EIP-8182: Private ETH and ERC-20 Transfers](https://eips.ethereum.org/EIPS/eip-8182) and get feedback on an inclusion path. Thanks!

- [Spec](https://eips.ethereum.org/EIPS/eip-8182)
- [Background](https://x.com/dumbnamenumbers/status/2047401379308745015)

--- COMMENT by misilva73 at 2026-05-20T13:23:01Z ---
I would like to give an update on [EIP-7904](https://eips.ethereum.org/EIPS/eip-7904), as part of the Glamsterdam fork section.

--- COMMENT by bomanaps at 2026-05-20T17:50:54Z ---
I would like to get client feedback on this two cases in Execution APIs   (1) PR porting eth_fillTransaction with  raw dropped from the result per the RPC Standards call Nethermind security concern https://github.com/ethereum/execution-apis/pull/803 , and (2) a cross-client RPC method matrix flagging methods implemented by 5-6 clients but not yet in spec https://hackmd.io/@bomanaps/rJXLPhYRWl 

--- COMMENT by petertdavies at 2026-05-20T20:36:56Z ---
STEEL would like to request CFI status for [EIP-4758: Deactivate SELFDESTRUCT](https://eips.ethereum.org/EIPS/eip-4758) in Hegota.

SELFDESTRUCT has been deprecated for a very long time now and [research](https://ethereum-magicians.org/t/can-we-completely-remove-selfdestruct/28464) shared by @chfast on the previous ACDE demonstrated that usage is low and the majority of that usage would not be broken by EIP-4758. We think that moving to CFI now gives advance notice to the tiny minority of users who still rely on account destruction and signals to Hegota EIP authors that they do not need to consider the interaction between their EIP and SELFDESTRUCT.

It would also be useful to reach some sort of consensus on what further research/outreach needs to be done order to be confident that SELFDESTRUCT can be safely deactivated. 

--- COMMENT by jochem-brouwer at 2026-05-20T22:58:50Z ---
Hiya, I want to discuss [EIP-8253: Upgrade pre-Spurious-Dragon accounts
](https://github.com/ethereum/EIPs/pull/11605) for Glamsterdam to replace PFId [EIP-7610: Revert creation in case of non-empty storage](https://eips.ethereum.org/EIPS/eip-7610).

--- COMMENT by jochem-brouwer at 2026-05-21T13:44:11Z ---
Hi @nixorokish I had some async discussions and I do not want to bring above point up anymore (can be removed from agenda), thanks!

--- COMMENT by github-actions[bot] at 2026-05-22T18:45:57Z ---
This meeting occurred more than 24 hours ago. Closing automatically.
