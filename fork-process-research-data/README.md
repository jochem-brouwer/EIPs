# Fork-process research — raw data archive

Source material gathered 2026-07-14 for `fork-process-review.md` (Glamsterdam EL devnet review) and `glamsterdam-fork-process-research.md`. Kept so future research doesn't have to re-download it. All read-only snapshots; nothing here was published.

## Contents

- `devnet-notes/` — raw HackMD markdown of the ethpandaops devnet spec pages (`notes.ethereum.org/@ethpandaops/...`), fetched via the `/download` endpoint:
  - `bal-0.md` … `bal-7.md` — bal-devnet-0..7 (note: bal-devnet-4 was prepared but never launched)
  - `glam-0.md` … `glam-7.md` — glamsterdam-devnet-0..7 (devnet-7 pre-launch state as of 2026-07-14)
- `pm/` — full issue bodies + comments from `ethereum/pm`, named `<CALL>-<number>-issue<pm issue id>.md`. Covers ACDT #62–87 and ACDE #225–240 plus pre-scope issues (`PRE-` prefix).
- `acdt/` — raw JSON dumps of Fusaka-era ACDT/ACDE/ACDC issues (mid-2025, pm issues ~#1528–#1736) used for the cadence baseline.
- `agent-report-devnet-notes-and-configs.md` — per-devnet EL scope, exact spec/test pins (EEST tags, EIP PRs, engine-API PRs), verified genesis timestamps (from `MIN_GENESIS_TIME` in the ethpandaops config repos), deltas, stated launch reasons, documented issues.
- `agent-report-eels-eest-feedback-loop.md` — EEST/EELS release timeline (~40 tags), per-EIP implementation timing, testing-team→EIP-spec feedback PRs, devnet-found vs test/review-found attribution for the headline incidents.
- `agent-report-acdt-devnet-triggers.md` — per-devnet launch trigger categorization from ACDT/ACDE records, devnet-only findings vs pre-knowable issues, decision-process quotes (sunk-cost, devnet-inclusion≠SFI, capacity signals).
- `agent-report-fusaka-cadence-baseline.md` — Fusaka devnet ladder (spec-freeze devnet, testnet-gate devnet, "no plans for devnet-4"), rollout-timeline vs 6-month-cadence tension (ACDC #164, pm PR #1715).

## Not archived (re-derivable or too large)

- `ethereum/EIPs` commit history — it's this repo's own git log.
- Full `gh` PR listings for `ethereum/EIPs` / execution-specs — re-queryable; key PR numbers are cited inline in the reports and in `fork-process-review.md` §9.
- forkcast.org call summaries and the glamsterdam-devnet-6 Discord status thread — never fetched (flagged as gaps in the reports).
