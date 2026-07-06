# Glamsterdam Fork Process Efficiency Research

Research conducted 2026-07-06 with Claude Code. All data gathered read-only from the local `ethereum/EIPs` checkout, the `ethereum/pm` repository (via `gh`), forkcast.org data sources, ethpandaops devnet notes, and ethereum-magicians.org. **Nothing was published anywhere.**

---

## 1. Original prompt

> Hi Claude, we want to research how we can improve the efficiency of the fork process. The current fork in scope is Glamsterdam, see EIP-7773. Relevant repository is also ethereum/pm (check the Issues, in particular ACDT, ACDE, ACDC agendas) and for outcomes of these agendas see forkcast.org.
> In the previous fork, Fusaka, launched December 3 2025, we were very ambitious to launch more forks (target 2 forks each year), but this is not happening. Throughout the process many changes were made. For the execution layer (EL) side the repricing EIPs (check Glamsterdam meta EIP for the list of those EIPs in question) and other EL-side EIPs are dependent on each other (lots of interactions) which means that changes to one EIP could also indirectly change the other EIPs. This puts a lot of burden on the execution-specs team which writes tests for these EIPs, and then clients have to implement these changes. This means that an EIP spec change means testing changes/additions, and new client implementations. We are now running devnet 6 or 7 for glamsterdam which is a very high number.
> We want to improve this process by making it more efficient. Focus on the EL side only. Study changes in the EIPs (look at their dates and cross-check with the ACD agendas) and advise how to make the process more efficient. Ask questions where necessary. Under no circumstances publish anything to GitHub.

Follow-up instructions during the session:

> Also include open and closed PRs to the EIPs repository. Updates to EIPs are marked as "Update EIP-x" and each PR only targets a single EIP. Obviously also include merged PRs.

> If not included, check the network upgrade process https://eips.ethereum.org/EIPS/eip-7723 as well, this is the canonical EIP spec for the updates (also check diffs or discussions against this one) and advise on this process.

## 2. Methodology

Five parallel research streams, cross-dated against each other:

1. **Commit-level EIP churn** — `git log`/`git show` on the local EIPs repo for all in-scope EL EIPs; every commit since 2025-01-01 classified as substantive / editorial / status.
2. **PR mining** — all `Update EIP-N` PRs (merged, open, closed-unmerged) to `ethereum/EIPs` since 2024-06-01 for the in-scope EIPs (347 PRs total).
3. **ACD agendas** — `ethereum/pm` issues for ACDE / ACDT / repricing-breakout calls, March 2025 → July 2026.
4. **Forkcast + devnets** — forkcast.org data files, ethpandaops devnet notes, devnet configs.
5. **EIP-7723 deep dive** — full change history, PRs, and the Ethereum Magicians debate on the inclusion-stage process itself.

## 3. Scope: Glamsterdam EL ("Amsterdam") EIPs (per EIP-7773, 2026-07-06)

- **SFI:** 7708 (transfers emit logs), 7732 (ePBS, CL headliner), 7778 (block gas accounting w/o refunds), 7843 (SLOTNUM), **7928 (Block-Level Access Lists, EL headliner)**, 7954 (max contract size), 7976 (calldata floor increase), 7981 (access list cost), 8024 (SWAPN/DUPN/EXCHANGE), 8037 (state creation gas increase).
- **CFI (repricing-relevant):** 2780 (intrinsic tx gas), 7904 (general/compute repricing — retired to Informational 2026-06-12), 8038 (state-access gas increase), 8246 (remove SELFDESTRUCT burn).
- The repricing cluster = 7778, 7976, 7981, 8037 (SFI) + 2780, 7904, 8038, 8246 (CFI). 2780's June 2026 rework declares `requires: 7904, 7928, 8037, 8038` — the interdependence is formalized in frontmatter.

## 4. Milestone timeline (from EIP-7773 git history)

| Date | Event |
|---|---|
| 2025-06-09 | PFI 7843 (with 7793) |
| 2025-07-04 | All previously-CFI'd EIPs reset to PFI |
| 2025-07-24 | CFI 7732 (with 7782, 7805) |
| 2025-07-31 | CFI 7928 |
| **2025-08-14** | **SFI 7732 + 7928 (headliners, ACDE #218)** |
| 2025-08-26 → 2025-10-28 | PFIs: 7981, 7778, 7976, 2780, 7904, 8037, 8038, 7708, 8024 |
| 2025-12-18 | CFI 7708, 7778, 7843, 8024 (ACDE #225) |
| 2026-01-02 | CFI 2780, 7904, 7976, 7981, 8038 (ACDE #226) |
| 2026-01-20 | CFI 7954 + CFI 8037 (ACDE #228 / ACDT #66) |
| 2026-05-10 | CFI 8246 (4 days after its creation) |
| **2026-05-11** | **SFI batch: 7708, 7778, 7843, 7954, 7976, 7981, 8024, 8037** (PR #11399; 2780, 7904, 8038 left at CFI) |

## 5. PR churn (ethereum/EIPs, "Update EIP-N" PRs since 2024-06-01)

347 matching PRs across the 15 EIPs. m=merged / o=open / c=closed-unmerged, by PR creation date:

| EIP | 2024H2 | 2025H1 | 2025H2 | 2026H1 | 2026H2* | Total |
|---|---|---|---|---|---|---|
| 7708 | 1m | 1m | – | 9m/1o/4c | – | 16 |
| 7732 | 2m | – | 1m/3o/1c | 2m/2o/1c | – | 12 |
| 7778 | – | 1m | 2m | 5m/2c | 1o | 11 |
| 7843 | 1m | 3m/1c | – | 1m | 1o | 7 |
| **7928** | – | 13m/1o | 41m/4c | 19m/1c | 1m | **80** |
| 7954 | – | – | – | 2m | – | 2 |
| 7976 | – | – | 3m | 3m/1o | – | 7 |
| 7981 | – | – | 2m | 6m | – | 8 |
| 8024 | – | – | 4m | 6m/2c | – | 12 |
| **8037** | – | – | 2m/1o | 23m/2o/4c | 1o | **33** |
| 8038 | – | – | 3m/1o | 10m/1o/2c | – | 17 |
| 8246 | – | – | – | 5m | – | 5 |
| **2780** | – | 1c | 24m | 12m/1o/1c | 1o | **40** |
| 7904 | – | – | 3m/2o | 3m | – | 8 |
| 7773 (meta) | – | 3m/1c | 54m/1o/9c | 13m/5o/1c | 2o | **89** |

*2026H2 = July 1–6 only.

Standouts: 7928 = 80 PRs (74 merged, ~60 by nerolation); 2780 = 40 (incl. a solo 20-PR modernization burst by benaadams, Sep–Oct 2025); 8037 = 33 (23 merged in 2026H1 alone).

### Currently-open substantive PRs (as of 2026-07-06)

- **2780 #11844** (rakita, 2026-07-01): move state-dependent charges to runtime — intrinsic = warm-access floor, cold surcharge + new-account state gas at runtime.
- **7778 #11857** (Helkomine, 2026-07-03): refund routing (resubmit of closed #11824).
- **8037 #11858** (chfast, 2026-07-03): charge account creation conditionally at access (ties to 7610/7928).
- **8037 #11570** (Carsons-Eels/STEEL, 2026-04-24): broad revision from implementation/testing findings.
- **8037 #11778** (lu-pinto, 2026-06-08): fix unfair allocation of 7702 overcharges.
- **8038 #11826** (Helkomine, 2026-06-23): WARM_ACCESS/LOW_WARM_ACCESS split, warm charge into base opcode cost.
- **7708 #11627** (danceratopz, 2026-05-08): gas costs for system-emitted transfer/burn logs.
- Plus 7928 #9873 (BAL-size scaling note, open since Jun 2025), 7732 constants PRs, 7773 meta housekeeping.

Four of these (11844, 11857, 11858, 11826) contest the **same question — where gas is charged (intrinsic vs runtime)** — across four EIPs, all filed within two weeks, three by client implementers. Active contention two months *after* SFI.

### Notable rejected/abandoned PRs

7928 #11181 (first-accessed indices, closed 2026-02-16); 8024 #11094 (push-postfix encoding, closed 2026-02-12); 2780 #11735 (superseded by #11844); several 8037 state-gas frame-failure variants superseded by #11476/#11532.

## 6. Commit-level substantive change analysis

SFI dates: 7732 & 7928 = 2025-08-14; the other eight = 2026-05-11.

| EIP | Created | Subst. 2025 | Subst. 2026 | Last subst. change | Post-SFI substantive changes |
|---|---|---|---|---|---|
| 7708 | 2024-06-11 (stagnant → revived 2026-01) | 0 | 8 | 2026-05-16 | **1** — burn-log section deleted 5 days after SFI (moved to 8246) |
| 7732 | 2024-07-03 | 1 | 0 | 2025-08-12 | 0 (spec lives in consensus-specs) |
| 7778 | 2024-10-04 | 0 | 3 | 2026-01-28 | 0 |
| 7843 | 2024-12-23 | 3 | 0 | 2025-05-20 | 0 |
| **7928** | 2025-05-06 | 22 | 16 | **2026-07-04** | **23 — churning for 11 months post-SFI** |
| 7954 | 2025-06-23 | 0 | 1 | 2026-05-21 | **1** — max code size doubled 32→64KiB 10 days after SFI |
| 7976 | 2025-06-24 | 0 | 1 | 2026-02-15 | 0 |
| 7981 | 2025-07-13 | 1 | 3 | 2026-05-05 | 0 (last change 6 days pre-SFI) |
| 8024 | 2025-09-17 | 2 | 2 | 2026-02-25 | 0 |
| **8037** | 2025-10-07 | 2 | 19 | 2026-06-18 | **6** — incl. bal-devnet-6 bugfixes 2 days after SFI |
| 2780 | 2020 (revived 2025-09-04) | 3 | 6 | 2026-06-25 | n/a (CFI) — full rework 2026-06-18 |
| 7904 | 2025-05-15 | 2 | 2 | 2026-06-12 | n/a — retired to Informational |
| 8038 | 2025-10-08 | 1 | 4 | 2026-06-22 | n/a (CFI) — numbers still TBD until 2026-06-18 |
| 8246 | 2026-05-06 | – | 0 | creation | n/a — CFI'd 4 days after creation |

### Key design reversals and post-SFI redesigns

- **7928** (SFI 2025-08-14): SSZ→RLP encoding switch **2 days after SFI**; constants re-derived Oct 2025; types changed Oct–Nov 2025; **BAL removed from EL block body → engine API** Dec 2025; two-phase gas validation Jan 2026; size caps introduced and twice reworked Feb 2026; BlockAccessIndex uint16→uint64→uint32 (Apr 2026); 7702 tracking added May 2026 then **reverted 2026-07-02**; SSTORE access-cost check added 2026-07-04 (required for devnet-7). Plus ~25 devnet-driven "clarify edge case" commits.
- **8037**: fixed multipliers → dynamic `cost_per_state_byte` (2026-01-06, post-CFI); + quantization & reservoir model (2026-02-10); reservoir mechanics redefined Mar 2026; implementer-feedback burst Apr 2026; bal-devnet-6 bugfixes + params 2026-05-13 (2 days post-SFI); calldata-floor alignment, 7702 rework, 7928 conflict resolution Jun 2026.
- **7778**: receipt `gas_spent` field added 2026-01-19, **reverted 2026-01-28** (9 days).
- **7904**: simplified Sep 2025 → new methodology Feb 2026 → **converted to Informational 2026-06-12** ("no compute repricing needed" at 100 Mgas/s with 7928-parallelized clients). The CFI'd "General Repricing" evaporated; content absorbed by 8037/8038/2780.
- **2780**: 21000→8000→6000 (Sep 2025) → 4500 + cost splits (Oct 2025) → **full resource-based rework, TX_BASE_COST=12000** (2026-06-18) → EIP-7523 emptiness, OOG handling (2026-06-25) → runtime-charging rework still open (#11844).
- **8038**: SSTORE clear refund reversed 2026-06-22 (prevented net-profitable round trips); first real numbers only 2026-06-18.
- **Cross-EIP coordination bursts** (same-day changes): 2026-01-19..28 (7778/7981/7928/7708/8037, around ACDE #228); 2026-02-09..20 (repricing methodology wave); 2026-06-18 (2780 rework + 8037 conflict-fix + 8038 numbers, all misilva73, same day); 2026-06-22..25.
- **Author concentration**: nerolation drove 7928/7778/7976/7981; misilva73 (EF) drove 8037/8038/7904/2780; benaadams the 2780 modernization; chfast owns 8246; frangio owns 8024. Client implementers (spencer-tb, qu0b, rjl493456442, jwasinger, rakita, danceratopz, Helkomine) only appear as PR authors from 2026Q1 — the implementation-feedback phase, post-CFI.
- **Stable counterexamples**: 7843 frozen since 2025-05-20; 7976 & 8024 frozen since Feb 2026; **7732 produced ~zero EIP churn because its normative spec lives in consensus-specs** (versioned executable spec) — a direct within-fork contrast with 7928.

## 7. ACD call timeline (ethereum/pm)

Caveat: pm issues in this era contain agendas + pre-call comments; firm decisions live in Magicians threads and forkcast.

### Phase 1 — process & headliner selection (Mar–Aug 2025)

- 2025-05-08 ACDE #211: Tim Beiko introduces the **headliner-first fork process** (Magicians t/24088).
- 2025-06-05 ACDE #213: first Amsterdam EIP proposals (7793+7843); BAL headliner presentation.
- 2025-07-17 ACDE #216: Geth formally backs **EIP-7928 BALs as EL headliner**; PFI deadline set 2025-08-21; "Fusaka scope frozen, Glamsterdam only looking at headliners."
- 2025-08-14 ACDE #218: **headliners finalized** (7732 + 7928); competing candidates (EOF/7692, 7782, 7886, 7937, 7942) declined; open 7928 design issues (RLP vs SSZ!) discussed *on the same call that SFI'd it*.
- 2025-08-28 ACDE #219: 18 PFI'd EIPs; CFI review deferred; teams "sprinting towards Fusaka releases" — explicit ACDE-time competition.

### Phase 2 — scoping under the Fusaka shadow (Sep 2025 – Jan 2026)

- 2025-09-11 ACDE #220: repricing EIPs floated (7976, 7981, 2780).
- 2025-10-09 ACDE #222: repricings tracked under meta EIP-8007; breakout call proposed.
- 2025-10-23 ACDE #223: scoping plan — **"finish Glamsterdam scoping by end of November 2025"**; Fusaka mainnet Dec 3 soft-confirmed same call (peak overlap).
- 2025-11-06 ACDE #224: client teams post written EL EIP rankings.
- 2025-12-04 ACDE #225 (day after Fusaka mainnet): hypothetical timeline — **Glamsterdam mainnet 2026-06-24, testnet client releases ~2026-04-18**. CFI 7708/7778/7843/8024; ~13 EIPs declined.
- 2025-12-18 ACDE #226: CFI 2780/7904/7976/7981/8038; Marius: "tie down the scope for repricings."
- 2026-01-05 ACDE #227: CFI count at 14 — "most EIPs ever if all SFI'd."
- 2026-01-15/19 ACDE #228 / ACDT #66: CFI 7954, CFI 8037.

### Phase 3 — bal-devnets + epbs-devnets (Dec 2025 – Apr 2026)

- 2025-12-01 ACDT #62: **bal-devnet-0 already live** (2 days before Fusaka mainnet). ACDT covers both forks until ~Mar 30 2026.
- 2025-12-15 ACDT #64: bal-devnet-1 = "Spec 2.0" with 7928 type changes.
- 2026-01-19 ACDT #66: bal-devnet-2 scope; **7843 dropped (underspecified)**.
- 2026-02-02 ACDT #68: bal-devnet-2 blockers — Besu/Nethermind shipping the later-reverted `gasSpent` receipt field; Nimbus chain split.
- 2026-02-04: **Glamsterdam Repricings breakout series starts** (8 biweekly calls, Feb 4 – May 27).
- 2026-02-23 ACDT #71: **8037 breaks test determinism** (cost depends on block gas limit, >600 test files); postponed to bal-devnet-4.
- 2026-03-12 ACDE #232: spencer-tb: "SFI all devnet EIPs?" (PR #11399) — pushback: "devnet inclusion ≠ SFI". **Erigon calls to remove or significantly redesign 8037** ("gas becoming 4-dimensional… impossible to reason about").
- 2026-03-26 ACDE #233: SFI question raised again — **ran out of time, no decision**.
- 2026-04-23 ACDE #235: STEEL temperature check — **8037 "may not be shippable without major changes"**, ~30% of tests affected.

### Phase 4 — SFI, unified devnets, devnet-5/6/7 (May–Jul 2026)

- 2026-05-07 ACDE #236: **SFI batch decided** (ratified in meta via PR #11399, merged 2026-05-11) — the same call reports **bal-devnet-6 has multiple bugs**; bal-devnet-7 planned as "last EL-only devnet"; 8246 proposed.
- 2026-05-13 Repricings #7: final 8037 params; 2780 rework; 8038 alignment; devnet inclusion for 8038+2780.
- 2026-05-18 ACDT #80: glamsterdam-devnet-4 = first combined devnet; **8246 CFI'd** ("required by 7708"); devnet-5 to consider 8038+2780.
- 2026-06-01 ACDT #81: devnet-4 chain-split (~May 25); post-SFI 7928/8037 changes merged; **repricing breakouts discontinued (specs stabilized)**; final basket 8246/2780/8038; 7904 → Informational.
- 2026-06-04 ACDE #238: all new pitches redirected to Hegotá — Glamsterdam scope "closed" (yet devnet-6/7 additions continue).
- 2026-06-15 ACDT #83: 2780 rework canonical; 8038 first numbers (STORAGE_WRITE +257%); **7904 moved to Informational**.
- 2026-06-22 ACDT #84: devnet-6 tests released; **8038 bug found (#11823)**; 7688 proposed for devnet-7.
- 2026-06-29 ACDT #85: devnet-7 planning; 8282 constants reduced; 2780 runtime-charging rework (rakita).
- 2026-07-02 ACDE #240: devnet-7 spec assembly — 8038 SSTORE fix + matching 7928 clause; **8282 finalized "by end of call"** with a new deposit contract address; meta EIP found to be missing 8282.
- 2026-07-06 ACDT #86: **7928 stipend-check change (#11854) required as of devnet-7**; devnet-7 genesis blocked on 8282 sys-asm issue.

## 8. Devnet history

Three series feed Glamsterdam — **18 devnets total** (vs the "devnet 6 or 7" surface count):

### bal-devnets (EL headliner track) & epbs-devnets (CL)

| Devnet | Launch | EL scope / delta |
|---|---|---|
| bal-devnet-0 | 2025-11-04 | 7928 only (before Fusaka mainnet!) |
| bal-devnet-1 | 2025-12-18 | 7928 "Spec 2.0" |
| bal-devnet-2 | 2026-02-06 | + 8024, 7843*, 7708, 7778 (*7843 dropped in scoping) |
| epbs-devnet-0 | 2026-03-04 | 7732 (CL) |
| bal-devnet-3 | 2026-03-04 | + 8037, 7954; optional 7975/8159 |
| epbs-devnet-1 | 2026-03-31 | 7732 |
| bal-devnet-4 | 2026-04-22 | + 7976, 7981; 8037 updated |
| bal-devnet-5 | 2026-04-29 | rerun |
| bal-devnet-6 | 2026-05-01 | same set — **multiple bugs** (execution-specs #2804) |
| bal-devnet-7 | 2026-05-18 | 8037 updated; eth/70+eth/71 mandatory; "last EL-only devnet" |

### Combined glamsterdam-devnets

| Devnet | Genesis | EL delta vs prior |
|---|---|---|
| glamsterdam-devnet-0 | 2026-04-24 | bal-devnet-4 EL set + 7732; EEST `bal@v5.6.1` |
| devnet-1 | 2026-04-29 | relaunch |
| devnet-2 | 2026-04-30 | + 8061 (CL) |
| devnet-3 | 2026-05-06 | same |
| devnet-4 | 2026-05-22 | bal-devnet-7 set + `targetGasLimit` engine field; **8037 final shape (cost_per_state_byte 1174→1530, STATE_BYTES_PER_STORAGE_SET 32→64, ref gas limit 96M→150M)** — chain-split ~May 25, abandoned |
| devnet-5 | 2026-06-04 | + 8045 (CL), 8189 optional; salvaged after Prysm/Grandine fixes; shut down 2026-06-25 |
| devnet-6 | 2026-06-25 | **Big post-SFI delta: + new 2780, 8038, 7997, 8246, 8282, 8070 opt.; 7954→64KiB; 8037 source-based refunds; 7610**; EELS `glamsterdam-devnet@v6.1.1` |
| devnet-7 | mid-July 2026 (planned) | + 7688 (CL); new 8282 deposit address; 7928 #11838/#11854; 2780/8038 fixes |

### Fusaka comparison

- Fusaka: **6 devnets** (2025-05-26 → 2025-09-10); Holešky 2025-10-01, Sepolia 2025-10-14, Hoodi 2025-10-28; **mainnet 2025-12-03**. First devnet → mainnet: **191 days**.
- Glamsterdam: bal-devnet-0 2025-11-04 (8 months ago); first combined devnet 2026-04-24; **no activation date projected** (forkcast: "2026"); the Dec-2025 target of mainnet 2026-06-24 has passed with devnet-7 still forming.

## 9. EIP-7723 (the inclusion-stage process spec)

### Change history

| Date | Change |
|---|---|
| 2024-07-04 | Initial: PFI/CFI/SFI/Included only. SFI = "client teams decide to work on an EIP; implementation and testing underway". No devnet linkage, no maturity criteria. |
| 2024-09-04 | MUST → SHOULD softening (SamWilsn's enforceability objections). |
| 2024-12-05 | DFI introduced; statuses per-upgrade only. |
| 2024-12-18 | CFI/SFI tied to devnets. SFI = "agree to implement in the **next** devnet". |
| 2025-03-15 | EELS/EEST requirements: CFI SHOULD have spec+tests; SFI MUST (tests "strictly mandatory"); post-SFI changes MUST update spec/tests. |
| 2025-02-12 | → Last Call, deadline 2025-04-01 — **still Last Call today, 15 months past deadline**, amended substantively while in Last Call. |
| 2025-10-03 | PFI proposer = point of contact (a watered-down "EIP Champion" — champion language removed in review). |
| 2026-05-08 | **Third SFI redefinition** (PR #11475, per ACDT #74): maturity criteria — in a stable devnet ≥1 week; spec close to final ("no placeholder values or pending design decisions"); minimal/tested interactions with other CFI EIPs; adequate test coverage. PR body admits the old definition was "not playing out in reality". |

### Open/closed process PRs

- **#9988** (spencer-tb, Jul 2025, **open ~1 year**): require benchmarking for new opcodes/precompiles at SFI.
- #11076 (open): remove the contradictory Last Call requirement in Rationale.
- **#11239** (nixorokish, Feb 2026, **closed unmerged**): Move to Living — needed because Fusaka's meta EIP-7607 `requires` 7723 and cannot go Final while 7723 is stuck in Last Call. Unresolved.

### Magicians debate highlights (t/20281 + parent t/20157)

- dapplion (Jun 2024): CFI/SFI split is over-engineering, adds governance cost of maintaining two lists.
- SamWilsn (Jul–Sep 2024): "who is responsible for taking actions?" — editors can't enforce SHOULDs on client teams.
- abcoathup (May 2024, parent thread): "It is the end of May, and the scope for Pectra is still not finalized, when formal discussions started in January" — proposed **scoping deadlines with no new EIPs post-scope**. Tim Beiko resisted: "Ethereum's roadmap is extremely volatile… we shouldn't create a false sense of certainty." **The Glamsterdam failure mode was predicted in 2024.**
- The headliner process itself is not codified in any EIP — it exists in Magicians posts and forkcast.

## 10. Diagnosis

Root cause: **design iteration happened after the inclusion gates instead of before them.** Every post-gate spec change fans out into EELS/EEST updates plus 7 client re-implementations; that multiplier is what burned the testing teams and produced 18 devnets.

1. **CFI was granted on unfinished designs.** Repricing EIPs CFI'd Dec 2025–Jan 2026; 8037's fundamental redesign (dynamic CPSB, then reservoir model) came *after* CFI; Erigon called to remove it in March; STEEL called it "may not be shippable" in April; 7904 was retired entirely in June. Clients implemented moving targets.
2. **SFI ratified momentum instead of gating maturity.** The headliner 7928 was SFI'd at 3 months old with encoding still undecided → 23 substantive post-SFI changes over 11 months (SSZ→RLP 2 days post-SFI; BAL moved out of the block 4 months post-SFI; changes still required for devnet-7 as of 2026-07-06). The May 2026 batch SFI violated the week-old maturity criteria on day one; 4 of 10 SFI'd EIPs changed substantively post-SFI (7928, 8037, 7708 — burn-log deleted 5 days post-SFI, 7954 — headline parameter doubled 10 days post-SFI).
3. **Interacting EIPs maintained as independent documents = O(N²) alignment work.** Gas semantics spread across 2780/7778/7976/7981/8037/8038 (+7904, +7928). Evidence: "align 8038 with 8037", "resolve 7928 conflict", same-day cross-EIP bursts, and four open PRs contesting the same gas-placement question across four EIPs. Each alignment = new test release + 7 client cycles.
4. **Implementer feedback arrived a year late.** PR authorship flips from researchers to client implementers only in 2026Q1, post-CFI — when the fatal problems (test determinism, 4-dimensional gas, runtime-vs-intrinsic) were all discoverable in 2025.
5. **The Fusaka overlap ate the scoping window.** "Scoping done by end of Nov 2025" collided with the Fusaka release sprint; actual CFI Dec–Jan, SFI May. The June 2026 mainnet target died silently.
6. **The process spec is soft.** SFI redefined 3×; 7723 stuck in Last Call 15 months past deadline while being amended; benchmarking-at-SFI PR open a year; scope changes routed around gates (8246 created→CFI'd→absorbed part of an SFI'd EIP within 10 days; devnet-6 added 6+ EIPs after "scope closed").

**What worked (keep):** the headliner-first process; parallel bal-/epbs-devnet tracks; the repricing breakout series (8 calls converged the basket and killed 7904 — its only flaw was starting 4 months after CFI); client EIP-ranking exercise (Nov 2025); and the 7732 model — a versioned executable spec produced ~zero EIP churn on the *more* invasive headliner, versus 38 substantive changes on document-spec'd 7928.

## 11. Recommendations

1. **One canonical spec per interacting cluster — make the gas schedule a single versioned artifact.** Publish one normative "Amsterdam gas schedule" (table + accounting rules, versioned like consensus-specs releases); demote individual repricing EIPs to motivation/rationale. Concretely: after CFI, make the EELS fork branch the single source of truth — spec changes land as EELS PRs first, EIP text follows. Collapses O(N²) alignment PRs into one diff per change and gives devnets an unambiguous pin. Proof point: 7732 (consensus-specs-based, ~0 churn) vs 7928 (document-based, 38 substantive changes) — same fork, same SFI date.
2. **Add an explicit spec-freeze stage between CFI and SFI; enforce SFI as written.** Mechanical gate: an EIP (or cluster) with open substantive `Update EIP-N` PRs cannot be SFI'd; a post-SFI substantive change triggers automatic SFI-status review at the next ACDE. Interacting EIPs are SFI'd **as a cluster or not at all**. Post-freeze, only bugfix-class changes via a named approval path. **Headliner-specific form:** headliner SFI is unavoidably early (a commitment signal), so it cannot double as "spec frozen" — add a separate headliner freeze milestone (e.g. "design frozen before the first combined devnet"). Glamsterdam never had one.
3. **Front-load implementer review: benchmarks + client design review before CFI.** Merge #9988 (or its spirit). Repricing numbers must come from the shared benchmark suite at CFI time so post-CFI changes are recalibrations, not redesigns. Require ≥2 client-team written design reviews as a CFI condition (the Nov 2025 ranking exercise, but with spec-quality obligations). Erigon's and rakita/chfast's objections were all knowable in 2025.
4. **Start breakout working groups at PFI, with a convergence deadline.** Any cluster of ≥3 interacting EIPs gets a chartered breakout with "converged or descoped by date X" as a CFI condition. The repricing breakout stabilized the basket in 8 calls — it just ran Feb–May 2026 instead of Sep–Dec 2025.
5. **Make devnets releases, not design laboratories.** Each devnet pins a tagged EELS/EEST release; delta between devnets = bugfixes + at most one pre-scheduled feature batch. Devnet-6 adding 6+ EIPs after SFI is the anti-pattern. Adopt a **"scope-add = schedule-slip" rule**: any post-SFI addition requires ACDE to explicitly restate the fork timeline.
6. **Shrink the unit of shipping.** Glamsterdam carries two maximal headliners (ePBS + BALs) *plus* a full gas-accounting overhaul, straight after Fusaka. Two forks/year is achievable only if fork N's scope is frozen before fork N−1 ships; anything not spec-frozen by then defaults to fork N+1 (the repricing basket was a natural Hegotá candidate). Complement the headliner process with: non-headliner CFI closes when the previous fork's mainnet date is set.
7. **Fix EIP-7723 itself and codify folk process.** Resolve the Last Call/Living contradiction (Fusaka's meta EIP is permanently blocked from Final otherwise). Codify: the champion role, the freeze stage, benchmarking requirements, cluster-SFI, and the headliner process. Surface **post-SFI substantive-PR count per EIP** (trivially measurable — this dataset found 347 update PRs) as a fork-health metric, e.g. on forkcast; it should trend to zero after freeze.

**Summary:** Glamsterdam's devnet count isn't a testing problem — it's the spec-maturity problem made visible. Every recommendation is a variant of "move the iteration before the gate, and make the gate mean something"; the testing and client-implementation burden then falls out automatically, because the (spec changes) × (7 clients) multiplier only shrinks at the source.

## 12. Sources

- Local: `EIPS/eip-7773.md`, `EIPS/eip-7723.md`, and git history of all in-scope `EIPS/eip-*.md` files in this repo.
- `ethereum/EIPs` PRs via `gh` (read-only), 347 `Update EIP-N` PRs since 2024-06-01.
- `ethereum/pm` issues: ACDE #208–240, ACDT #62–86, Glamsterdam Repricings breakouts #1–8 (pm#1910 ff.).
- Forkcast data: `github.com/ethereum/forkcast` — `src/data/upgrades.ts`, `devnets/glamsterdam*.json`, `generated/devnet-launches.json`, `eips/*.json`, `prioritization/glamsterdam.json`.
- ethpandaops: `notes.ethereum.org/@ethpandaops/glamsterdam-devnet-0…7`, `github.com/ethpandaops/glamsterdam-devnets`, `github.com/ethpandaops/bal-devnets`.
- execution-specs: issue #2915 (devnet-6 test release), #2804 (bal-devnet-6 bugs).
- Ethereum Magicians: t/20281 (EIP-7723), t/20157 (process improvements parent thread), t/24088 (headliner process), t/21195 (Glamsterdam meta).
- Secondary: EtherWorld headliner articles, Christine Kim ACDE #218 minutes, ethdaily.io/glamsterdam.
