# Glamsterdam EL EIP review — dependency tree, ownership, and required changes

**Base commit (ethereum/EIPs `master`): `2e7e88bb8dc0ead4db7726ac6af89f576c221d93`**

All findings, line numbers, and links below refer to that commit. One item is already
addressed by [PR 11908](https://github.com/ethereum/EIPs/pull/11908) (branch
`eip-8037-updates-interaction`, commit `bcce075b`): the EIP-7623/7976 calldata floor was
missing from EIP-8037's two-dimensional *block* accounting, letting data-heavy
transactions contribute only their pre-floor gas to `block_regular_gas_used`. Everything
else is still open on master.

Scope — EL EIPs in [EIP-7773](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7773.md) not Declined for Inclusion:

- **Scheduled:** 7708, 7778, 7843, 7928, 7954, 7976, 7981, 8024, 8037
- **Considered:** 2780, 7904, 7997, 8038, 8246
- **Proposed:** 7610, 7979, 8163

(7732 is CL-dominant; 7688/8045/8061/8080 are CL; 7975/8070/8136/8159/8189 are networking — out of scope here.)

---

## 1. Principles

1. **`requires: Y` means Y is fully specifiable and implementable without knowledge of X.**
   An upper EIP may redefine what a lower EIP owns; a lower EIP must never normatively
   reference an upper one (informative "superseded by" notes are fine).
2. **Each subject has exactly one owner.** Everyone else references the owner's symbol,
   never restates its value or formula.
3. **Each EIP is a diff against the stack below it.** The topmost EIP of a stack (8037)
   restates the fully-composed formulas — that is what makes the fork implementable from
   the EIPs alone, without reverse-engineering tests.

## 2. Proposed dependency tree

The tree below shows the proposed in-fork `requires:` graph after cleanup (it is a DAG;
an EIP appearing more than once is the same node, marked "as above"). An edge means the
lower EIP must be readable and implementable first; the upper EIP is a diff against it.
Pre-fork dependencies (7623, 2930, 7702, 7825, 1014, …) are omitted except where they
explain an edge.

```text
Legend:  ──▶  hard `requires` edge (read/implement the target first)
         ─✳   value-only symbol binding, NOT a `requires` edge: the lower EIP
              charges a named gas-schedule symbol; 8037 (re)defines that
              symbol's value and gas dimension. This is what breaks the
              2780 ⇄ 8037 cycle.

8037  state-gas dimension: CPSB, reservoir, 2-D block accounting (integrator)
 ├──▶ 2780  intrinsic/runtime split, pre-execution phase
 │     ├──▶ 8038  regular-gas state-access schedule (COLD_ACCOUNT_ACCESS, …)
 │     ├──▶ 7976  calldata floor 64/64
 │     ├──▶ 7708  transfer logs (log shape priced as TRANSFER_LOG_COST)
 │     │     └──▶ 8246  SELFDESTRUCT burn removal
 │     │           └──▶ 7928  (records surviving balance-only accounts in BAL)
 │     ├──▶ 7928  block-level access lists (sender/recipient inclusion timing)
 │     └─✳  GAS_NEW_ACCOUNT, PER_AUTH_BASE_COST (state component)
 │          value := STATE_BYTES_* × CPSB, dimension := state gas,
 │          both defined by 8037's existing "Parameter changes" table
 ├──▶ 8038  (as above)
 ├──▶ 7981  access-list data surcharge
 │     └──▶ 7976  (as above)
 ├──▶ 7778  block accounting without refunds
 │     └──▶ 7976  (floor term, as above)
 └──▶ 7928  (as above: CREATE/CALL access timing; BAL gas-dimension note)

No in-fork requires (self-contained):
  7610  creation-collision rule (amends 684)
  7843  SLOTNUM  (engine-API/header coordination with 7928 via execution-apis)
  7954  code/initcode size limits  (informative note on 8037 code-deposit bound)
  7997  factory predeploy → 1014  (informative notes on 7610 and 8037)
  8024  DUPN/SWAPN/EXCHANGE
  8163  reserve EXTENSION 0xae
  7979  CALLSUB/ENTERSUB/RETURNSUB  (validator must track the fork's opcode set)
  7904  Informational, no changes — must appear in NO requires chain
```

Cycle check: all edges point downward in the listing order 7976/7928/8038/… → 7708/7981/7778
→ 2780 → 8037; 8246 → 7928 and 7708 → 8246 introduce no back-edges. The `requires:`
headers to change: 2780 drops 8037; 8038 drops 8037 and 7904; 8037 drops 7904 and adds
7778.

### Why the 2780 ⇄ 8037 cycle breaks with 2780 *below* — via symbol indirection

The goal is to untangle the circular `requires:` **without materially rewriting either
specification**. The trick is the one the gas schedule has always used: an EIP charges a
*named symbol* without owning its value, and a repricing EIP redefines the symbol. 2780
must keep charging the **correct** amount when an account is created — under Glamsterdam
that is `STATE_BYTES_PER_NEW_ACCOUNT × CPSB` (183,600), *not* a legacy fallback — it
just stops deriving that product inline.

- **2780 (below):** owns the structural change — decompose the flat 21,000, introduce
  the pre-execution phase, move state-dependent charges to runtime. Where it currently
  writes `STATE_BYTES_PER_NEW_ACCOUNT × CPSB` / `STATE_BYTES_PER_AUTH_BASE × CPSB` "in
  state gas", it instead charges the pre-existing schedule symbols `GAS_NEW_ACCOUNT` and
  `PER_AUTH_BASE_COST` (state component), whose value *and gas dimension* are whatever
  the active schedule defines. One informative sentence covers the binding: "Under
  [EIP-8037] these charges are `STATE_BYTES_* × CPSB`, metered in the state-gas
  dimension." The numeric reference tables (183,600 etc.) stay as informative examples
  under the Glamsterdam schedule. Test case 9's LIFO-refill mechanics are 8037 semantics
  and are referenced, not restated.
- **8037 (above):** needs almost no new text — its existing "Parameter changes" table
  already redefines exactly these symbols (`GAS_NEW_ACCOUNT`, `PER_EMPTY_ACCOUNT_COST`,
  `PER_AUTH_BASE_COST`) as `STATE_BYTES_* × CPSB` in the state-gas dimension. It keeps
  owning the reservoir, LIFO refills, and the two-dimensional block accounting, and its
  §Transaction validation section is the place where 2780's pre-execution phase is
  *extended* (gas split into `gas_left`/`state_gas_reservoir`) rather than mutually
  cited.
- The reverse ordering (8037 below) would force 8037 to define the state dimension
  against *legacy* intrinsic gas — which contains state-dependent components such as
  `PER_EMPTY_ACCOUNT_COST` — that 2780 would then delete and re-plumb: two rounds of
  churn, and exactly the interleaved-spec situation master has today.

Net effect: charged amounts are unchanged from today's combined intent, both documents
keep nearly all their current text, and the `requires:` arrow becomes one-directional
(2780 no longer requires 8037; 8037 keeps requiring 2780).

### Why the 8038 ⇄ 8037 cycle breaks with 8038 *below*

8038's only upward references are the state-gas column of its SSTORE table and the
`GAS_NEW_ACCOUNT` note. Move the combined regular+state SSTORE table into 8037 (which
already carries the state-only version) and leave 8038 a purely regular-gas repricing
with an informative pointer. 8038 then stands alone and could even ship without 8037.

## 3. Ownership map

| EIP | Owns (single source of truth) | Must stop owning / restating |
| --- | --- | --- |
| **7976** | Floor rate (64/64), `calldata_floor_gas_cost`, floor-reservation validity rule | Literal `21000`/`32000` in its formula — use named symbols that 2780/8037 redefine |
| **7981** | Access-list *data* surcharge + its inclusion in the floor | Frozen `2400`/`1900` per-entry values (8038 owns them); the name `TX_BASE_COST` for 21,000 |
| **7778** | Principle: block accounting uses pre-refund gas | The composed block formula with the floor (8037 owns the final composition) |
| **7708** | Transfer-log shape, emission points, ordering | Should state: opcode-level logs carry no new charge; tx-level log price is 2780's `TRANSFER_LOG_COST` |
| **7928** | BAL structure, recording/ordering, access timing, two-phase gas-validation framework | Concrete gas values (stay parametric); SELFDESTRUCT edge case needs 8246-aware rewrite (carried by 8246) |
| **8038** | All regular-gas state-access values (`COLD_ACCOUNT_ACCESS`, `ACCOUNT_WRITE`, `STORAGE_WRITE`, `COLD_STORAGE_ACCESS`, `WARM_ACCESS`, `STORAGE_CLEAR_REFUND`, `CREATE_ACCESS`, `ACCESS_LIST_*`), SSTORE regular-gas formula, refund rules, EXT* second read | State-gas column, any 8037 reference |
| **2780** | Intrinsic/runtime split, pre-execution phase order, `TX_BASE_COST`, `TX_VALUE_COST`, `TRANSFER_LOG_COST`, `REGULAR_PER_AUTH_BASE_COST` (move down from 8037), 7702 authorization-processing charge placement, the account **existence rule** (move down from 8037) | Deriving `STATE_BYTES_* × CPSB` inline — charge the schedule symbols `GAS_NEW_ACCOUNT` / `PER_AUTH_BASE_COST` instead (values + state-gas dimension defined by 8037); LIFO-refill mechanics (reference, don't restate) |
| **8037** | `CPSB`, `STATE_BYTES_*`, state-gas dimension, reservoir + LIFO refills, per-opcode state charges, 2-D block accounting incl. per-dimension floor, receipt semantics, system-call gas, **fully-composed final formulas** | `REGULAR_PER_AUTH_BASE_COST` (→2780), existence rule (→2780), stale 7904/intrinsic text |
| **8246** | SELFDESTRUCT finalization semantics | Should additionally carry its own BAL-recording rules (→7928) and a state-gas non-refill note (→8037) |
| **7843** | SLOTNUM opcode, `slotNumber` header field | Engine-API version numbers (collide with 7928) |
| **7954** | The two size limits | Needs informative note on the 8037 code-deposit bound (below) |
| **7610 / 8163** | Collision rule / opcode reservation | Fine as-is |
| **8024** | The three stack opcodes | Price symbolically (`GAS_VERYLOW`), not a literal 3 |
| **7979** | CALLSUB/ENTERSUB/RETURNSUB + MAGIC validation | Price symbolically (literal 8/5/1 today); validator-update procedure across forks unspecified |
| **7997** | The predeploy account + code | Silent on 7610 and on state-creation gas (below) |
| **7904** | Nothing normative (Informational) | Must be removed from every `requires:` and every "updated by 7904" sentence |

## 4. Findings — changes needed on master (`2e7e88bb`)

### A. Meta / scheduling (EIP-7773)

**1.** **EIP-7979 is listed under both Proposed and Declined** —
   [eip-7773.md#L57](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7773.md#L57)
   vs [eip-7773.md#L88](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7773.md#L88). Pick one.

**2.** **Status-tier inversions.** Scheduled EIPs require merely-Considered ones:
   [8037](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8037.md#L11) (SFI) requires 2780 + 8038 (CFI);
   [7708](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7708.md#L11) (SFI) requires 8246 (CFI).
   Either promote 2780/8038/8246 to SFI in 7773, or make the scheduled EIPs
   self-contained. As written, 8037 and 7708 cannot ship without their CFI dependencies.

### B. Dependency cycles

**3.** **2780 ⇄ 8037** —
   [eip-2780.md#L11](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-2780.md#L11) and
   [eip-8037.md#L11](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8037.md#L11) require each other.
   Fix via symbol indirection: 2780 charges the schedule symbols `GAS_NEW_ACCOUNT` /
   `PER_AUTH_BASE_COST` — whose Glamsterdam values (`STATE_BYTES_* × CPSB`) and state-gas
   dimension are defined by 8037's existing Parameter-changes table — and drops
   `requires: 8037`; 8037 keeps requiring 2780. Charged amounts are unchanged
   (183,600 for a new account), and neither spec is materially rewritten. See §2.

**4.** **8037 ⇄ 8038** —
   [eip-8038.md#L11](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8038.md#L11) requires 8037 back.
   Fix: move the combined SSTORE table
   ([eip-8038.md#L70-L82](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8038.md#L70-L82))
   and the `GAS_NEW_ACCOUNT` note
   ([eip-8038.md#L94](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8038.md#L94)) up into 8037.

**5.** **7904 is Informational with no changes** yet sits in the `requires:` of 8037 and 8038, and
   [eip-8037.md#L56](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8037.md#L56) still says
   `PRECOMPILE_ECRECOVER` "is updated by EIP-7904". The 7,816 derivation of
   `REGULAR_PER_AUTH_BASE_COST` only works with ecRecover at its current 3,000 — state 3,000 directly.

### C. Cross-document contradictions

**6.** **8037 contradicts 2780 (and itself) on where the account-creation charge lives.**
   [eip-8037.md#L394](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8037.md#L394) and
   [eip-8037.md#L433](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8037.md#L433) say 2780 adds it
   **to intrinsic gas**; 2780
   ([eip-2780.md#L115](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-2780.md#L115)) and 8037's own
   §Transaction validation
   ([eip-8037.md#L74-L81](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8037.md#L74-L81)) make it a
   **runtime** charge. Stale text from the previous 2780 revision.

**7.** **7976 hardcodes the legacy base its neighbors replace.**
   [eip-7976.md#L51-L63](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7976.md#L51-L63) uses literal
   `21000` and `isContractCreation * 32000`; 2780 replaces the base
   ([eip-2780.md#L143](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-2780.md#L143)) and 8037 replaces
   `GAS_CREATE`. The composed floor (7976 rate + 7981 surcharge + 2780 base) is never
   written normatively in one place, while 8037 consumes `calldata_floor_gas_cost`
   without saying which composition it means. The top of the stack (8037) must state the
   composed definition once; 7976 should switch to named symbols.

**8.** **Name collision on `TX_BASE_COST`.**
   [eip-7981.md#L104](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7981.md#L104) uses it for the
   legacy 21,000; [eip-2780.md#L39](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-2780.md#L39)
   defines it as 12,000. Also 7981's parameter table
   ([eip-7981.md#L26-L30](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7981.md#L26-L30)) freezes
   `2400`/`1900` per-entry costs that 8038 reprices to 3000/3000 — mark them as
   referenced values owned by the schedule, not parameters of 7981.

**9.** **Engine API collision between two Scheduled EIPs.**
   [eip-7843.md#L51](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7843.md#L51) and
   [eip-7928.md#L278-L292](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7928.md#L278-L292) both
   define `ExecutionPayloadV4` and `engine_getPayloadV6` with different, mutually unaware
   field sets. Both also add a header field (`slotNumber`, `block_access_list_hash`)
   and no document owns the combined Amsterdam header RLP layout/ordering.
   Recommendation: EIPs specify required fields only; version numbers and the combined
   header layout are assigned in execution-apis / the fork spec, referenced from 7773.

**10.** **7928's SELFDESTRUCT edge case encodes deletion semantics that 8246 removes.** [eip-7928.md#L259](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7928.md#L259) says destroyed accounts are included "without nonce or code changes"; under [8246](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8246.md#L37-L44) the account survives with nonce reset to 0, cleared code, retained balance — a state-reconstructing BAL must record those. 8246 (the lower-certainty EIP) should own its BAL recording rules. Likewise [eip-8037.md#L157](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-8037.md#L157)'s claim that a same-tx selfdestructed account "is not included in the state trie" becomes false under 8246 when balance remains (the no-refill outcome stays correct; the justification needs rewording, and the storage-cleared-at-finalization ⇒ no-refill case should be stated).

**11.** **7928 constants drift under 8038 / dimension ambiguity under 8037.** `ITEM_COST` rationale cites cold SLOAD at 2,100 ([eip-7928.md#L126](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7928.md#L126)) and the phantom-read invariant uses 1900+100 ([eip-7928.md#L587](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7928.md#L587)). Both invariants stay sound (minimum costs only rise), but the claims go stale, and under 8037's two dimensions nobody says which counter "remaining block gas" means (it must be the regular dimension — every BAL item costs ≥ 3,000 regular gas even when its dominant cost is state gas). This integration note belongs in 8037.

**12.** **7928's pre-state table charges `GAS_CREATE` "as defined in EIP-2929"** ([eip-7928.md#L152-L153](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7928.md#L152-L153)); under 8037/8038 that becomes `CREATE_ACCESS` (regular, pre-state) plus the conditional post-access state charge. 8037 handles the semantics — 7928 should stay parametric so it cannot contradict.

**13.** **7954 × 8037: the 64 KiB limit is unreachable at current gas limits.** Code-deposit state gas is `L × CPSB` = 65,536 × 1,530 ≈ 100.3M, so a 64 KiB deployment needs a block gas limit above ~101M; at 60M the effective cap is ~38 KiB. Needs an informative note in [eip-7954.md](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7954.md) (or 8037).

**14.** **7997 is silent on 7610 and on state-creation gas.** [eip-7997.md](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7997.md) does not say that pre-existing storage at the factory address blocks the ordinary-tx install path under 7610 (the irregular-state path bypasses it), nor that 8037 re-breaks keyless (Nick's-method) deployment gas limits — which is precisely the interaction 8037's own §Deterministic deployment factories describes. Cross-link the two; scheduling 7997 *with* 8037 resolves the regression 8037 introduces.

**15.** **7708's gas claim goes stale and its log pricing ownership is implicit.** [eip-7708.md#L76](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-7708.md#L76) says transfers cost ≥ 6,700 (legacy `CALL_VALUE − stipend`); under 8038 the surcharge is `ACCOUNT_WRITE` = 8,000. 7708 should also state explicitly that opcode-level transfer logs carry no new charge and that the transaction-level log is priced by 2780's `TRANSFER_LOG_COST` (= 1,756, [eip-2780.md#L41](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-2780.md#L41)).

### D. Editorial defects

**16.** **2780 Abstract sentence truncated** — [eip-2780.md#L16](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-2780.md#L16): "…while ETH transfers to new accounts and 7702-related transactions" (no predicate).

**17.** **Stray `=`** at the end of [eip-2780.md#L73](https://github.com/ethereum/EIPs/blob/2e7e88bb8dc0ead4db7726ac6af89f576c221d93/EIPS/eip-2780.md#L73).

**18.** **Literal opcode gas prices that repricing strands:** 8024's "Charge 3 gas" (intent is parity with `DUP*`/`SWAP*` — say `GAS_VERYLOW`), 7979's literal 8/5/1 (tier-keyed in prose but numeric in spec). 7979 additionally has TBD opcode bytes (0xB0–0xB2 placeholders) and an unspecified procedure for updating its canonical validator when a fork adds opcodes (8024, 8163, 7843) or deprecates them.

**19.** **Duplicate step numbering** in 8024's execution steps (two "5." steps per opcode).

## 5. Status of PR 11908 relative to this list

PR 11908 (commit `bcce075b`, one commit on top of `2e7e88bb`) fixes the calldata-floor
hole in 8037's block-level accounting: `tx_regular_gas` now takes
`max(…, calldata_floor_gas_cost)` in both the standalone and the EIP-7778-integrated
formulas, with rationale sections "Calldata floor in block accounting" and "State-gas
refills under EIP-7778". It does **not** address any of findings 1–19; notably findings
5, 6 (stale 7904/intrinsic text in 8037) touch the same file and could ride a follow-up
to the same branch or a separate PR.

## 6. Suggested PR breakdown

- **PR a (meta):** 7773 — resolve the 7979 double listing; promote or annotate the
  2780/8038/8246 status inversions (findings 1–2).
- **PR b (de-circularization):** 2780 + 8037 + 8038 — in 2780, replace the inline
  `STATE_BYTES_* × CPSB` products with the schedule symbols `GAS_NEW_ACCOUNT` /
  `PER_AUTH_BASE_COST` (values and dimension owned by 8037) and drop `requires: 8037`;
  move `REGULAR_PER_AUTH_BASE_COST` and the existence rule down into 2780; move the
  combined SSTORE table from 8038 into 8037 and drop 8038's `requires: 8037, 7904`;
  fix stale §Mispricing/7904 text; state the composed floor definition once in 8037
  (findings 3–7, 16–17).
- **PR c (peripheral integration):** 7928↔8246 recording rules; 7928/8037 BAL dimension
  note; 7708 constants + pricing ownership; 7954 note; 7997↔7610/8037 notes; escalate
  the 7843/7928 engine-API collision to execution-apis (findings 8–15, 18–19).
