All data collected. Here is the full report.

# Glamsterdam fork devnets — EL spec scope, deltas, and launch rationale

Sources: raw HackMD notes (`notes.ethereum.org/@ethpandaops/{bal,glamsterdam}-devnet-N`, fetched via `/download`), `github.com/ethpandaops/bal-devnets` and `github.com/ethpandaops/glamsterdam-devnets` (network-configs `metadata/config.yaml` `MIN_GENESIS_TIME`, git history, READMEs). Raw note copies saved in `/tmp/claude-1000/-media-jochem-research-disk-EIPs/16ffd9db-c6fc-4b71-abd6-03e39402312a/scratchpad/` (`bal-0.md`…`glam-7.md`).

**Date corrections vs the "known dates" you gave** (all verified against on-repo genesis configs):
- **bal-devnet-3 launched 2026-04-08 14:30:00 UTC**, not 2026-03-04 (note says "targets to launch 08 April 2026"; `MIN_GENESIS_TIME: 1775658600`).
- **bal-devnet-4 never launched as a public devnet.** No `network-configs/devnet-4` exists; a genesis was prepared ("devnet-4: set genesis to 2026-04-27 15:00 UTC", 2026-04-26) and then the branch was renamed — commit `rename bal-devnet-4 -> bal-devnet-5` (2026-04-28). So "2026-04-22" is not a launch date for it.
- bal-devnet-7 genesis was **2026-05-19 13:05:00 UTC** (target was Monday 18.05).
- glamsterdam-devnet-0 genesis was **2026-04-29 02:07:42 UTC** (targeted 24 April); glamsterdam-devnet-2 genesis **2026-05-01 12:59 UTC** (targeted 30 April); glamsterdam-devnet-6 genesis **2026-06-25 11:29 UTC** (note says "targets 26th June").

---

## BAL devnet series (EL-focused, CL pinned on consensus-specs v1.6.x / Gloas)

### bal-devnet-0
1. **Genesis:** 2025-11-04 17:02:44 UTC (targeted end Oct 2025).
2. **EL EIPs:** EIP-7928 (Block-Level Access Lists) only. Spec refs: execution-apis PR #691 (BAL `ExecutionPayloadV4`).
3. **Test pins:** EEST `bal@v1.8.0` (pre-release); Consensus specs `v1.6.0-beta.0`. No EELS branch pinned.
4. **Delta:** n/a (first devnet).
5. **Reason:** First interop devnet for EIP-7928 BAL (client TDD tracked via "pokebal" dashboard). All EL clients paired with Lodestar.
6. **Issues:** "Prysm does not manage to peer with lodestar after the Gloas fork. All Prysm blocks are orphaned" — CL client bug. No EL spec issues documented in the note.

### bal-devnet-1
1. **Genesis:** 2025-12-18 13:00:00 UTC (note says original target was "early Jan 2026", launched early).
2. **EL EIPs:** EIP-7928 only. Execution-apis PR #691 again.
3. **Test pins:** EEST `bal@v2.0.0`; Consensus specs `v1.6.0-beta.0` (unchanged).
4. **Delta vs devnet-0:** EEST bump `bal@v1.8.0 → bal@v2.0.0`; otherwise same scope. Client config additions: Besu `--Xbal-trust-state-root=true`, Geth `--history.state=0 --gcmode=archive --syncmode=full`.
5. **Reason:** Not explicitly stated; effectively a re-run of BAL interop against the newer EEST release (and the devnet Prysm gained EIP-7928 support from this devnet onward, per later trackers).
6. **Issues:** none documented in the note.

### bal-devnet-2
1. **Genesis:** 2026-02-06 14:29:50 UTC (targeted 04 Feb 2026).
2. **EL EIPs:** EIP-7708 (🆙), EIP-7778 (🆕), EIP-7843 (🆕), EIP-7928 (🆙), EIP-8024 (🆕, "included WITHOUT" EIPs#11094 push-postfix encoding switch). EIP spec pins via merged EIP PRs: 7708 #9003/#11127/#11187/#11190; 7843 #11083; 7928 #10990/#11066/#10895/#10940; 7778 #11191/#11138. EELS PRs: #2023 (7708), #1401 (7778), #2007 (7843), #1719+#1917 (7928), #2021 (8024). Engine API: Amsterdam spec, `engine_forkchoiceUpdatedV4` (execution-apis #731), #691, #727 merged; #726 (BAL JSON-RPC) still open.
3. **Test pins:** EEST `bal@v5.1.0`; Consensus specs `v1.6.1`.
4. **Delta vs devnet-1:** +EIP-7778, +EIP-7843, +EIP-8024 new; EIP-7708 newly in scope (marked 🆙 relative to its EIP text); EIP-7928 spec updates (spurious entry detection #10990, gas validation #11066); consensus specs `v1.6.0-beta.0 → v1.6.1`; EEST `v2.0.0 → bal@v5.1.0`. New requirement: EL clients must add flags to toggle BAL optimizations (batch/parallel IO, parallel exec).
5. **Reason:** Broaden from BAL-only to the wider Glamsterdam EL candidate set (repricing/opcode EIPs) plus updated BAL spec.
6. **Issues:** none documented in the devnet-2 note itself. (Erigon lacked 7778/7843/8024 at launch — implementation gap, not an on-chain incident.)

### bal-devnet-3
1. **Genesis:** 2026-04-08 14:30:00 UTC (note target: 08 April 2026). Gas limit later bumped: commits "devnet-3: update gas limit to 120M" (2026-04-21), "update to 100M gas" (2026-04-23).
2. **EL EIPs:** EIP-7708 (🆙), 7778, 7843, 7928 (🆙), 7954 (🆕), 7975 eth/70 (🆕/optional), 8024 (🆙), **8037 (🆕, "static value 1174"** — "hardcoded value for the cost per state byte of 1174. This aligns with a block gas limit of 100M … treated as a fork constant for this devnet"), 8159 eth/71 (🆕/optional). EIP PRs included: #11234 (7928 cap max items), #11306 (8024 branchless normalization + EXCHANGE extension), #11311 (7708 ETH burn logs).
3. **Test pins:** EEST `bal@v5.6.0`; Consensus specs `v1.6.1` (unchanged).
4. **Delta vs devnet-2:** +7954, +7975 (optional), +8037 (static cpsb=1174), +8159 (optional); updates to 7708/7928/8024; EEST `bal@v5.1.0 → bal@v5.6.0`.
5. **Reason:** First devnet with the state-creation repricing (EIP-8037) and the new networking EIPs; also first with Ethrex participating.
6. **Issues found ON this devnet** (documented retrospectively in the bal-devnet-4/5/6 notes):
   - **EL spec ambiguity → client split:** "Spillover classification | Ambiguous (**bal-devnet-3 client split root cause**)" — fixed by EIPs#11522 (merged 2026-04-14). This was a genuine chain split caused by under-specified EIP-8037 spillover semantics.
   - Client implementation bugs (EL): Nethermind — tx gas-limit validated against 1D `header.GasUsed`, artificially shrinking budget; Besu — reservoir consumed on top-level exceptional halt after child spill-restore (delta `112 × cpsb`, fixed in `f4c8b36`); Geth — nested CREATE failure incorrectly restores `GAS_NEW_ACCOUNT` to parent reservoir; Erigon — 2D gas-pool bug. Besu also OOM'd (infra commit "devnet-3: rotate besu OOM heap dumps").

### bal-devnet-4 — prepared, never launched
1. **Genesis:** none. Prepared with genesis 2026-04-27 15:00 UTC, then renamed to devnet-5 (commit 2026-04-28 "rename bal-devnet-4 -> bal-devnet-5"). No `network-configs/devnet-4` in the repo.
2. **Planned EL EIPs:** the devnet-3 set + EIP-7976 (🆕) + EIP-7981 (🆕), with EIP-8037 switching to **dynamic** `cost_per_state_byte` "derived from header gas limit (quantized via `CPSB_SIGNIFICANT_BITS=5`, `CPSB_OFFSET=9578`)", EIP-7708 extended to CREATE/CREATE2, EIP-7928 block access index `uint64 → uint32` (EIPs#11550), EIP-8159 empty BAL response `0xc0 → 0x80` (EIPs#11553).
3. **Test pins:** EEST `snøbal-devnet-4@v1.0.0` — "tagged 2026-04-21 on `devnets/bal/4` (commit `524b446`)"; also references `tests-bal@v5.7.0`; Consensus specs `v1.6.1`.
4. **Delta vs devnet-3:** as above plus the 7 EIP-8037 state-gas accounting decisions from the 2026-04-15 Glamsterdam Repricing Breakout #6 (top-level failure zeroes state gas — specs#2689/EIPs#11476; 0→x→0 SSTORE refund direct to reservoir — specs#2698; CREATE silent-failure refund — specs#2704; same-tx SELFDESTRUCT refund — specs#2707; CALL-to-selfdestructed no-change — specs#2646; 7702 `intrinsic_state_gas` stop-mutation — specs#2711; 2D per-tx inclusion check — specs#2703).
5. **Reason (stated):** "Primary focus of this devnet is **EIP-8037 stabilization**". It was superseded before launch: the dynamic-cpsb plan was dropped ("bal-devnet-4 keeps the static `cost_per_state_byte = 1174` … The changes to accounting at the frame boundry will land in glamsterdam-devnet-1" — an info box edited late in the note's life) and the prepared network was relaunched as devnet-5 with EIPs#11573 frame accounting included.
6. **Issues:** n/a (never ran).

### bal-devnet-5
1. **Genesis:** 2026-04-29 10:00:00 UTC.
2. **EL EIPs:** 7708 (incl. CREATE/CREATE2), 7778, 7843, 7928 (uint32 index), 7954, 7975 (optional), 8024, **8037 (static `cpsb=1174` + frame-boundary accounting per EIPs#11573)**, 8159 (optional, empty response `0x80`), 7976 (🆕), 7981 (🆕). Note: "**EIP-7976 and EIP-7981 must be implemented** by all EL clients … Both were added post-ACDT."
3. **Test pins:** EEST `snøbal-devnet-5@v1.0.0` with the caveat "Tests are WIP … which does not include the frame accounting"; Consensus specs `v1.6.1`.
4. **Delta vs devnet-3** (the note diffs against 3, since 4 never launched): +7976, +7981; EIP-8037 frame accounting changed per EIPs#11573 (state diff at frame boundary instead of frame return) while cpsb stays static 1174; 7708 CREATE/CREATE2 logs; 7928 uint32; 8159 `0x80`; the 7 state-gas decisions; spillover classification clarified (EIPs#11522). Devnet gas limit 100M.
5. **Reason:** launch of the devnet-4 scope after the last-minute EIP-8037 re-spec (frame accounting EIPs#11573, static cpsb) — literally the renamed devnet-4 network.
6. **Issues:** none documented in the note for the devnet itself; the bal-devnet-3 client regressions (Nethermind/Besu/Geth/Erigon above) were still pending re-verification. Short-lived: devnet-6 genesis came ~44h later.

### bal-devnet-6
1. **Genesis:** 2026-05-01 06:00:00 UTC (target "Friday 01.05").
2. **EL EIPs:** identical set to devnet-5: 7708 (CREATE/CREATE2), 7778, 7843, 7928 (uint32), 7954, 7975 (optional), 8024, 8037 ("keeps the **static** `cost_per_state_byte = 1174` from bal-devnet-3. We will use opcode level accounting."), 8159 (optional, `0x80`), 7976, 7981.
3. **Test pins:** EEST `snøbal-devnet-6@v1.0.0` — "tagged 2026-04-21 on `devnets/bal/4` (commit `524b446`)"; `tests-bal@v5.7.0` referenced for the state-gas changes; Consensus specs `v1.6.1`.
4. **Delta vs devnet-5:** EIP-8037 accounting methodology switched again — **opcode-level accounting** instead of the frame-boundary variant that devnet-5 launched with (same EIP set otherwise). Open spec gap flagged: "change 2 ↔ 4 nested-child-frame refund interaction is implemented in tests via specs#2733; canonical EIP-8037 text still needs a clarification PR."
5. **Reason (stated):** "Primary focus of this devnet is **EIP-8037 stabilization**" — a rapid relaunch two days after devnet-5 to settle the accounting model.
6. **Issues found ON/around this devnet:** bugs reported by Dragan (EF testing) "in `#el-testing` on 2026-05-04 against `snobal-devnet-6@v1.1.x`": tx-CREATE halt/revert handling, EIP-7702 refund missing from `block.state_gas_used`, isolated `0→x→0` SSTORE refund, CREATE address-collision gas classification — all **EL spec/test bugs**, closed by EIP-8037 changes 3–8 in devnet-7. Repo commit "switch to lighthouse for nethermind" (2026-05-06) suggests a CL pairing swap (operational).

### bal-devnet-7
1. **Genesis:** 2026-05-19 13:05:00 UTC (target Monday 18.05, "subject to ACDT"). Geth was dropped from the initial node set at launch (commit "devnet-7: drop geth, repack validators, push genesis/gloas") — client-readiness, layer: EL client.
2. **EL EIPs:** 7708, 7778, 7843, 7928, 7954, **7975 (required)**, 7976, 7981, 8024, **8037 (🆙: "CPSB=1530, system-call gas bump, halt/revert + SELFDESTRUCT refunds")**, **8159 (required)**. EIP-8037 parameters verbatim: "`cost_per_state_byte` **`1174 → 1530`** (fixed), `STATE_BYTES_PER_NEW_ACCOUNT` `112 → 120`, `STATE_BYTES_PER_STORAGE_SET` `32 → 64`, `STATE_BYTES_PER_AUTH_BASE` unchanged at `23`. `SYSTEM_CALL_GAS_LIMIT` raised from `30M` to `30M + STATE_BYTES_PER_STORAGE_SET × CPSB × SYSTEM_MAX_SSTORES_PER_CALL` (`SYSTEM_MAX_SSTORES_PER_CALL = 16`)". EIP PRs: #11573 (merged), #11606 (merged), #11611 (open), #11616 (open). Execution-apis **#794 required** (uint32 `BlockAccessIndex`, `blockAccessListHash` header field, `debug_getRawBlockAccessList`, error `-32001`).
3. **Test pins:** EELS branch `devnets/bal/7`; EEST `tests-bal@v7.2.0` (spec changes confirmed in `tests-bal@v7.0.0`); Consensus specs `v1.6.1`.
4. **Delta vs devnet-6:** EIP-8037 recalibration (CPSB 1530, byte constants) + 8 spec changes (specs#2827, commit `7b3e8016`, #2815, #2816, #2823, #2828); eth/70 and eth/71 **optional → mandatory** (decided post-SFI check in #block-access-lists, 2026-05-08); reference block gas limit **96M → 150M**; target state growth 100 → 120 GiB/yr.
5. **Reason (stated):** ":checkered_flag: **Last `bal-` prefixed devnet.** Consolidates the bal-devnet-3 optimisations with the EIP-8037 spec changes finalised since bal-devnet-6."
6. **Issues:** changes 3–8 "directly close the bugs Dragan reported" from devnet-6 testing (EL spec). No on-chain split documented for devnet-7 itself in the note.

---

## Glamsterdam devnet series (CL Gloas/ePBS + BAL EL set)

### glamsterdam-devnet-0
1. **Genesis:** 2026-04-29 02:07:42 UTC (targeted 24 April 2026). Launch was rocky — commits "new launch" (00:15), "fix the mixed up vali network" (01:52), "re-re-relaunch" (02:00).
2. **EL EIPs:** 7708, **7732 ePBS (🆙, CL-driven)**, 7778, 7843, 7928 (🆙), 7954, 7975 (optional), 7976 ("🆕 - maybe"), 7981 ("🆕 - maybe"), 8024, 8037 (🆙 — "Will stay static" cpsb, strikethrough over dynamic plan), 8159 (optional). EIP PR pinned: EIPs#11476 (8037 top-level refund, merged). Engine: execution-apis #786 merged.
3. **Test pins:** Consensus specs `v1.7.0-alpha.5`; EEST "`bal@v5.6.1` or `bal@v5.7.0` pending release :exclamation: pending devnet3 or 4 scoping". No EELS branch pin.
4. **Delta vs bal series (its EL baseline is ~bal-devnet-3/4):** adds EIP-7732 (ePBS/Gloas CL) — the defining feature; consensus specs jump to `v1.7.0-alpha.5` with ~10 Gloas PRs (builder onboarding #4817, bid DoS #4831, 7843-in-Gloas #4840, deferred payload processing #5094, etc.).
5. **Reason:** first Gloas/ePBS-focused devnet ("advance builder separation"), running the BAL EL feature set underneath. EL client support for 7732 was ❌ across the board — this series was CL-driven (Kurtosis matrix pairs many CLs against a single EL).
6. **Issues:** multiple immediate relaunches at genesis (validator network misconfiguration — infra); the network was replaced by devnet-1 within ~20 hours (see below). No specific EL bug documented.

### glamsterdam-devnet-1
1. **Genesis:** 2026-04-29 22:35:00 UTC (recovered from deleted `network-configs/devnet-1/metadata/config.yaml`, `MIN_GENESIS_TIME: 1777502100`).
2. **EL EIPs / pins:** note verbatim: "This devnet used the same spec as glamsterdam-devnet-0." Same consensus `v1.7.0-alpha.5`, same EEST reference.
3. **Test pins:** inherited from devnet-0.
4. **Delta:** none (spec-identical; new chain id 7078355962).
5. **Reason:** relaunch of devnet-0 — launched <21h after devnet-0's troubled genesis. The notes do not state the precise failure; the git record (devnet-0 "re-re-relaunch"/"fix the mixed up vali network") indicates devnet-0 was unhealthy from genesis (infra/validator config, plus general Gloas instability). **Explicit reason not documented.**
6. **Issues:** not documented; devnet-2 followed ~2 days later with a new CL spec, implying devnet-1 served as the alpha.5 interop chain until superseded.

### glamsterdam-devnet-2
1. **Genesis:** 2026-05-01 12:59:00 UTC (targeted 30 April).
2. **EL EIPs:** 7708, 7732 (🆙), 7778, 7843, 7928 (🆙), 7954, 7975 (optional), 7976, 7981, 8024, 8037 (🆙), **8061 (🆕, CL churn)**, 8159 (optional). EIP PR: #11476 (merged).
3. **Test pins:** Consensus specs `v1.7.0-alpha.7`; EEST "`bal@v5.6.1` or `bal@v5.7.0` pending release … pending devnet3 or 4 scoping". Kurtosis EL image at the time: `ethpandaops/geth:bal-devnet-6` — i.e. EL was effectively the bal-devnet-5/6 feature branch.
4. **Delta vs devnet-0/1:** CL bump alpha.5 → alpha.7 (10 consensus-breaking Gloas PRs: #4769, #5061, #5135, #5152, #5164, #5172 "Fix genesis state in Gloas", #5177 PTC_SIZE=16, #5190, #5191, #5196); +EIP-8061; 7976/7981 firmed up from "maybe" to included. EL-side: no new EL EIPs.
5. **Reason:** pick up the new consensus-specs alpha (Gloas fixes, EIP-8061); EL unchanged.
6. **Issues:** none documented in the note; superseded by spec-identical devnet-3 five days later (implying the chain degraded — see below).

### glamsterdam-devnet-3
1. **Genesis:** 2026-05-06 15:41:24 UTC.
2. **EL EIPs / pins:** note verbatim: "This devnet used the same spec as glamsterdam-devnet-2." Same `v1.7.0-alpha.7` / EEST refs. Same `DEPOSIT_CHAIN_ID: 7057084805` as devnet-2 (config copied).
3. **Test pins:** inherited.
4. **Delta:** none spec-wise; purely a fresh chain. Around this time client images were being bumped (commits "bump prysm img", "bump lighthouse", "more peers").
5. **Reason:** relaunch of devnet-2 with the same spec. **Explicit failure reason not documented in the notes**; pattern (same-spec relaunch + peer/image-bump commits) indicates the devnet-2 chain became unhealthy — most plausibly CL/ePBS-side instability, but the notes don't say.
6. **Issues:** not documented.

### glamsterdam-devnet-4
1. **Genesis:** 2026-05-22 09:59:00 UTC ("targets to launch in late May 2026"). README now marks it 🔴 (offline).
2. **EL EIPs:** 7708, 7732 (🆙), 7778, 7843, 7928 (🆙), 7954, **7975 (required)**, 7976, 7981, 8024, **8037 ("CPSB=1530, system-call gas bump, halt/revert + SELFDESTRUCT refunds", 🆙)**, 8061, **8159 (required)**. Engine API: **`targetGasLimit` on `PayloadAttributesV4`** — "CL now passes the per-validator target gas limit to the EL on `engine_forkchoiceUpdatedV4`, replacing the static EL-side flag" (execution-apis **#796 required**, also #778, #786, **#794 required**, #798; CL #5235/#5236).
3. **Test pins:** Consensus specs `v1.7.0-alpha.8`; EELS/EEST `tests-bal@v7.2.0` on branch `devnets/bal/7` (also cites `tests-bal@v7.1.1`).
4. **Delta:** vs bal-devnet-7 EL: "None — all EIP updates were already incorporated into bal-devnet-7"; new is the `targetGasLimit` engine field and CL alpha.7→alpha.8 (13 PRs incl. #5235/#5236). Vs glamsterdam-devnet-2/3: entire bal-devnet-7 EIP-8037 final shape (CPSB=1530 etc.), eth/70+eth/71 mandatory, gas limit 150M.
5. **Reason (stated):** ":checkered_flag: **First `glamsterdam-` prefixed CL+EL devnet** combining `consensus-specs` **v1.7.0-alpha.8** with the **bal-devnet-7** EL feature set, plus the new `targetGasLimit` engine API field." (i.e., first unified CL+EL Glamsterdam devnet.)
6. **Issues:** none documented in the note (clients were still :hammer: on 8037/targetGasLimit at authoring time).

### glamsterdam-devnet-5
1. **Genesis:** 2026-06-04 12:59:00 UTC; planned shutdown "shut off on 25th of June 2026" (which matches devnet-6 genesis day).
2. **EL EIPs:** devnet-4 set plus **EIP-8045 (🆕, CL)** and **EIP-8189 (🆕 optional, "snap/2 - BAL-Based State Healing")**; 7975/8159 required.
3. **Test pins:** Consensus specs `v1.7.0-alpha.10`; EELS/EEST `tests-bal@v7.2.0`, branch `devnets/bal/7`.
4. **Delta vs devnet-4:** CL alpha.8 → alpha.10 (PRs #5300, #5302, #5250, #5281, #5310, #5309, …); +8045, +8189 (optional snap/2); EL EIP set otherwise unchanged ("None — all EIP updates were already incorporated into bal-devnet-7"); by now all ELs ✅ on 8037 (CPSB=1530) and eth/71; `targetGasLimit` still :hammer:.
5. **Reason:** iterate the unified devnet on the newer consensus alpha; scheduled lifetime with planned replacement by devnet-6.
6. **Issues:** none documented.

### glamsterdam-devnet-6
1. **Genesis:** 2026-06-25 11:29:00 UTC (note: "targets to launch on the 26th June 2026"). Currently "On" per README.
2. **EL EIPs (biggest EL delta of the series):**
   - **New:** **EIP-2780** (reduce intrinsic tx gas, resource-based rework — EIPs#11645, EELS#3017), **EIP-7997** (Deterministic Factory Predeploy, switched to Arachnid factory — EIPs#11783, EELS#2944), **EIP-8038** (state-access gas cost update; requires 7904+8037 — EIPs#11802/#11696, EELS#2972), **EIP-8070** (eth/72 Sparse Blobpool, optional — EIPs#11444, EELS#2948 open), **EIP-8246** (Remove SELFDESTRUCT Burn — EIPs#11626, EELS#2842), **EIP-8282** (builder execution requests, ePBS; EIPs PR #11760 Draft, EELS#2990 draft; two system contracts: Builder deposit `0x03` at `0x0000884d2AA32eAa155F59A2f24eFa73D9008282`, Builder exit `0x04` at `0x000014574A74c805590AFF9499fc7A690f008282`). Also EIP-7610 now listed explicitly.
   - **Changed:** **EIP-7954 → 64 KiB** max code size (EIPs#11540, EELS#2987); **EIP-8037 refund model → source-based** (EIPs#11807 + #11759/#11715/#11706, EELS#2999, merged to `forks/amsterdam` via #2901); **EIP-7928** clarifications (EXTCODECOPY GAS_COPY #11718, CREATE inclusion #11717, no-op storage #11750, 7702 edge case #11699); "BAL × 7702 warming **still decided keeping status quo**" (EELS#2900 rejected in favor of status quo).
   - Carried: 7708, 7732, 7778, 7843, 7975, 7976, 7981, 8024, 8045, 8061, 8159.
3. **Test pins:** Consensus specs `v1.7.0-alpha.11`; EELS/EEST **`glamsterdam-devnet@v6.1.1`** (release `tests-glamsterdam-devnet@v6.1.1`).
4. **Delta vs devnet-5:** "devnet-6 adds the post-bal-devnet-7 EL work" — the New/Changed list above; devp2p #264 (eth/71) now Merged; CL alpha.10 → alpha.11 (ePBS PRs #5335–#5377 incl. EIP-8282 CL side #5359).
5. **Reason (stated):** land the full remaining Glamsterdam EL scope (repricing suite 2780/8038, 7997, 8246, 8282, 64 KiB code size, source-based 8037 refunds) — the "all the EIPs" phase.
6. **Issues:** none documented in the note yet (network currently running).

### glamsterdam-devnet-7 (planned)
1. **Launch:** "targets to launch on the middle of July 2026" (README: WIP; no genesis config yet).
2. **EL EIPs:** devnet-6 set plus **EIP-7688 (🆕, CL SSZ forward-compat)** and informational EIP-7904; EIP-8070 marked :sparkles: optional. EIP-8282 system contracts move: **new deposit address `0x0000bff46984e3725691fa540a8c7589300d8282`** (exit `0x000064d678505ad48f8ccb093bc65613800e8282` "same as devnet-6"). EIP PRs pinned: #11844 (move state-dependent charges to runtime, ✅), #11854 (7928 SSTORE access-cost check, ✅), #11891/#11906 (2780 clarifications, ✅), #11908 (7976 calldata floor in block-level gas accounting, ✅), #11902 open. EELS: #3064 (SSTORE access-cost check before read, ✅). sys-asm PRs #49/#50 (builder-deposit contract disable-switch, reduced TARGET/MAX per block).
3. **Test pins:** Consensus specs `v1.7.0-alpha.12`; EELS/EEST **`glamsterdam-devnet@v7.2.0`** (`tests-glamsterdam-devnet@v7.2.0`).
4. **Delta vs devnet-6:** +7688; EIP-8282 spec matured (new deposit contract address, sys-asm changes, CL PRs #5416 `BUILDER_WITHDRAWAL_PREFIX=0xB0`, #5420, #5426, #5436, #5439); 2780/7928/7976 clarification PRs; CL alpha.11 → alpha.12.
5. **Reason:** consolidation devnet on the near-final Glamsterdam scope with updated 8282 contracts and spec clarifications.
6. **Issues:** n/a (not launched at research time).

---

## Cross-cutting observations
- **EEST pin lineage:** `bal@v1.8.0` → `bal@v2.0.0` → `bal@v5.1.0` → `bal@v5.6.0` → (`bal@v5.6.1`/`bal@v5.7.0` for glam-0..3) → `snøbal-devnet-4/5/6@v1.0.0` (+`tests-bal@v5.7.0`) → `tests-bal@v7.0.0`/`v7.1.1`/`v7.2.0` (branch `devnets/bal/7`) → `glamsterdam-devnet@v6.1.1` → `glamsterdam-devnet@v7.2.0`.
- **The only documented on-devnet chain split** in the series is on **bal-devnet-3**, root-caused to an **EL spec ambiguity** (EIP-8037 spillover classification, fixed by EIPs#11522), with four distinct EL client implementation bugs found alongside it (Nethermind, Besu, Geth, Erigon).
- The rapid devnet-4→5→6 churn (Apr 27–May 1) was entirely driven by **EIP-8037 accounting model instability** (dynamic vs static CPSB, frame-boundary vs opcode-level accounting), not by client failures.
- glamsterdam-devnet-1 and -3 are **spec-identical relaunches** of devnet-0 and devnet-2 respectively; the notes deliberately contain nothing but a "same spec" pointer, and no explicit failure post-mortem is published for either.
