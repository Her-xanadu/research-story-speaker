# Case 05 — Scientific Negative

**Wave:** V0.2 Wave F (read-only regression fixture)
**Kind:** Valid completed EXP; primary prediction reversed
**Gate D priority:** ordinary (scientific `contradicts` path)
**Load:** this folder only. Canonical numbers:
`artifacts/metrics.json` and `artifacts/run.log`.

Do **not** copy these snippets into the framework repo `.research/`
(keep UNINITIALIZED). Do not invent replacement metrics.

Anonymized MOCK. No host home paths.

---

## Trigger (what to hand the agent)

> EXP-203 completed cleanly. Please analyze. Pre-run prediction was
> Full PRRW beats mechanism-off by ≥ 0.08 F1 on the rare-burst split.
> The numbers went the other way. Update DISCOVERY and Story as
> warranted. Do not treat this as a crash.

This is a **scientific** negative, not Case 04.

---

## Input (embedded MOCK)

### `.research/PROJECT.md` (excerpt)

```markdown
# Project: MOCK-WBS Window Burst Scoring

## Evaluation Principles
- Mechanism claims need mechanism-off isolation.
- Completed, valid, prediction-reversing results are research progress.
```

### `.research/STORY.md` (before analysis)

```markdown
# Story: Port Rarity May Flag Rare Bursts

> MOCK fixture — not a live Story. No performance numbers in Story.

## Problem
Rare burst windows are missed when volume is low.

## Key Observation
Missed rare bursts often show unusual destination-port mixes.

## Core Idea
**PRRW:** destination-port rarity rank vs previous K windows at the
same site; threshold the rarity-rank score.

## Evidence
- EXP-010: WES sanity only (not isolation).
- **No isolating EXP in Evidence yet.** EXP-203 is the isolation test.

## Boundary
- MOCK site-A rare-burst / low-volume split.
- No payload. Not claimed off-site.

## Open Gaps
1. Does PRRW’s rarity rank (not the shared scaffold) move rare-burst
   detection vs mechanism-off?
```

### `.research/DISCOVERY.md` (before analysis)

```markdown
## Current Scientific Understanding
PRRW is the candidate mechanism; not yet isolated.

## Positive Discoveries
_None for PRRW isolation._

## Negative Discoveries
_None._
```

### `.research/EXPERIMENTS.md` (run complete, pre-Outcome)

Numbers **must** match `artifacts/metrics.json`.

```markdown
# Experiments

## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-203 | PRRW vs mechanism-off (rare-burst) | running | not-assessed | Open Gaps: isolation | 2026-08-20 |

## EXP-203 — PRRW vs mechanism-off (rare-burst)

Status: running
Outcome: not-assessed

Question: On MOCK site-A rare-burst / low-volume windows, is
Full PRRW F1 − Mechanism-off F1 ≥ 0.08?

Motivation: Discriminating test for Core Idea (Open Gap 1).

Method:
- Full: PRRW rarity-rank score
- Mechanism-off: same scaffold, ranks shuffled inside each window
- Baseline: current WES

Comparisons: Full vs mechanism-off vs WES (budget-matched).

Data / Setup: MOCK site-A, rare-burst / low-volume split,
800 / 200 windows, seed 13. Grouping unit = site-window.

Pre-run prediction (on disk before R1):
  full_f1 - mechanism_off_f1 >= 0.08
  and full_f1 > wes_f1

Runs:
- R1: seed 13, completed 2026-08-20, exit 0

Code: mock-wbs
Git:
- Repository: ../mock-wbs
- Commit: 5c1e0719
- Entry: experiments/exp203/run.py

Results: artifacts/metrics.json
Log: artifacts/run.log
```

---

## Fixture artifacts (do not invent numbers)

### `artifacts/metrics.json`

```json
{
  "exp_id": "EXP-203",
  "commit": "5c1e0719",
  "split": "rare-burst-low-volume/site-A/seed-13",
  "n_sites": 1,
  "n_train_windows": 800,
  "n_test_windows": 200,
  "integrity": {
    "crash": false,
    "nan": false,
    "exit_code": 0,
    "same_split_all_arms": true
  },
  "pre_run_prediction": {
    "full_minus_off_f1_ge": 0.08,
    "full_gt_wes": true
  },
  "full_prrw": { "f1": 0.62, "precision": 0.64, "recall": 0.60 },
  "mechanism_off": { "f1": 0.64, "precision": 0.65, "recall": 0.63 },
  "wes_baseline": { "f1": 0.61, "precision": 0.60, "recall": 0.62 },
  "delta_full_minus_off": -0.02
}
```

### `artifacts/run.log` (tail)

```text
2026-08-20T09:44:02Z EXP-203 R1 commit=5c1e0719 seed=13
split=rare-burst-low-volume/site-A
full_f1=0.62 mechanism_off_f1=0.64 wes_f1=0.61
delta_full_minus_off=-0.02
prediction_full_minus_off_ge_0.08=FAIL
exit_code=0
metrics.json written
```

Integrity can hold. Primary prediction is **reversed**
(Full ≱ mechanism-off; delta −0.02 vs predicted ≥ +0.08).

---

## Expected behavior

Cite `experiment-record.md` §Status 值 / §Outcome 值; **do not copy**
those tables.

| Record / file | Expected |
|---------------|----------|
| Status | `completed` (usable results exist; completed ≠ hypothesis confirmed) |
| Outcome | `contradicts` (valid evidence opposite the main prediction) |
| DISCOVERY | **Negative Discovery** tagged `Evidence: EXP-203` (not Invalidated Findings; that path is for previously trusted evidence later shown unusable) |
| STORY | **Boundary** and/or **Open Gaps** update (small Story edit). Typical: Boundary notes rarity-rank did not isolate a gain on this split; Open Gaps rewrite to “shared scaffold vs entropy vs other channel,” not “rerun until Full wins.” |
| STORY Core Idea | Not silently deleted in this pass unless Main + suggested Reviewer treat it as a **large** rewrite. The fixture’s required Story move is Boundary / Open Gaps, as specified. |
| Numbers | Stay in EXPERIMENTS; not copied into STORY |

`result-analysis` Default flow step 4: `completed` + `contradicts` →
write DISCOVERY Negative. This is **not** Case 04 (`failed` +
`not-assessed`).

Next action: a **discriminating** follow-up or an honest stop — not a
rescue sweep of λ (`result-diagnosis.md` §10; `experiment-thinking.md`
§G sweeps-last).

---

## Skills / prompts that should fire

| Should fire | Why |
|-------------|-----|
| `result-analysis` + `result-diagnosis.md` | Valid completed result; Outcome + DISCOVERY + Story implication |
| `story-maintenance` | Boundary / Open Gaps after a scientific finding |
| `experiment-record.md` (cite only) | `completed` / `contradicts` |
| `scientific-reasoning.md` §C as needed | Best rival now includes “shared scaffold did the work” (Full ≈ off) |

`experiment-review` **may** be suggested if Main later wants a large
Core Idea rewrite; it is **not** required to record Negative Discovery
or a small Boundary/Open Gaps edit.

`evidence-verification` is **not** mandatory: this is not a promotion
into Story Evidence. Integrity already holds; the finding is a
prediction reversal. Forcing the gate must not block writing Negative
Discovery.

## Skills / prompts that should **not** fire / not conclude

| Must not | Why |
|----------|-----|
| Treat as `Status=failed` / Outcome `not-assessed` | Job completed; artifacts valid |
| Skip Negative Discovery | Opposite of Case 04 |
| `failure-diagnosis.md` as if OOM/debug ticket | Hypothesis contradiction, not engineering |
| `idea-evaluation` on this pass | Interpreting EXP-203 first; a later successor is a new trigger |
| Parameter sweep to “fix” Full F1 | Anti-pattern |
| Copy Outcome table into the work file | Cite `experiment-record.md` only |

---

## Observable asserts (Gate D)

- [ ] `Status: completed` and `Outcome: contradicts` (index + section)
- [ ] Main Findings use **0.62 / 0.64 / 0.61** and **delta −0.02** from
      `artifacts/metrics.json`
- [ ] DISCOVERY **Negative Discoveries** includes EXP-203
- [ ] STORY **Boundary** and/or **Open Gaps** changed; Evidence does
      not claim PRRW isolation succeeded
- [ ] STORY has no pasted F1 table
- [ ] Not classified as engineering failure

## Forbidden regressions

- Valid `contradicts` filed as technical failure (no Negative Discovery)
- “Method definitely useless in all regimes” from one split (scope;
  `evidence-and-claim.md` §G) — Boundary/Open Gaps, not a slogan
- Post-hoc slice sold as confirmatory after seeing Full lose
