All data gathered. Here is the report.

# Glamsterdam (EL) — Testing Team Role: EELS/EEST vs Devnet Iterations

Repos: EEST = ethereum/execution-spec-tests; EELS = ethereum/execution-specs. Note: **mid-May 2026 releases moved from EEST to EELS** (tags `tests-*`) after the "Weld" repo merge — bal@v7.0.0 notes: "this release is mirrored on EELS, we plan to solely release from EELS in the future."

## 1. Release timeline (all dated, from `gh release list/view`)

**~40 pre-release tags for this one fork** (bal@v1.0.0 → tests-glamsterdam-devnet@v7.2.0, Sep 2025 – Jul 2026). Fusaka needed ~10 devnet tags over a similar span.

| Release | Date | Content / spec version |
|---|---|---|
| bal@v1.0.0 | 2025-09-04 | First EIP-7928 tests (EEST PR #2067, fselmo). Notes: "**This release revealed updates needed on both the specifications and the testing side**" — spec feedback from the very first fill. |
| bal@v1.1–1.8 | Sep–Nov 2025 | Iterative 7928 coverage |
| bal@v2.0.0 | 2025-12-12 | Breaking: storage keys/slots re-encoded uint256 (spec change, EELS #1912) |
| bal@v3.0.0/.1 | 2026-01-13 | Breaking: BAL removed from block body (spec change); all static tests filled for Amsterdam |
| bal@v4.0.0 | 2026-01-23 | bal-devnet-2: adds EELS specs+tests for **7708, 7778, 7843, 8024** |
| bal@v5.0.0 | 2026-01-26 | **Same-day re-fill of the entire suite** after ACDT #67 (that morning) reverted 7778 receipt changes |
| bal@v5.1–5.6.1 | Jan 30–Apr 2 | bal-devnet-3 era |
| bal@v5.7.0/v6.0.0 | 2026-04-21/27 | bal-devnet-4: 8037 stabilization (+208 tests from devnet-3 issues), 7976/7981 new, 7928 index uint16→uint32; dynamic CPSB introduced then **struck through and reverted in the same release note** |
| snobal-devnet-5@v8037.0.0 | 2026-04-29 | Fixed CPSB=1174, frame-end accounting. Release notes are literally a row of frustrated emoji (100/crying/angry/nail-polish) — the team's editorial on the CPSB flip-flop |
| snobal-devnet-6@v1.0.0/1.1.0 | 2026-04-30 | bal-devnet-6 (Soldøgn interop) |
| tests-bal@v7.0.0/7.1.0/7.1.1 | 2026-05-11/13/18 | bal-devnet-7: fixes all devnet-6 bugs; CPSB 1174→1530; params changed; **v7.1.0 removes all SELFDESTRUCT state-gas refunds** (spec change 2 days after v7.0.0) |
| tests-bal@v7.2.0 | 2026-05-18 | Inline state-gas refund crediting bug **found by fuzzing with goevmlab** (EELS #2863) |
| tests-bal@v7.3.0–7.3.2 | Jun 10–15 | Coverage only |
| tests-glamsterdam-devnet@v6.0.0 | 2026-06-19 | New: **2780, 8038, 8246, 7997, 8282** (tracker #2915); devnet-6 launch 2026-06-24 |
| v6.0.1 / v6.1.0 / v6.1.1 | Jun 24 / Jun 25 / Jul 2 | 8038 dedicated suite; 2780/EIP-161 precompile bug fix (#3048); ~15 test PRs from chfast |
| v7.0.0 / v7.1.0 / v7.2.0 | **Jul 8 / 9 / 10** | Three releases in three days chasing EIP churn (EIPs #11844, #11858, #11891, #11854, #11899, #11902, #11908) |

## 2. EELS implementation timing per EIP

- **EIP-7928**: EEST impl merged Aug–Sep 2025 (PR #2067); first release Sep 4, 2025 — ~a year of continuous iteration. Spec updates it consumed: #11699 (May 21), #11750 (Jun 1, "EELS test added" per ACDT #81 agenda), #11854 (Jul 4), #11902 (Jul 9).
- **EIP-8037**: CFI'd Jan 20 (EIPs #11117); EELS tracker #2040 opened Jan 19; impl attempts #2181 (Feb 10), #2235 (Feb 18), full impl #2363 (spencer-tb, Feb 28). Thereafter EELS tracked ~20 EIP-text PRs within days each (see section 3/5).
- **EIP-8038**: EELS PR #2972 (danceratopz) opened **Jun 10 — two days before the EIP's "preliminary numbers" PR #11802 even existed** (Jun 12); both merged Jun 18; released Jun 19.
- **EIP-2780**: first EELS attempt #2175 (gurukamath) Feb 9 — four months before devnet inclusion; EIP reworked via #11645 (misilva73, May 11 → merged Jun 18); final EELS #3017 opened+merged Jun 18, released Jun 19. July restructure (#11844/#11891) → EELS #3126 (Jul 7–13).
- **EIP-7778**: EELS in bal@v4.0.0 (Jan 23); ACDT #67 receipt decision (Jan 26 morning) → EELS PRs #2073/#2074 and full re-release **the same day**.
- **EIP-7708 / 8024**: EELS in bal@v4.0.0 (Jan 23). **7976 / 7981**: added for bal-devnet-4 (Apr 27). **7954**: constant change EIPs#11540 → EELS #2987 (Jun 15–16) + new tests #2993/#2998 (danceratopz). **8246**: EELS #2842 (LouisTsai-Csie) opened May 12, merged Jun 18. **7997**: EELS #2802 (kclowes) May 4 → Jun 16. **8282**: EELS #2990 (marioevz) Jun 16–30.

## 3. Feedback direction: testing team → EIP specs (concrete, dated)

EIPs-repo PRs authored by testing-team members:
- **spencer-tb**: #11328 (Feb 17, clarify 8037 reservoir mechanics), #11421 (Mar 18, regular gas before state gas), #11532 (Apr 16, "more state gas accounting changes" — points 2–6 of the misilva/EELS review), #11548 (Apr 19, SSTORE restoration rollback semantics), #11399 (Mar 12, SFI the 8 EL EIPs). Also line-by-line review of #11292 the day after it merged ("The current EELS implementation does this").
- **danceratopz**: #11586 (Apr 30, 2780 missing 7708 log costs for CREATE), #11627 (May 8, **EIP-7708 had no gas costs specified for its logs at all** — PR adds TRANSFER_LOG_COST/BURN_LOG_COST; **still open in July 2026**, i.e. devnets run costs the EIP text doesn't define), #11818 (Jun 19, 8037 REGULAR_PER_AUTH_BASE_COST derivation).
- **Carsons-Eels (STEEL)**: #11084 (Jan 14, move 7708 to Draft), #11160 (Jan 23, add 7708 test cases), #11192 (Jan 28, tighten 7708 SELFDESTRUCT-log definition vs 6780), #11596 (May 3, Soldøgn interop decisions), **#11570 (Apr 24, wholesale "Proposal for Revising 8037": "reconcile issues... discovered during implementation and testing")**. misilva73's reply on #11570 — "Would a fixed cpsb sort the testing complexities and issues that this proposal is trying to address?" — led directly to #11573 (fixed CPSB, merged May 4).
- **fselmo**: #10364 (Sep 18, 2025, clarify 7928 empty-state-change expectations — two weeks after bal@v1.0.0), #11200 (Jan 28, **EIP-8024 example bytecode in the EIP was wrong** vs test case).
- **marioevz**: #11634 (May 8–16, simplify 7708 via 8246).
- EELS-side spec-bug issues: #2578 (Mar 27, 8037 pre-check stricter than EIP), #2748 (Apr 23, spec ambiguity on state_gas_used commit point), #3020 (Jun 19, CALL* NEW_ACCOUNT refund missing — found by misilva73 "while cross-checking those PRs against ethereum/EIPs#11807", fixed same day in the v6.0.0 release).

## 4. Devnet-found vs test/review-found

| Problem | First discovered | Evidence |
|---|---|---|
| **bal-devnet-6 "multiple bugs"** (6 gas-attribution bugs, May 2026) | **On the devnet**, by client teams: rakita (reth), benaadams (Nethermind: "in order to sync with Geth and Besu on bal-devnet-6... block 15149"), ethrex | EELS #2804 (May 5). All were exact-shape EEST coverage gaps (benaadams enumerated the missing fixtures, each off by exactly AccountCreationCost=131,488); EELS/EEST turned them into spec PRs (#2815/#2816/#2823/#2827/#2828) + EIP fixes (#11611/#11616 by misilva73) within 6 days → tests-bal@v7.0.0 May 11. These were *findable* by tests in principle — the spec ("pay-then-refund" semantics) was ambiguous enough that EELS itself disagreed with clients. |
| **glamsterdam-devnet-4 chain split** (~2026-05-25, slot 22800) | **On the devnet** — but it was a **CL/ePBS fork-choice bug**, not EL: "prysm-nethermind-1 forked on EMPTY after missing the 22800 payload" (ACDT #81 agenda, ethereum/pm#2085); root cause consensus-specs#5307 (`get_head()` vs `should_build_on_full()` conflict, filed by twoeths/Lodestar, May 28). Out of EL testing scope — EELS/EEST could not have caught it. |
| **8038 net-profitable x→0→x refund** (EIPs#11823, Jun 22) | **Spec-text review vs implementation, NOT devnet.** The EIP text had dropped EIP-2200/3529 reversal logic; the v6.0.1 tracker (#3001) records "**No changes in EELS required!**" — EELS/EEST had implemented it correctly. Found 4 days after the EELS 8038 merge, 2 days before devnet-6 launch. |
| **7928/8038 SSTORE stipend** (EIPs#11854, Jul 2–4) | **Implementation/review, via Discord discussion** — EELS fix #3064 (gurukamath, Jun 30: post-8038 COLD_STORAGE_ACCESS 3000 > 2300 stipend, so the EIP-2200 sentry no longer protects the read) was opened **before** the EIP fix (#11854, nerolation, Jul 2). No devnet incident is referenced anywhere. I could not read the Discord thread, so I can't rule out a client hitting it first, but the paper trail starts in spec/EELS space. |
| Bonus: 8037 inline-refund crediting bug (May 18) | **Differential fuzzing (goevmlab)**, credited to chfast/benaadams/rakita/fselmo — tests-bal@v7.2.0. |
| Bonus: 2780/EIP-161 zero-balance-precompile bug (Jun 24) | **Client cross-validation**: h/t rjl493456442 (geth) + gurukamath, EELS #3048 → v6.1.0 Jun 25, one day after devnet-6 launch. |

**Verdict on the hypothesis**: mixed but largely supportive. Of the four named incidents, only bal-devnet-6's bugs were genuinely devnet-discovered EL-spec problems — and those were precisely the class of thing tests catch (they became tests within a week). The devnet-4 split was CL-side and untouchable by the EL testing team. The two June/July "spec bugs" (11823, 11854) were caught by the EELS pipeline *before or independent of* devnets. Additionally, a large majority of 8037's spec churn (Feb–Jul: ~20 EIP PRs) was driven by EELS implementation/review (spencer-tb, misilva73's line-by-line EELS-vs-EIP diff in #2804, Carsons-Eels #11570, issues #2578/#2748/#3020), not by devnet failures.

## 5. Lag and churn

**Typical lag** (spec change → test release → devnet): remarkably short, often inverted:
- 7778 receipt change: ACDT decision and full suite re-fill on the **same day** (Jan 26).
- bal-devnet-7: EIP fixes merged May 4–13; test releases May 11/13/18; devnet ~May 18.
- devnet-6: EIP-8038 numbers + EIP-2780 rework merged **Jun 18**, EELS merged **Jun 18**, test release **Jun 19**, devnet launch **Jun 24** — a 1-day spec→tests lag and 5-day tests→devnet lag, with EELS impl PRs opened *before* the EIP text they implement (#2972 vs #11802; #3064 vs #11854).

**Churn complaints (explicit, on the record):**
- **EELS #2744** (spencer-tb, Apr 22): "our release process is extremely messy... For larger EIPs like EIP-8037 this diff can get very big... this had taken me personally around 2 hrs to get right... cross-EIP regressions surface... at devnet compose time with everything on fire at once"; 8 branches to maintain, "3 more CFI'd EIPs... so this moves to 11 branches ;)". Proposed "Optimistic Inclusion" to fix it.
- **The CPSB flip-flop**: fixed 1174 (devnet-3) → dynamic-from-gas-limit in the EIP (#11292, Feb 9) → EELS/EEST framework rebuilt to thread block-gas-limit into every gas function (#2687, Apr 15–21, touching the whole fixture surface) → **reverted to fixed six days later** (#11573, Apr 27) after misilva73 asked whether fixed CPSB would "sort the testing complexities" (on #11570). The snobal-devnet-5 release notes' emoji-only header is the team's reaction. Note: I could **not** locate the specific ">600 test files, ~Feb 2026" citation from the task — the determinism concern is documented (dynamic CPSB makes every filled fixture depend on fill-time gas limit; #2687, #11570 thread), but the 600-file figure was likely said on a call or Discord, not in a searchable issue.
- **#2708** (Apr 17): refactor solely "to avoid merge-conflicts due to 8037".
- Three releases in three days (Jul 8–10) for devnet-7; SELFDESTRUCT refund removal two days after a major release (v7.1.0).

## Could not determine
- The exact source of the ">600 test files" figure (above).
- Who first raised the SSTORE-stipend issue on Discord (link inaccessible).
- Whether glamsterdam-devnet-5's failure (relaunched as devnet-6 per ACDT 82; later "recovered" per spencer-tb, #2915) had any EL component.
- ethpandaops/glamsterdam-devnets has only one public issue (devnet-3 finality stall, May 11); devnet incident reports otherwise live in Discord/calls, so devnet-found bug attribution rests on ACDT agendas (ethereum/pm#2085) and EELS issue cross-references.
