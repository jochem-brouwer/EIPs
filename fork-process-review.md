# Glamsterdam EL Devnet Review — Could We Have Had Fewer Devnets?

Research conducted 2026-07-14 with Claude Code. Follow-up to `glamsterdam-fork-process-research.md` (2026-07-06). All data gathered read-only from ethpandaops HackMD notes and config repos (`bal-devnets`, `glamsterdam-devnets`, `epbs-devnets`), `ethereum/pm` (ACDT #62–87, ACDE #225–240), `ethereum/execution-specs` (EELS) and `ethereum/execution-spec-tests` (EEST) releases/issues/PRs, `ethereum/EIPs` git history and PRs, and client bug trackers. Raw snapshots of the downloaded material are archived in [fork-process-research-data/](fork-process-research-data/README.md) (see §9). **Nothing was published anywhere.**

## 1. Question

For each Glamsterdam devnet (EL focus): how did the EIP specs change between devnets, and could the devnet have been cut by a tighter feedback loop with the testing team (EELS Python spec + EEST tests)? Secondary: the EIP-interaction problem — EIP authors write standalone documents, but a fork composes them, and the composition is where the surprises live.

## 2. Corrected devnet inventory

The earlier research used ACDT-announcement dates; genesis configs (`MIN_GENESIS_TIME` in the ethpandaops repos) correct several of them:

| Devnet | Genesis (verified) | EL delta vs prior devnet |
| --- | --- | --- |
| bal-devnet-0 | 2025-11-04 | 7928 only, EEST `bal@v1.8.0` |
| bal-devnet-1 | 2025-12-18 | 7928 "spec 2.0" type/encoding changes (specs#1912), EEST `bal@v2.0.0` |
| bal-devnet-2 | 2026-02-06 | + 7708, 7778, 7843, 8024; 7928 updates; EEST `bal@v5.1.0` |
| bal-devnet-3 | **2026-04-08** (not Mar 4) | + 8037 (static CPSB=1174), 7954, 7975/8159 optional; EEST `bal@v5.6.0` |
| bal-devnet-4 | **never launched** | prepared (dynamic-CPSB 8037, +7976, +7981), genesis set for Apr 27, renamed to devnet-5 after last-minute 8037 re-spec |
| bal-devnet-5 | 2026-04-29 | devnet-4 scope with 8037 **static CPSB + frame-boundary accounting** (EIPs#11573) |
| bal-devnet-6 | 2026-05-01 | **identical EIP set**, 8037 switched to **opcode-level accounting** — 44 h after devnet-5 |
| bal-devnet-7 | 2026-05-19 | 8037 recalibration (CPSB 1530, byte constants, 8 spec fixes for devnet-6 bugs); eth/70 + eth/71 mandatory; 150M gas; "last EL-only devnet" |
| glamsterdam-devnet-0 | 2026-04-29 (not Apr 24) | first merged EL+CL (Gloas alpha.5); EL ≈ bal-devnet-3/5 set |
| glamsterdam-devnet-1 | 2026-04-29 (22:35) | **spec-identical relaunch** of devnet-0 (<21 h later) |
| glamsterdam-devnet-2 | 2026-05-01 | CL alpha.5→alpha.7, +8061 (CL); **no EL delta** |
| glamsterdam-devnet-3 | 2026-05-06 | **spec-identical relaunch** of devnet-2 |
| glamsterdam-devnet-4 | 2026-05-22 | bal-devnet-7 EL set + `targetGasLimit` engine field; CL alpha.8 — chain split ~May 25 |
| glamsterdam-devnet-5 | 2026-06-04 | relaunch of devnet-4 scope + 8045 (CL), 8189 optional; **no EL EIP delta**; CL stability mandate |
| glamsterdam-devnet-6 | 2026-06-25 | **largest EL delta of the series:** + 2780 (reworked), 8038, 8246, 7997, 8282, 8070 opt., 7610 explicit; 7954→64 KiB; 8037 source-based refunds; 4 × 7928 clarifications; EELS `glamsterdam-devnet@v6.1.1` |
| glamsterdam-devnet-7 | mid-Jul 2026 (deploying) | + 7688 (CL); new 8282 deposit address; 7928 stipend check (#11854); 2780 runtime-charging (#11844); 8038 SSTORE ordering fix; 7976 block-accounting alignment (#11908) |

Two structural facts fall out immediately: **bal-devnet-4 was cut** (proof that skipping a devnet was possible when someone proposed it — MariusVanDerWijden, ACDT #78), and **three of the sixteen launched networks were spec-identical relaunches** (gd-1, gd-3, plus gd-5 which was a relaunch with only CL additions).

## 3. Per-devnet verdicts (EL perspective)

Classification of each launch: **KEEP** (genuine devnet-only value), **MERGE** (should have been folded into an adjacent devnet), **AVOIDABLE** (existed only to absorb a spec iteration that a tighter spec/test loop would have resolved off-network), **RELAUNCH** (operational/CL failure, no EL content).

### bal-devnet-0 — KEEP

First interop of the headliner. Multi-client BAL exchange over engine API + gossip is exactly what devnets are for. No EL spec issue on record from the network itself; the self-destruct-OOG problem that "affected multiple clients" (specs#1846) came from **test filling**, not the chain.

### bal-devnet-1 — AVOIDABLE

Trigger was purely upstream spec churn: 7928 type re-encoding (storage keys/slots to uint256, specs#1912) and RLP reference updates. No findings are documented from the network. This devnet exists because 7928 was SFI'd (2025-08-14) with its encoding still undecided — the SSZ→RLP switch landed **two days after SFI** and the type definitions kept moving through November. Had the encoding question been closed before bal-devnet-0 (it was a pure design decision, not something a network run could inform), devnet-0 would have run on the final encoding and devnet-1 has no reason to exist. **Root cause: SFI before design freeze, not a missing devnet.**

### bal-devnet-2 — KEEP, but launched on an unfrozen spec

The feature-batch expansion (7708/7778/7843/8024) is a legitimate devnet trigger, and it produced real integration-only findings: Nimbus activating Gloas opcodes at Fulu → chain split under fuzzed load; Nethermind `engine_getPayloadV6` omitting `blobGasUsed`; six geth BAL bugs under fuzzed multi-client runs. None of those are catchable by EELS alone.

The avoidable part: it launched amid the **7778 `gasSpent` receipt-field thrash**. The field was added to the EIP 2026-01-19, EEST `bal@v4.0.0` shipped with it while the spec was contested — fselmo flagged on ACDT #67 (Jan 24): "this is a consensus issue on receipts roots so no client can test against this latest test release" — ACDT reverted it Jan 26, the EIP text followed Jan 28, and the testing team re-filled the **entire suite the same day** (`bal@v5.0.0`). Besu and Nethermind had already implemented the doomed field and had to back it out as a devnet blocker. The feedback loop caught this *before* genesis; the process just didn't require the spec to be settled before clients started building against it.

### bal-devnet-3 — KEEP the devnet, not its four-week detour

First 8037 devnet; eventually the long-lived benchmarking baseline. It hosted the **only documented EL-spec chain split of the whole series** — root cause: ambiguous 8037 spillover classification (fixed by EIPs#11522). Real client bugs were found around it (Nethermind 1D gas budget, Besu reservoir-on-halt, geth nested-CREATE restore, Erigon 2D pool).

But the record shows most of the pain was *upstream* of the network and pre-known:

- spencer-tb reported on ACDT #71 (**Feb 23**) that dynamic `cost_per_state_byte` broke test determinism (~30 % of tests; every filled fixture depends on fill-time gas limit); Marius proposed postponing 8037 rather than butchering the framework.
- Erigon's architectural objection ("gas becoming 4-dimensional… impossible to reason about") was on ACDE #232, **Mar 12** — before genesis.
- The pre-launch "cross-client bug hunt" caught geth/besu/nimbus-eth1 8037 bugs **before genesis** (go-ethereum#33972, besu#9994, nimbus-eth1#4036).
- The spillover ambiguity that split the chain is a specification defect of exactly the kind EELS implementation review had been surfacing all spring; the split forced the clarification but did not discover anything conceptually new.

The devnet slipped from ~Mar 11 to Apr 8 waiting on the test framework to absorb a design (dynamic CPSB) that was **reverted three weeks after launch anyway** (EIPs#11573, Apr 27) — after misilva73 asked on the testing team's revision proposal (#11570): "Would a fixed cpsb sort the testing complexities and issues that this proposal is trying to address?" That question was answerable in November 2025.

### bal-devnet-4 (unlaunched) + bal-devnet-5 + bal-devnet-6 — one devnet's worth of value, three devnets' worth of cost — AVOIDABLE ×2

This 5-day sequence (Apr 27 – May 1, Soldøgn interop) is the purest "devnet as spec-iteration vehicle" episode:

1. devnet-4 prepared with **dynamic CPSB** → EELS/EEST framework rebuilt to thread block-gas-limit through every gas function (specs#2687, touching the whole fixture surface) →
2. dynamic CPSB **reverted to fixed 1174** six days later; devnet-4 renamed devnet-5, launched with **frame-boundary accounting** (EIPs#11573) →
3. 44 hours later devnet-6 launches with the **identical EIP set** but **opcode-level accounting**.

Three accounting models in five days. The EEST release for devnet-5 (`snobal-devnet-5@v8037.0.0`) has release notes consisting of a row of frustrated emoji — the testing team's on-the-record editorial. The "multiple bugs" credited to bal-devnet-6 (specs#2804) were reported by rakita, benaadams and ethrex from **differential testing around the devnet branches**, were all exact-shape EEST coverage gaps (benaadams enumerated the missing fixtures, each off by exactly `AccountCreationCost = 131,488`), and became tests + spec PRs within six days. Nothing in this episode required a live network; it required the accounting-model decision to be made once, in the repricing breakout (which had existed since Feb 4), with EELS as the arbiter.

### bal-devnet-7 — KEEP; the strongest evidence for what devnets are actually for

Consolidation + final parameters (CPSB 1530) + eth/70/eth/71 made mandatory + 150M gas. Its devnet-only yield is exactly the irreducible core:

- **Live benchmarking** for the repricing numbers (clients quoting ~3× exec speed-up vs devnet-3 baselines; Nethermind OOM distorting results; Besu AOT fix) — not reproducible locally at fidelity.
- **Builder-vs-validator BAL divergence**: geth's 7702 re-delegation had the block builder charge 35,190 state-gas where the validator charged 0 → diverging BAL hash and state root. The ACDT #83 agenda names the systemic reason this class *only* shows up on devnets: "**EELS does payload re-execution only, no block-building tests — which is why this slipped**" (fixed by specs#2945 adding block-building/rejection tests). This is the single clearest devnet-only EL finding of the fork — and it is simultaneously a to-do: once EELS has building-path tests, this class moves off-network too.
- Networking (eth/70/71) compliance forcing — though note the devnet was explicitly used as a deadline device ("Would be great if clients could confirm work on eth/70 and eth/71 is done").

### glamsterdam-devnet-0/1/2/3 — CL-driven; EL is a free rider — no EL verdict

Four networks in eight days with **zero EL EIP delta** between them (gd-1 and gd-3 are spec-identical relaunches; gd-2 is a consensus-specs alpha bump). From the EL point of view these cost little and taught little; their justification and their failures (validator misconfig at gd-0 genesis, unexplained gd-2 degradation, the gd-3 finality stall from epoch 492) are CL/ePBS/infra matters. They do, however, illustrate that the *series count* ("devnet 7!") overstates EL iteration — and that relaunch-vs-recover decisions were being made without published post-mortems (neither gd-0/1 nor gd-2/3 has a stated failure cause in the notes).

### glamsterdam-devnet-4 — KEEP (first unified EL+CL on near-final EL)

EL delta vs bal-devnet-7: none ("all EIP updates were already incorporated") except the `targetGasLimit` engine field. The chain split (~slot 22800, May 25) was a **Gloas fork-choice spec defect** (`should_build_on_full()` vs `get_head()` after skipped slots, consensus-specs#5307) — real devnet-only discovery, but consensus-side; EL testing could not have prevented it.

### glamsterdam-devnet-5 — RELAUNCH (CL)

Devnet-4 replacement under an ACDC "stability first, no scope increase for 2 weeks" mandate. No EL EIP delta. Findings all CL-side (Prysm peering, Grandine fork-choice, Lodestar peer-scoring self-starvation). Notable process signal: the builder path had **no local harness at all** — terencechain on ACDT #82: "Any latest update on builder API integration into kurtosis… Or just yolo it on devnet?"

### glamsterdam-devnet-6 — the anti-pattern made flesh — AVOIDABLE in its actual form

Six-plus EIPs (2780 rework, 8038, 8246, 7997, 8282, 8070) added **after** SFI (May 11) and after ACDE #238 declared scope "closed" (Jun 4), on a spec snapshot finalized days before genesis: the 2780 rework and 8038's first-ever real numbers merged **Jun 18**, EELS merged the same day, tests released **Jun 19**, devnet launched **Jun 24/25**. The 8038 net-profitable-refund bug (EIPs#11823) was found by **spec review two days before launch** — EELS had implemented it correctly and required no change, i.e. the pipeline out-ran the devnet again. The devnet's genuine unique find so far is Erigon's builder sealing wrong state roots under ePBS payload-orphan reorg churn (erigon#22254) — again the building-path class.

A verification devnet for the full scope was always going to be needed; what was avoidable is *this* devnet-6 — carrying a week-old, first-composition spec basket. In a process where the repricing basket (2780/8038/8246) had converged before SFI — or been deferred to the next fork, as the earlier research argued — devnet-6 is a boring verification run and devnet-7 doesn't exist.

### glamsterdam-devnet-7 — MERGE (with a healthy devnet-6)

Its EL delta is entirely pre-identified spec work: 7928 stipend check (EELS fix #3064 opened Jun 30 **before** the EIP fix #11854, Jul 2), 2780 runtime-charging restructure, 8038 SSTORE ordering, 7976 alignment, new 8282 deposit address. In the agent's words from the ACDT record: "a verification round, not a discovery round" — necessary only because devnet-6 shipped an unstable basket. The testing team shipped **three releases in three days** (Jul 8/9/10) to chase the churn.

## 4. Scorecard

EL-driven launches: bal-0, 1, 2, 3, 5, 6, 7 + gd-6 + gd-7 = **9 spec iterations** (bal-4 prepared and cut; gd-0..5 CL-driven from the EL seat).

| Verdict | Devnets | Count |
| --- | --- | --- |
| KEEP (devnet-only value) | bal-0, bal-2, bal-3 (core), bal-7, gd-6-as-verification | 5 |
| AVOIDABLE (spec iteration off-network) | bal-1, bal-5 *or* bal-6, gd-7 (given healthy gd-6) | 3 |
| Cost hidden in KEEPs | bal-3's 4-week slip + framework rebuild for a reverted design; bal-2's `gasSpent` blocker; gd-6's week-old basket | — |

**Counterfactual EL schedule: ~5 devnets instead of 9**, plus roughly two months of calendar (the bal-3 slip and the gd-6→gd-7 respin), *without any new tooling* — only with three decisions made earlier: 7928 encoding before devnet-0, 8037 accounting model (fixed CPSB, one accounting level) before its first devnet inclusion, and the repricing basket frozen-or-deferred at SFI. Each of those three was answerable from information the testing team and client implementers already had at the time (specs#1846/#2067 feedback Sep 2025; ACDT #71 determinism report Feb 2026; #11570/#11573 Apr 2026).

## 5. The feedback loop: it was fast — it was pointed at the wrong stage

The hypothesis going in was "more/faster testing-team feedback could have replaced devnets". The record refines this: **the loop's latency was never the problem.**

- 7778 receipt revert: ACDT decision in the morning, EIP + full suite re-fill **the same day** (Jan 26).
- devnet-6 basket: EIP merge Jun 18 → EELS merge Jun 18 → test release Jun 19 → genesis Jun 24. One-day spec→test lag.
- EELS PRs repeatedly **preceded the EIP text they implement**: #2972 (8038) opened two days before the EIP's numbers PR #11802 existed; #3064 (stipend) opened before EIP fix #11854. The Python spec was already the leading artifact; the process just doesn't acknowledge it.

Where problems were actually found, tallied across the fork's headline incidents:

| Incident | First found by |
| --- | --- |
| bal-devnet-3 chain split (8037 spillover) | devnet — but the defect class (spec ambiguity) was the same one EELS review kept surfacing |
| bal-devnet-6 "multiple bugs" | differential testing around devnet branches (rakita/benaadams/ethrex) — all EEST coverage gaps, testable in principle |
| 8037 inline-refund crediting | **goevmlab differential fuzzing** (tests-bal@v7.2.0) |
| 8038 profitable-refund (#11823) | **spec-text review**, 2 days pre-devnet; "No changes in EELS required!" |
| 7928/8038 SSTORE stipend (#11854) | **EELS implementation** (fix opened before the EIP PR) |
| 2780/EIP-161 precompile (#3048) | **geth cross-validation** (rjl493456442) |
| 7702 builder/validator BAL divergence | devnet-only — because EELS had **no block-building tests** (gap since closed, specs#2945) |
| gd-4 chain split | devnet-only — CL fork-choice spec defect |

Six of eight were found off-network or were off-network-findable; the two genuine devnet-only finds map to *named, closable* test-modality gaps (building-path tests; CL fork-choice scenario vectors). Meanwhile the testing team's upstream feedback was voluminous and early: fselmo's spec-gap PR two weeks after the first BAL fill (Sep 2025, #10364), the 8024 wrong-example catch (#11200), Carsons-Eels' wholesale 8037 revision (#11570), danceratopz's discovery that **7708 had no gas costs specified for its logs at all** (#11627 — still open in July while devnets run undefined costs), spencer-tb's line-by-line EELS-vs-EIP reviews. The problem was that this feedback stream had no *gate* to act on: EIPs entered devnets on ACDT convenience (parithosh: "an EIP is added to devnets mostly based on convenience of sequencing and testing — usually not based on importance"), devnet inclusion drifted toward de-facto SFI (fselmo: "the more time that passes… the more risk in our process to **passively push things through because of sunken cost**"), and spec authors kept iterating after inclusion because nothing stopped them.

The costs of pointing the loop at the wrong stage are on the record: spencer-tb, EELS #2744 (Apr 22): maintaining **8 devnet branches heading to 11**, releases taking hours of by-hand diffing, and "cross-EIP regressions surface… at devnet compose time with everything on fire at once". ~40 test-release tags for this fork vs ~10 for Fusaka.

## 6. The EIP-interaction problem

Every devnet snapshot is implicitly a *joint specification* of ~10–13 EIPs, but no artifact represents that composition except (a) the EELS fork branch and (b) the devnet itself. Since (a) is treated as downstream documentation rather than a gate, interactions surface at (b) — devnet compose time. The interaction pairs this fork actually paid for:

| Pair | What happened |
| --- | --- |
| 8037 × 7928 | gas-validation phases alignment (Mar), conflict resolution PR (Jun 18) |
| 8037 × 7702 | auth gas accounting reworked repeatedly; refund asymmetry caused the builder/validator BAL split |
| 8037 × 7976 | calldata-floor accounting alignment (Jun 1); floor applied to block-level accounting only in devnet-7 (#11908) |
| 8038 × 7928 | post-8038 cold-storage cost (3000) broke the EIP-2200 stipend sentry (2300) → #11854 |
| 8038 × 3529/2200 | reversal logic dropped in rework → net-profitable round trips (#11823) |
| 7708 × 8246 | burn-log semantics moved between EIPs 5 days after 7708's SFI |
| 2780 × 7708 | missing transfer-log costs for CREATE-with-endowment (#11586) |
| 2780 × 7523/161 | emptiness-definition dependency; zero-balance precompile bug (#3048) |
| 7928 × 7702 | delegation tracking added May, reverted Jul 2 |

Two structural observations:

1. **The interaction surface is not O(N²) in practice — it is a hub-and-spokes graph around shared resources.** Nearly every pair above routes through one of two hubs: *the gas/refund accounting* (8037/8038/2780/7778/7976/7981 all mutate one meter) or *the BAL contents* (7928 must observe whatever any other EIP does to state). Recognizing the hubs tells you the fix: specify the hub once, as one artifact, and make each EIP a patch against it — rather than specifying N documents and reconciling them pairwise. This is the "single versioned gas-schedule spec" recommendation from the earlier research, now with the devnet-level evidence behind it.
2. **Late additions multiply against everything already present.** 8246 (created May 6, CFI'd May 10) forced same-month edits to 7708 and interacts with 8037's SELFDESTRUCT charges; 8038's June numbers forced the 7928 stipend clause. A fork with k EIPs absorbing a new one doesn't get one new spec — it gets up to k new pairwise questions, each answered under devnet deadline pressure. The "scope closed" declaration of Jun 4 was followed by the largest EL delta of the series on Jun 25.

## 7. Fusaka baseline: what a purpose-labeled devnet ladder looks like

Fusaka ran **6 devnets in ~3.5 months** (May 26 – Sep 10, 2025) against Glamsterdam's 16 launched networks over 8+ months — and the difference is not that Fusaka found fewer bugs, it's that each Fusaka devnet had a declared purpose and exit criterion (ethereum/pm #1528–#1736):

- **devnet-2** was assembled at Berlinterop with an explicit rule separating speed from governance: "Faster process in order to test, but the changes included will now have to use ACD process to be officially included in Fusaka" (ACDT #40). Glamsterdam's interop week instead produced bal-devnet-5/6 whose spec changes *became* the fork spec.
- **devnet-3** was the **spec-freeze devnet** — ralexstokes (ACDE #216): "the rest of July to get any last minute spec things sorted and devnet-3 live and healthy, all of August can be devoted to hardening implementations, and then we move to the deployment pipeline in September." It was then kept alive long-term as background infra rather than replaced.
- **"There are no plans to have devnet 4"** (barnabasbusa, ACDC #160) — the default was *no next devnet*; one had to argue for a launch. (A short-lived devnet-4 slipped through anyway in August — the discipline was imperfect, but the burden of proof pointed the right way.)
- **devnet-5** was the **testnet gate**: "Dates won't be set until successful testing on fusaka-devnet-5" (ACDC #165), with pre-launch config verification via `eth_config` and a ~20,000-test Hive dashboard as launch-readiness signals, followed by a code freeze and release-candidate validation step.

Glamsterdam's only comparable declared milestone was "bal-devnet-7 = last EL-only devnet". No Glamsterdam devnet was designated the spec-freeze devnet; consequently every devnet was implicitly allowed to be one more iteration.

The Fusaka record also carries the capacity warning that bounds all of this: rolfyone (ACDC #164): 30d release→mainnet + 30d release→first testnet + ~30d between testnets ≈ "3 months of a potential 6 month cycle… **Something has to give. If we want this release timeframe, then I don't see how we can maintain a 6 month cadence as well**"; lightclient agreed the rollout can't compress because "too many people and companies depend on Ethereum". The rollout tail is fixed cost — the only place a 6-month fork cadence can recover time is the devnet/spec-iteration phase. Glamsterdam spent that budget several times over.

## 8. Recommendations

The earlier research's recommendations (gates that mean something, cluster-SFI, single gas-schedule artifact, scope-add = schedule-slip) all stand; the devnet-level evidence adds these sharper, devnet-specific ones:

1. **Devnets validate; they never decide.** A devnet launch requires a frozen spec snapshot: the EELS fork branch tagged, EEST green against it, and *no open substantive EIP PRs against any included EIP*. If a design question is open (dynamic vs fixed CPSB, frame vs opcode accounting, receipt field or not), it is answered in the breakout with an EELS prototype — not by launching two networks 44 hours apart. This single rule deletes bal-1, one of bal-5/6, and the gd-6/7 respin from the historical record.
2. **Make the EELS fork branch the composition gate ("in the fork" = "merged into EELS").** An EIP is a candidate for devnet inclusion only after it lands on the EELS fork branch *composed with everything already there*, with the composed diff reviewed by the authors of the EIPs it touches. The fork's own history shows EELS already leads the EIP text; formalizing that turns interaction discovery from a devnet-compose-time fire into a PR review. Corollary: the EIP-text change and the EELS change should be one reviewable unit (the Weld merge of EEST into execution-specs makes this natural).
3. **Close the named test-modality gaps so the devnet-only class keeps shrinking.** Each gap converted a locally catchable bug into a devnet discovery: block-building/rejection tests (specs#2945 — the 7702 BAL split), builder-API kurtosis harness (the "yolo it on devnet" gap), engine-API integration coverage (Nethermind `blobGasUsed`), fork-activation matrix tests (Nimbus activating Gloas opcodes at Fulu), and standing differential fuzzing (goevmlab found what EEST shapes missed) as a *launch gate* rather than an interop-week activity. Within the testing team's capacity, the prioritization question is modality work vs. re-fill responsiveness, and the compounding direction favors modality work: re-fill demand is set upstream by spec churn (recommendations 1–2 are what actually shrink it, not testing-team effort), while each modality investment permanently retires a devnet-only bug class — fewer devnets, fewer re-fill rounds, more capacity freed. A concrete priority order: (a) automate the re-fill/release pipeline itself, so a spec change costs minutes instead of the hours of by-hand diffing described in specs#2744; (b) block-building/rejection tests and the builder-API harness (specs#2945 already proved the payoff by retiring the fork's clearest devnet-only bug class); (c) standing differential fuzzing as a launch gate; (d) fork-activation and engine-API coverage. Glamsterdam's ~40 test-release tags (vs ~10 for Fusaka) should be read as a process-health metric charged to upstream spec churn — not as a testing-team backlog to be absorbed by working harder.
4. **Give shared resources a single owner-spec, and require an Interactions section.** The gas/refund meter and the BAL are hub artifacts: one versioned normative document each (or an EELS module treated as normative), with the individual EIPs as motivated patches. Any EIP touching a hub must carry an "Interactions" section enumerating behavior against every other CFI'd EIP on the same hub, kept current by the cluster breakout (which starts at PFI, not four months after CFI).
5. **Adopt a devnet budget with named purposes — Fusaka already demonstrated the ladder.** Declare the plan up front — e.g. headliner interop → feature batch(es) → consolidation/benchmark → merged-fork verification (≈5 EL devnets), with one devnet designated the spec-freeze devnet and one the testnet gate (§7) — and require an ACDT-stated purpose ("what can only this network tell us?") for any addition; the default answer to "devnet N+1?" should be Fusaka's "there are no plans" until argued otherwise. Devnet-as-deadline is legitimate (bal-7 forcing eth/70/71) but should be declared as such, not smuggled. Publish a one-paragraph post-mortem for every abandoned network (gd-0/1 and gd-2/3 have none) so relaunch costs are visible.
6. **Keep the sunk-cost firewall.** The ACDT #74 consensus (devnet inclusion ≠ SFI; explicit SFI decisions only) and the two-step used for 8282 (ACDT prelim → ACDE ratify, with a stated fork-delay risk) are the right pattern — the latter was the best-run scope decision of the fork and should be the template, not the exception.

**Bottom line:** the devnet count was a symptom. The EL feedback machinery (EELS, EEST, differential fuzzing, hive, kurtosis) was fast and repeatedly out-ran the devnets — finding bugs before genesis, sometimes before the EIP text existed. What was missing was the *authority* of that machinery in the process: nothing required a design to survive EELS composition and green tests before a network was cut for it, so networks were cut to force the question instead. Roughly 4 of 9 EL spec iterations, and about two months, went to answering questions off-network machinery had already raised — or could have answered — earlier. Give the testing team a gate, not just a queue.

## 9. Sources

Raw source material is archived locally in [fork-process-research-data/](fork-process-research-data/README.md), including the four underlying research reports: [devnet notes & configs](fork-process-research-data/agent-report-devnet-notes-and-configs.md), [EELS/EEST feedback loop](fork-process-research-data/agent-report-eels-eest-feedback-loop.md), [ACDT devnet triggers](fork-process-research-data/agent-report-acdt-devnet-triggers.md), [Fusaka cadence baseline](fork-process-research-data/agent-report-fusaka-cadence-baseline.md).

- ethpandaops: `notes.ethereum.org/@ethpandaops/{bal,glamsterdam}-devnet-0…7` (raw copies in [fork-process-research-data/devnet-notes/](fork-process-research-data/devnet-notes/)), `github.com/ethpandaops/{bal,glamsterdam,epbs}-devnets` (genesis configs, git history).
- `ethereum/pm`: ACDT #62–87 (issues #1820–#2151), ACDE #225–240, repricing breakouts; pre-scope ACDT #58–61 (issue dumps in [fork-process-research-data/pm/](fork-process-research-data/pm/)); Fusaka-era baseline issues #1528–#1736 and PR #1715 (Protocol Upgrade Process doc; dumps in [fork-process-research-data/acdt/](fork-process-research-data/acdt/)).
- EELS/EEST: release tags `bal@v1.0.0`…`tests-glamsterdam-devnet@v7.2.0`; issues/PRs #1846, #1912, #2040, #2363, #2578, #2687, #2689–#2733, #2744, #2748, #2804, #2827, #2901, #2915, #2945, #2972, #2990, #3001, #3017, #3020, #3048, #3064, #3126.
- `ethereum/EIPs` PRs: #11117, #11181, #11292, #11328, #11399, #11421, #11475, #11476, #11522, #11532, #11540, #11548, #11570, #11573, #11586, #11596, #11611, #11616, #11626, #11627, #11634, #11645, #11696, #11699, #11715–#11718, #11750, #11759–#11760, #11783, #11802, #11807, #11818, #11823, #11844, #11854, #11858, #11891, #11899, #11902, #11906, #11908; local git history of all in-scope EIPs.
- Client trackers: go-ethereum#33735/#33972, besu#9994, nimbus-eth1#4036, erigon#22038/#22152/#22254, lighthouse#8726, lodestar#9415/#9475/#9477/#9562/#9596; consensus-specs#4858/#4979/#5307/#5348/#5355/#5399; execution-apis#691/#727/#731/#770/#786/#794/#796; potuz.net epbs-devnet-0 post-mortem.
