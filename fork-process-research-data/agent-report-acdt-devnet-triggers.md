The fusaka baseline agent didn't return in time and the two web-research agents' outputs were never delivered, so per the coordinator's instruction I'm writing the report now strictly from the primary sources I verified directly (ethereum/pm issues #1769–#2151, ethpandaops config repos, execution-specs/consensus-specs/client bug trackers, potuz's devnet-0 post-mortem). Anything I could not verify is flagged as unknown.

---

# Glamsterdam devnets: why each was launched, what each actually found

**Sources.** ethereum/pm ACDT #62–87 (issues 1820, 1834, 1842, 1865, 1878, 1882, 1901, 1913, 1920, 1932, 1948, 1956, 1966, 1976, 1988, 2008, 2019, 2038, 2049, 2085, 2103, 2116, 2126, 2133, 2144, 2151), ACDE #230–240 (1921, 1931, 1955, 1970, 2004, 2015, 2033, 2044, 2076, 2115, 2140), plus pre-scope context ACDT #58–61 / ACDE #225–229. Launch dates are from first commits to `network-configs/devnet-N` in ethpandaops/bal-devnets, ethpandaops/glamsterdam-devnets, ethpandaops/epbs-devnets. Bug evidence from ethereum/execution-specs, ethereum/consensus-specs, erigon, lodestar issue trackers and potuz.net. **Not obtained:** forkcast.org call summaries and the notes.ethereum.org spec sheets (sub-agent results never landed), and the Soldøgn interop recap blog — gaps flagged below.

## Timeline (hard evidence from config-repo first commits)

| Devnet | Config first commit | Trigger category (see per-devnet) |
|---|---|---|
| bal-devnet-0 | 2025-10-28 | (a) headliner start: EIP-7928 |
| bal-devnet-1 | 2025-12-18 | (b) 7928 spec change |
| bal-devnet-2 | 2026-02-06 | (a) +7708, 7778, 7843, 8024 |
| bal-devnet-3 | 2026-04-08 | (a) +8037 (+7954) |
| bal-devnet-4 | **never deployed** | skipped (process decision) |
| bal-devnet-5 | 2026-04-28 (interop) | (b) 8037 rework |
| bal-devnet-6 | 2026-05-01 (interop) | (b) 8037 fixes |
| bal-devnet-7 | 2026-05-19 | (b)+(a) devnet-6 fixes, CPSB 1530, eth/70+71 mandatory; "last EL-only devnet" |
| glamsterdam-devnet-0/1 | no public configs (interop week ~Apr 27–30) | (d)+(e) first merged EL+CL (Gloas) nets |
| glamsterdam-devnet-2 | 2026-05-01 | (c)/(d) interop iteration |
| glamsterdam-devnet-3 | 2026-05-06 | (c) post-interop consolidation |
| glamsterdam-devnet-4 | 2026-05-22 | (b)+(e) bal-devnet-7 EL + targetGasLimit + CL alpha.8 |
| glamsterdam-devnet-5 | 2026-06-04 | (c) devnet-4 chain split; CL stability mandate |
| glamsterdam-devnet-6 | 2026-06-25 | (a) big EL EIP integration (2780, 8038, 8246, 7954, 7997) |
| glamsterdam-devnet-7 | ~2026-07-10 (repo add), deploying at ACDT #87 | (a)+(b) EIP-8282, 7688, stipend/runtime-charge spec fixes |
| (context) epbs-devnet-0 | 2026-03-04 | (e) CL-driven, EIP-7732 |
| (context) epbs-devnet-1 | 2026-03-31 | (e) new CL spec release (consensus-specs #4858) |

---

## bal-devnet series (EL-only, EIP-7928 substrate)

### bal-devnet-0 (genesis ~late Oct/early Nov 2025)
1. **Trigger — (a):** first devnet for the Glamsterdam EL headliner EIP-7928 (BALs). Pre-launch client readiness was tracked on ACDT #58 (pm#1769, Oct 20: qu0b — "besu, geth seem ready, nethermind nearly ready, reth no idea, erigon work in progress"). Standing agenda item from ACDT #60–65.
2. **Devnet-only findings:** *unknown from the sources gathered* — ACDT #62–65 issue bodies carry only "bal-devnet-0 updates" with no substance in comments. (The forkcast/pandaops-notes fetches that would have filled this did not return.)
3. **Spec-level issues knowable beforehand:** the self-destruct-OOG EEST test (execution-specs#1846) "affected multiple clients" (qu0b, ACDT #64, pm#1842, Dec 15) — found in test filling, not on the devnet.
4. **Decision process:** launched by ethpandaops/testing team as the natural post-headliner-selection step; no recorded pushback.

### bal-devnet-1 (config Dec 18, 2025)
1. **Trigger — (b):** spec change to EIP-7928: BAL type-definition changes in the execution-specs 2.0 release (execution-specs#1912) plus RLP-encoding reference updates; clients asked to cut `bal-devnet-1` branches and add a BAL debug endpoint (qu0b, ACDT #64, pm#1842, Dec 15).
2. **Devnet-only findings:** *unknown* — agendas through ACDT #66 only say "bal-devnet-0/1 updates".
3. **Knowable beforehand:** the type-definition/RLP churn itself; EEST bal@v2.0.0 was still filled against devnet-0 pending client branches (same comment) — i.e., the trigger was upstream spec work, not network evidence.
4. **Decision process:** ethpandaops (qu0b) drove it; mechanical follow-on, no pushback recorded.

### bal-devnet-2 (config Feb 6, 2026; targeted "Wednesday 28.01")
1. **Trigger — (a):** scope expansion around the BAL substrate: EIP-7708 (ETH transfer logs), EIP-7778 (block gas accounting), EIP-7843 (SLOTNUM), EIP-8024 (SWAPN/DUPN/EXCHANGE). Scoped at ACDT #66 (pm#1878, Jan 16: spencer-tb proposed keeping 8024/7778/7708, deferring 7843) and ACDE #228 (pm#1867, Jan 15: qu0b asked whether 7843 stays given its small CL change). 7843 ended up included (qu0b's readiness matrix, ACDT #67, pm#1882, Jan 26).
2. **Devnet/integration-only findings** (qu0b's kurtosis + devnet reports, ACDT #68 pm#1901 Feb 2 and #67):
   - **Nimbus-EL activated Gloas opcodes (SLOTNUM, EIP-8024) at Fulu instead of Gloas → chain split under fuzzed transactions.** Explicitly a multi-client-under-load find: "Without fuzz transactions, Nimbus works perfectly in multi-client networks" (qu0b, Feb 2).
   - **Nethermind `engine_getPayloadV6` omitted `blobGasUsed`** → Lodestar/Lighthouse rejected payloads; Nethermind could validate but not propose (engine-API integration find).
   - Geth: 6 BAL state-transition bugs (go-ethereum#33735) surfaced by fuzzed multi-client runs.
   - Lighthouse `upgrade_to_gloas` hardcoded slot 0 (sigp/lighthouse#8726).
3. **Knowable beforehand / spec-level:**
   - The **EIP-7778 `gasSpent` receipt-field consensus problem** was flagged before spec freeze: EEST bal@v4.0.0 was released with `gasSpent` in receipts while the spec was still contested (fselmo, ACDT #67, Jan 24: "this is a consensus issue on receipts roots so no client can test against this latest test release"). The Besu/Nethermind "remove gasSpent from receipt RLP trie" blockers were fallout of that unfrozen spec, not new network knowledge.
   - Launch was known-blocked on client EIP gaps: "most kurtosis integration testing is currently blocked because apart from geth no EL client has merged the SLOTNUM EIP" (qu0b, Jan 26).
4. **Decision process:** qu0b set the date ("Lets get bal-devnet-2 up and running by Wednesday 28.01", ACDT #67); marioevz used the same call to propose a formal **CFI→devnet process** (EIPs must be posted to the ACDT agenda ahead of the call; silence = proceed; SFI/DFI decisions stay on ACDE/ACDC) — evidence the prior process was ad hoc. No "do we need this devnet" pushback; the debate was about spec freeze discipline.

### bal-devnet-3 (config Apr 8, 2026; originally targeted "> 2026-03-11", ACDT #73)
1. **Trigger — (a):** adding **EIP-8037** (state-creation gas / multidimensional metering) to the BAL substrate (plus 7954 in the hive "bal-quick" group, ethpandaops/bal-devnets#37). Readiness tracked ACDT #71–73 (pm#1932, #1948, #1956).
2. **Devnet-only findings:** thin. The launch slipped ~4 weeks for reasons **upstream of the network** (below). It later served as the long-lived **benchmarking baseline** (clients quoted ~3x exec speed-up on devnet-7 branches "vs devnet-3", ACDT #81 pm#2085). Related live find on its Kurtosis twin: erigon nondeterministic parallel-execution producing wrong trie roots at tip under tx load (erigontech/erigon#22152, referenced as "the previous devnet" in erigon#22254).
3. **Knowable beforehand:**
   - EIP-8037's dynamic `cost_per_state_byte` broke the EELS/EEST testing paradigm — ~30% of tests needed modification, edge-case OOG tests became unpredictable (spencer-tb, ACDT #71, Feb 23; marioevz's four-point testing-concern list, ACDE #235, pm#2015, Apr 23). MariusVanDerWijden floated postponing 8037 to devnet-4 rather than butchering the framework (ACDT #71).
   - The pre-launch "cross-client bug hunt" caught geth/besu/nimbus-eth1 8037 bugs **before genesis** (ACDT #73 agenda: go-ethereum#33972, besu#9994, nimbus-eth1#4036; new coverage from benaadams, execution-specs#2426).
   - Erigon's architectural objection to 8037 ("adds the 4th dimension to gas... impossible to reason about", yperbasis, ACDE #232 pm#1955, Mar 12) and STEEL's shippability warning (petertdavies, ACDE #235, Apr 23) — both raised while the devnet was live/launching; the devnet did not surface these, implementation did.
4. **Decision process:** ethpandaops + STEEL sequenced it; the real gate was test-framework readiness, not client binaries. Retirement had to be asked for explicitly months later: "Anyone still using bal-devnet-3 or can we retire it?" (qu0b, ACDT #84, pm#2126, Jun 22) — a small but concrete pandaops-capacity signal.

### bal-devnet-4 — never publicly deployed
- Spec discussed at ACDT #77 (pm#2008, Apr 13, qu0b); `testing_buildBlockV1` was planned "for bal-devnet-4 onwards" (parithosh, ACDT #71, Feb 17); infra was prepared (bal-devnets PR #44 terraform Apr 14, issue #47 ansible Apr 26) — but no `network-configs/devnet-4`, `terraform/devnet-4` or `ansible/inventories/devnet-4` exists in the repo today.
- **Why skipped:** MariusVanDerWijden, ACDT #78 (pm#2019, Apr 20): "I propose to go straight to glamsterdam-devnet-0 for ELs instead of going to BAL-devnet-4, since we can use non-epbs CL nodes for BAL testing... (The only change for ELs to support full glamsterdam is to allow for reorging the head block)". The interop event the following week produced bal-devnet-5/6 instead.
- Categorize as: **process decision to collapse the EL-only track into the merged track**, partially reversed at interop (devnets 5/6 still EL-only). Whether a short-lived devnet-4 ran privately at interop: *unknown*.

### bal-devnet-5 (config Apr 28, 2026 — interop week) and bal-devnet-6 (config May 1, 2026 — interop week)
1. **Trigger — (b):** rapid iterations during the Soldøgn interop (event referenced in ACDE #236 agenda, pm#2033, and execution-specs#2804: "Latest EIP changes made at the end of Soldøgn interop"). The 8037 parameterization was reworked across these spins (EIPs #11573, #11616; CPSB later moved 1174 → 1530). Exact devnet-5 vs devnet-6 spec delta: *unknown* (pandaops notes not retrieved).
2. **Findings (devnet-6):** "Multiple bugs found" — the canonical list is execution-specs#2804 (fselmo, May 5): 8037 halt-semantics state-gas refund to top call; tx-create halt/revert refund missing; `block.state_gas_used` excluding the 7702 state refund; refund wrongly propagated on sstore 0→x→0 with same-frame revert; create-collision test not consuming regular gas. **Important nuance for Q3:** these were "test discrepancies reported... highlighted by @rakita/@benaadams & ethrex" — i.e., cross-client *differential testing around the devnet branches*, mostly spec/test-level and arguably knowable pre-genesis; the devnet mainly forced the confrontation on a deadline.
3. **Spec output:** EIPs #11611 ("Fix bugs from bal-devnet-6") and #11616 (parameters) — discussed ACDT #79 (pm#2038, May 11, misilva73).
4. **Decision process:** interop-driven, EF testing team; no on-issue pushback (in-person coordination, minimal paper trail — flagged as an evidence gap).

### bal-devnet-7 (config May 19, 2026)
1. **Trigger — (b)+(a):** consolidation devnet: devnet-3 optimizations + devnet-6 8037 fixes + `cost_per_state_byte`=1530, and **eth/70 (EIP-7975) and eth/71 (EIP-8159) made mandatory** (spec sheet posted by qu0b at ACDT #79, May 11; detailed at ACDT #80, pm#2049). Explicitly framed as terminal: "This should be the last EL only devnet" (qu0b, ACDE #236, pm#2033, May 7); "`bal-devnet-7` — the last EL-specific devnet; EL benchmarking" (ACDT #83 agenda, pm#2116, Jun 15).
2. **Devnet-only findings:**
   - Its main job was **live benchmarking for the gas repricings** (8037/8038/2780 numbers): "Stable, 100% EVM fuzzing participation"; per-client BAL-optimization status; **Nethermind OOM distorting benchmark results**; Besu AOT + OOM fix shipped (ACDT #81, pm#2085, Jun 1).
   - **BAL-divergence class of bug:** geth's EIP-7702 re-delegation produced a *builder/validator split* — "miner charges 35,190 state-gas, validator 0" — diverging BAL hash and state root; plus an erigon split and a besu cap issue (ACDT #83 agenda, commented-out section). The same agenda names the systemic reason this class **only** shows up on devnets: "EELS does payload re-execution only, no block-building tests — which is why this slipped" (fix: execution-specs#2945 adds building/rejection tests). This is the single clearest devnet-only-discoverable EL finding in the whole series.
3. **Knowable beforehand:** the 35,190-gas 8037 auth-refund asymmetry also reproduced as a *flaky local test* (erigon#22038, Jun 25 — parallel-exec nondeterminism) — i.e., part of the class was reachable by better local tests, once block-building tests existed.
4. **Decision process:** ethpandaops (qu0b) + repricing group (misilva73). It also became the vehicle for forcing eth/70+eth/71 client work ("Would be great if clients could confirm work on eth/70 and eth/71 is done", qu0b, ACDT #81) — a devnet used as a compliance deadline.

---

## glamsterdam-devnet series (merged EL+CL, Gloas/ePBS)

### Context: epbs-devnet-0 (Mar 4) / epbs-devnet-1 (Mar 31)
CL-only precursors (category (e)). epbs-devnet-0 was on ACDT agendas from Dec 2025 but launched only Mar 4, 2026. Launch week (per potuz's post-mortem, linked from ACDT #73): two Prysm bugs (missing state-diff replay logic; payload-signature validation rejecting instead of queuing) cascaded into **fork-choice divergence between Prysm and Teku, peer downscoring that fully isolated Prysm from Lodestar/Lighthouse, and unexplained payload-envelope gossip persistence** — textbook only-on-live-multi-client findings. Also from devnet-0: ePBS forcing the EL to trigger block production on older heads, and the PTC-lookahead problem (consensus-specs#4979) — both raised by potuz at ACDT #73 (pm#1956, Mar 9). epbs-devnet-1 was cut for the consensus-specs v1.7.0-alpha spec release (consensus-specs#4858, ACDT #68 agenda).

### glamsterdam-devnet-0 and -1 (interop week, ~Apr 24–30; no public configs)
1. **Trigger — (d)+(e):** first merged EL+CL Glamsterdam devnets, proposed by MariusVanDerWijden at ACDT #78 (Apr 20) as a *replacement* for bal-devnet-4 — client readiness rationale: the only EL delta for full Glamsterdam was head-ancestor reorg support (execution-apis#770, on the ACDT #78 agenda). Spun during the Soldøgn interop.
2/3. **Findings:** *unknown* — no public configs, no issue-tracker traces found; the interop recap blog was not retrieved. The rapid 0→1→2 respin (devnet-2 config on May 1) implies early instances were short-lived, but I have no direct evidence of the specific failure that killed devnet-0 or -1. **Flagged unknown.**
4. **Decision process:** on record only as Marius's ACDT #78 proposal + interop execution; no pushback recorded.

### glamsterdam-devnet-2 (config May 1) and -3 (config May 6)
1. **Trigger:** post-interop consolidation of the merged stack; exact spec deltas *unknown* (pandaops notes not retrieved). The 5-day 2→3 respin suggests devnet-2 was replaced quickly (category (c) presumed, unverified).
2. **Devnet-only findings (devnet-3):** **finality stall from epoch 492 for ~2.5 days** — chain kept producing blocks with ~43/100 missed slots, participation below 2/3; knock-on: checkpoint-sync anchored 18k+ slots behind head and discv5/peer stability degraded for joining nodes, making range sync through the non-finalized gap nearly impossible (ethpandaops/glamsterdam-devnets#3, filed May 11). That non-finality-degrades-joining dynamics is a live-network-only observation.
3. **Knowable beforehand:** *unknown* root cause attribution for the participation collapse (the issue documents symptoms, not cause).
4. **Decision process:** interop/pandaops-driven; devnet-3 was left running while attention moved to devnet-4 (ACDT #75/#76 agendas list bal-devnet-3/4 + epbs-devnet-1; glamsterdam devnets enter the ACDT agenda from #79, May 11).

### glamsterdam-devnet-4 (config May 22)
1. **Trigger — (b)+(e):** merge of the near-final EL set with fresh CL spec: "`glamsterdam-devnet-4`: `bal-devnet-7` + `targetGasLimit` on `PayloadAttributesV4` + consensus-specs `v1.7.0-alpha.8`" (ACDT #80 agenda, pm#2049, May 18).
2. **Devnet-only findings:**
   - **Chain split ~slot 22800 (~May 25): prysm-nethermind-1 forked onto EMPTY after missing the slot-22800 payload** (ACDT #81 agenda: "glamsterdam-devnet-4 not so healthy as of ~May 25").
   - Root-caused to a **Gloas fork-choice spec defect**: a proposer whose `should_build_on_full()` disagreed with `get_head()` after skipped slots builds on `(X, EMPTY)` while the network keeps `(X, FULL)` as head, so the proposal is deterministically orphaned (consensus-specs#5307, twoeths, May 28; live logs in ChainSafe/lodestar#9415; potuz confirming the `should_build_on_full` tie-breaker misuse in the issue comments). A spec bug — but one whose *symptom* (orphaned proposals/split under skipped-slot + PTC-vote conditions) realistically only manifests in live multi-client fork-choice interaction.
3. **Knowable beforehand:** arguably the fork-choice edge case was reachable by spec analysis/fork-choice test vectors (the fix went through `get_proposer_head` spec PRs #5348/#5305, ACDT #83 agenda), but nobody had constructed the scenario before the devnet hit it.
4. **Decision process:** scoped at ACDT #80 explicitly as "prep discussion for a fast resolution in ACDE #237". At ACDT #81 (Jun 1) the question was posed outright: **"Recover the chain or abandon for devnet-5?"**

### glamsterdam-devnet-5 (config Jun 4)
1. **Trigger — (c):** devnet-4 broken (chain split above) plus a **CL stability mandate**: "CL ask from ACDC #179: stability first, no scope increase for 2 weeks" (ACDT #81 agenda). ACDT #81 laid out four scoping options, including **running two devnets simultaneously (glamsterdam-devnet-5 for CL stability + bal-devnet-8 for close-to-final EL spec, merging into devnet-6)** — the single-relaunch option won; bal-devnet-8 never happened.
2. **Devnet-only findings:**
   - Early instability nearly killed it: **Prysm peering bug + Grandine fork-choice bug**, root-caused at ACDC #180 and fixed; the network was "salvaged by @barnabasbusa (not abandoned)" (ACDT #83 agenda, Jun 15).
   - Lodestar live-network issues during the devnet-5 window: missing `PayloadEnvelopeInput` for a known block (lodestar#9475, Jun 8); SSE events confusing Dora (lodestar#9477); **peer-scoring DIAL_TIMEOUT bans causing self-inflicted peer starvation in degraded networks** (lodestar#9562, Jun 29); block production dropping same-FFG attestations that vote for the other payload-status variant (lodestar#9596, Jul 5).
   - Devnet experience fed a **range-sync design challenge**: "data columns and payload envelopes by range is just dysfunctional and really error prone... how about we completely get rid of by-range and always query by root?" (jtraglia relaying nflaig, ACDT #82, pm#2103, Jun 8).
   - Builder-path testing gap surfaced as process pain: "Any latest update on builder API integration into kurtosis... Or just yolo it on devnet?" (terencechain, ACDT #82) — i.e., mev/builder flows had no local harness and could only be exercised on devnets.
3. **Knowable beforehand:** the Prysm/Grandine bugs were client implementation bugs; whether they were reachable by local fork-choice tests is *unknown* — the record only shows devnet discovery.
4. **Decision process:** ACDT #81 (Jun 1) hosted the explicit option-weighing; the "next steps" on ACDT #82 were "Get all CL clients included in glamsterdam-devnet-5. Finalize scope (both layers) for glamsterdam-devnet-6."

### glamsterdam-devnet-6 (config Jun 25)
1. **Trigger — (a):** "target for the larger EL EIP integration" (ACDT #83 agenda): EIP-2780 rework, EIP-8038 first numbers, EIP-8246 (SELFDESTRUCT burn removal, required by 7708), EIP-7954 larger sizes, EIP-7997 pivoted to the Arachnid keyless factory (EIPs#11783 following ACDT #82's Option 2), 8037 refund decisions; tracker: execution-specs#2915; tests-glamsterdam-devnet@v6.0.0 + consensus v1.7.0-alpha.11 (ACDT #84 agenda, pm#2126).
2. **Devnet-only findings:**
   - **Erigon's builder produced invalid payloads (wrong state root) specifically under ePBS payload-orphan reorg churn** — 3 invalid blocks on Jul 3 incl. slot 57122/block 43132; the builder sealed `stateRoot=0x275b…` while every executor (nethermind, ethrex, *erigon's own execution stage*) computed `0xbb2c…`; "bid charged, payload not included" (erigontech/erigon#22254, Jul 6). Builder-vs-validator divergence under live reorg pressure is exactly the class local tests missed.
   - Ongoing devnet-6 status was tracked in a Discord thread by qu0b (linked from ACDT #87 agenda, pm#2151) — content *not retrieved*, flagged.
3. **Knowable beforehand / spec-level found in parallel:** the Gloas attestation bug — `data.index == 1` attestations *always* failing payload matching due to `slot_index` using `data.slot` (consensus-specs#5399, michaelsproul, Jun 25) — was found by **spec reading**, not the devnet; its fix (#5355) then gated devnet-7 (barnabasbusa, ACDT #86, pm#2144, Jul 6). Likewise the EIP-8038 refund bug came from an Eth-Magicians reader (misilva73, ACDT #84, Jun 22).
4. **Decision process:** scope finalized across ACDT #82–83 and ACDE #238/#239; EIP-8282 (builder deposits) was deliberately **held out** of devnet-6 pending a go/no-go: "Deferred here by ACDC #180... Prelim decision here in ACDT, ratify at the next ACDE" with an explicit "fork-delay risk of a late change" warning (ACDT #83 agenda).

### glamsterdam-devnet-7 (repo add Jul 10; deploying at ACDT #87, Jul 13)
1. **Trigger — (a)+(b):**
   - (a) **EIP-8282 builder deposits** enters, with new system contracts and a **new deposit contract address** (`0x00006AE8…8282`) — "make sure you hardcode the new deposit contract address" and "merge state should be settled before the devnet-7 genesis bytecode is cut" (barnabasbusa, ACDE #240 pm#2140, Jul 2; ACDT #86, sys-asm#49); plus **EIP-7688** inclusion requested by tersec (ACDT #84, Jun 22) after etan-status showed 200M-gas blocks overflow the deposit-request SSZ limit (ACDT #82, Jun 2).
   - (b) spec changes to already-included EIPs: EIP-7928 stipend-check clarification "needed as of devnet-7" (nerolation, ACDT #86, EIPs#11854); EIP-2780/8037 state-dependent charges moved to runtime (EIPs#11844, then #11908 merged per ACDT #87); EIP-8038 SSTORE access-cost ordering fix ("Sentry check OOG DoS", qu0b's devnet-7 spec-update list, ACDE #240); CL v1.7.0-alpha.12 including the #5399 fix (#5355).
2. **Findings:** too early — deployment in progress at the ACDT #87 snapshot (Jul 13). *Unknown.*
3. **Knowable beforehand:** the entire devnet-7 delta is pre-identified spec work — this devnet is a verification round, not a discovery round.
4. **Decision process:** the 8282 ACDT-prelim/ACDE-ratify two-step (ACDT #83 → ACDE #239) is the clearest instance of the formalized devnet-gating process in the whole series.

---

## Meta-discussion: cadence, gating, capacity

- **CFI→devnet process formalization:** marioevz, ACDT #67 (Jan 26): EIPs proposed for the next devnet must be posted to the ACDT agenda ahead of time; no objections on the call → proceed; EIPs can still be pulled later; **SFI/DFI decisions explicitly excluded from ACDT** (stay in ACDE/ACDC).
- **Devnet-inclusion ≠ SFI, and sunk-cost worry:** spencer-tb pushed "SFI all devnet EIPs per EIP-7723" repeatedly (ACDE #232 Mar 12, ACDE #233 Mar 26, ACDT #74 Mar 16 — bounced between calls three times before landing). nerolation: "it should not become 'the process' that included-in-a-devnet means = SFI... the SFI decision should be explicit" (ACDT #74). parithosh: "an EIP is added to devnets mostly based on convenience of sequencing and testing — usually not based on importance" (ACDT #74). fselmo's sharpest version: "the more time that passes without SFIing EIPs that were in previous devnets, with more EIPs being put on top... the more risk in our process to **passively push things through because of sunken cost**. We should find time to talk about devnet process" (ACDE #233, Mar 26). Resolution: SFI definition updated (EIPs#11475) and bal-devnet-6's EIPs batch-SFI'd ~May 13 (noted ACDT #80).
- **Call/devnet cadence:** ACDT #73 (Mar 9) agenda item: "ACDT call format/cadence discussion (**biweekly proposal**)"; the very next week's call was nearly cancelled — "If no compelling topics are proposed... we would propose to cancel the call in favor of a breakout room" (ACDT #74 body; it wasn't cancelled). ACDT #78→#79 has a 3-week gap (event conflict + Soldøgn interop). From ~March the calls restructured into plenary + standing BAL and ePBS breakouts; the separate Gas-Repricing breakout was dissolved back into ACDT once specs stabilized (ACDT #81, Jun 1).
- **"Last EL-only devnet":** declared in advance for bal-devnet-7 — qu0b, ACDE #236 (May 7): "This should be the last EL only devnet"; restated as fact in the ACDT #83 agenda (Jun 15). MariusVanDerWijden's ACDT #78 proposal to skip bal-devnet-4 for glamsterdam-devnet-0 is the same sentiment three weeks earlier — the EL-only track was seen as ending.
- **Capacity/parallelism:** never a headline agenda item, but visible at the edges: qu0b asking to retire bal-devnet-3 (ACDT #84); the ACDT #81 devnet-5 scoping explicitly weighing **"launch two devnets simultaneously"** as a cost worth debating rather than a default; bal-devnet-2 "winding down" announced (ACDT #73); through Feb–Mar pandaops was concurrently running blob-devnet-0, perf-devnet-2/3, nft-devnet-10, epbs-devnet-0/1 and two bal devnets (ACDT #70/#72 agendas). Also the "recover vs abandon" framing for broken devnets (devnet-4, ACDT #81; devnet-5 "salvaged... not abandoned", ACDT #83) shows relaunches were treated as having real cost.
- **What gates a launch (as practiced):** (1) per-devnet EEST tagged release (bal@vN…, tests-bal@v7.x, tests-glamsterdam-devnet@v6.0.0) — sometimes explicitly a **spec freeze** fight (fselmo on 7778, ACDT #67); (2) qu0b's kurtosis client×EIP integration matrix (ACDT #67) with launch blocked on stragglers ("apart from geth no EL client has merged SLOTNUM"); (3) hive dashboards (hive.ethpandaops.io bal-quick/bal groups, ACDT #80/#81); (4) for devnet-7, settled genesis system-contract bytecode (ACDE #240).

## Fusaka-era baseline
**Not completed.** The sub-agent tasked with the mid-2025 fusaka ACDT sample did not return before this report was due. The only in-scope signals I verified directly: abcoathup's Fusaka-vs-Glamsterdam scoping table warning "Glamsterdam would have the most EIPs ever if all CFI'd EIPs are SFI'd into devnets" (ACDE #227, pm#1854, Dec 29) and the ACDE #225 "minimum hard fork testnets & mainnet rollout timeline" thread (Nov 2025) that framed the Glamsterdam devnet schedule (illustrative mainnet June 24, 2026 → testnets by mid-April). A proper fusaka-devnet cadence comparison remains open.

## Explicitly unknown / unverified
- bal-devnet-0/-1 concrete findings (ACDT #62–65 agendas are placeholders; forkcast/pandaops-notes fetches not returned).
- bal-devnet-5 vs -6 exact spec deltas; whether any bal-devnet-4 or glamsterdam-devnet-0/-1 instance briefly ran at interop.
- Root cause of the glamsterdam-devnet-3 participation collapse (only symptoms documented in ethpandaops/glamsterdam-devnets#3).
- glamsterdam-devnet-6 rolling status (qu0b's Discord thread, ACDT #87) and everything about devnet-7 post-genesis.
- forkcast.org call summaries (ACDT #80+ have them, e.g. forkcast.org/calls/acdt/080) and notes.ethereum.org spec sheets — cited in agendas but not fetched; they would refine per-devnet EIP tables and the devnet-0/1 gaps.
