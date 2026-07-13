Done. Here is the baseline meta-summary of Fusaka-era devnet cadence/process discussion from ethereum/pm (all read-only; nothing posted).

# Fusaka devnet-era (Apr–Oct 2025) meta-discussion baseline — ethereum/pm

## Devnet timeline as reconstructed from pm issues
- **peerdas-devnet-7** — launched May 9, Fulu forked May 13 (ACDT #36, issue #1528)
- **fusaka-devnet-0** — launched May 26 (planned at ACDT #37, #1543; status ACDT #39, #1561)
- **fusaka-devnet-1** — target June 9; shut down ~June 17 (ACDT #40, #1573 notes)
- **fusaka-devnet-2** — activated ~June 23, assembled at **Berlinterop** (planned as "EL/CL Interop, June 8–14" in ACDT #36/#37 schedule tables)
- **fusaka-devnet-3** — spec finalized around ACDC #160 (July 10, #1598); launched late July; deliberately **long-running** ("will continue running throughout the testnet upgrades", ACDT #55 #1736); suffered non-finality incidents in Aug/Sep
- **fusaka-devnet-4** — explicitly not planned, then briefly happened anyway (see below)
- **fusaka-devnet-5** — planned Sep 3 (ACDE #219, #1687), launched Sep 10 (ACDT #53, #1719); explicitly the **testnet-gating devnet**
- Testnets: Holesky Oct 1, Sepolia Oct 14, Hoodi Oct 29 (ACDT #55, #1736; ACDC #165, #1716)

## Fork/devnet cadence meta (the strongest baseline material)
- **ACDT #36 (#1528), benaadams, May 12**: "One issue with historic fork timelines is they are so infrequent that they need to be jammed with EIPs… Hopefully with principle of adopting a faster fork cadence, EIPs can be included more frequently and the pressure to stuff forks reduced (e.g. Fusaka 6 months after Pectra, and Glamsterdam perhaps 6 months after Fusaka)."
- **ACDC #164 (#1700), rolfyone (Teku), Sep 4** — key complaint that rollout eats the cycle: 30d release→mainnet + 30d release→first testnet + ~30d between testnets = "3 months of a potential 6 month cycle as the road to mainnet is not practical… **Something has to give. If we want this release timeframe, then I don't see how we can maintain a 6 month cadence as well.**"
- **lightclient, same issue**: "Agree… no way we can maintain 6 month fork cadence. It just isn't possible with the rollout strategy. However I don't think we can adjust the rollout strategy much… Too many people and companies depend on Ethereum and need to actively prepare for the fork."
- **timbeiko, pm PR #1715 (merged)** — response: update the Protocol Upgrade Process doc after consulting L2s/LSTs/infra: don't couple testnet+mainnet forks in one release; **min 14 days release→first testnet; min 10 days (ideally ~2 weeks) between testnets; min 30 days release→mainnet**. Notes the original testnet timelines "were somewhat contentious and we never finalized our discussion on ACD."

## What gated devnet launches
- **Spec + test releases per devnet**: each devnet shipped with a consensus-specs alpha tag + an EEST `fusaka-devnet-N@vX` release and an ethpandaops spec page (`notes.ethereum.org/@ethpandaops/fusaka-devnet-N`). E.g. devnet-0: consensus-specs v1.6.0-alpha.0 + `fusaka-devnet-0@v1.0.0` (ACDT #39, #1561).
- **Not all clients required**: devnet-0 planning — "Agreement to proceed even without all clients ready at launch" (ACDT #37 notes, will-corcoran).
- **Devnet-3 as the spec-freeze devnet**: raulk (ACDT #44, #1609, Jul 14): "As we push for Fusaka spec freeze…"; ralexstokes (ACDE #216, #1610, Jul 15): "the rest of July to get any last minute spec things sorted and devnet-3 live and healthy, all of August can be devoted to hardening implementations, and then we move to the deployment pipeline in September."
- **Testnet gates**: nalepae (ACDE #216): "before running any testnet, we should at least: ensure all clients can sync in the 'perfect peerDAS' configuration… ensure all clients perform well without engine_getBlobsV2."
- **Devnet-5 gated testnet dates**: abcoathup (ACDC #165, #1716): "Dates won't be set until successful testing on fusaka-devnet-5"; devnet-5 outcomes also confirmed the BPO (target, max) values (ACDE #219 timeline). ACDC #165 proposal: **code freeze Sep 22 → "PandaOps team can run some validation on frozen code that would serve as release candidate"** → testnet releases Sep 25.
- **Config verification as a gate**: for devnet-5 and testnets, EEST/STEEL verified client configs pre-launch via `eth_config` / `execute config` (ACDT #53 #1719, #55 #1736); Hive Fusaka dashboards (~20,000 tests) tracked as launch-readiness signal (ACDT #53).

## "No more devnets" / skipped-devnet signals
- **barnabasbusa (ACDC #160, #1598, Jul 10)**: "**There are no plans to have devnet 4.**" (in response to whether CLZ/tx-cap changes were devnet-3 or devnet-4 material — everything got folded into devnet-3). Yet ACDC #163 (#1673, Aug 21) agenda has "devnet-3 (?), devnet-4 reflections; devnet-5 plans" — a short-lived devnet-4 happened anyway in mid-Aug, then devnet-5 was re-planned as the final one.
- Devnet-5 was implicitly the last devnet before testnets: ACDE #219 timeline (ralexstokes, Aug 28): "3 Sep: fusaka-devnet-5 launch; 10 Sep: fusaka-devnet-5 done → use analysis to confirm (target, max) values; 15 Sep: Holesky/Sepolia releases; bug bounty begins."

## Interop events
- **Berlinterop (June 2025)** was a real cadence accelerator: devnet-1 wound down and devnet-2 assembled during the event. ACDT #40 notes (via ethereum-magicians thread 24549): "devnet-2 interop: **Faster process in order to test, but the changes included will now have to use ACD process to be officially included in Fusaka.**" Sunnyside Labs presented "devnet results from Berlin interop" (ACDT #41, #1583).

## pandaops capacity signals (thin in pm issues)
- No explicit "capacity/fatigue" complaints found in issue text (that discussion likely happened on calls/Discord). Closest signals: **watchtower (auto image updates) not run for devnet-5** — "If you need your client image to be updated, reach out to one of the pandas" (ACDT #53, #1719); devnet-3 kept alive long-term as background infra; barnabasbusa maintained a curated syncing-bug list after devnet-3 non-finality (notes.ethereum.org/@ethpandaops/fusaka-syncing-bugs, ACDC #164).

## Call cadence itself
- ACDT ran **weekly** throughout the entire Fusaka era (#36 May 12 → #58 Oct 20); some issue templates flip between "weekly"/"bi-weekly" metadata but no explicit discussion of changing ACDT frequency appears in the issues.
- Meta note on call load: timbeiko (ACDE #219, #1687, Aug 26) flagged that Glamsterdam PFI presentations would consume "2–4 full ACDEs" while "teams are now sprinting towards Fusaka releases" — an explicit statement that fork-N testing crowds out fork-N+1 planning.

Key issues for later comparison: #1528, #1543, #1561, #1573, #1583, #1598, #1609, #1610, #1638, #1673, #1687, #1700 (+PR #1715), #1716, #1719, #1736. Raw JSON dumps are in the scratchpad (`.../scratchpad/acdt/`) if you want follow-up quotes. Notable gap: no explicit "too many devnets" or "devnet fatigue" complaint exists in pm issue text for Fusaka — the fatigue discussion was framed as *rollout-timeline vs 6-month-cadence* tension (ACDC #164), which is the cleanest baseline to compare Glamsterdam against.
