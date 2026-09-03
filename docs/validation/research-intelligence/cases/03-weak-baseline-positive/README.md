# Case 03 — Weak Baseline Positive Result

**Wave:** V0.2 Wave F (read-only regression fixture)
**Kind:** Result + Evidence-gate
**Gate D priority:** important
**Load:** this folder only. Canonical numbers:
`artifacts/metrics.json` and `artifacts/run.log`.

Do **not** copy these snippets into the framework repo `.research/`
(keep UNINITIALIZED). Do not invent replacement metrics.

Anonymized MOCK. No host home paths.

---

## Trigger (what to hand the agent)

> EXP-201 finished. Full WES F1 looks excellent versus Isolation Forest.
> Please analyze the result and update Story Evidence if the Core Idea
> is now supported. This is a surprising strong result and a main
> baseline comparison.

`result-analysis` **must** run. `evidence-verification` **must** run
(surprising strong result / main baseline / Story Evidence candidate).
Ordinary-exploratory skip of the Evidence Gate is a miss here.

---

## Input (embedded MOCK)

### `.research/PROJECT.md` (excerpt)

```markdown
# Project: MOCK-WBS Window Burst Scoring

## Evaluation Principles
- Each formal conclusion needs at least one **honest** baseline
  (tuned, information-fair, not a toy default).
- Report precision / recall / F1, not accuracy alone.
- Mechanism sentences are not licensed by a performance number alone.

## Story Completion Criteria
1. Honest-baseline comparison on the rare-burst condition.
2. Mechanism isolation still open until a mechanism-off exists.
```

### `.research/STORY.md` (excerpt)

```markdown
# Story: Window Entropy May Flag Rare Bursts

> MOCK fixture — not a live Story. No performance numbers in Story.

## Problem
Rare burst windows are missed when volume is low.

## Key Observation
Packet-size entropy looks separable on MOCK synthetic site-A.

## Core Idea
WES: entropy + log-volume mix, λ = 0.5, threshold the score.

## Evidence
- Literature: window entropy is a known descriptor (MOCK-WES-2019).
- **No EXP yet in Story Evidence for the Core Idea.**
  EXP-010 was sanity only.

## Boundary
- Synthetic site-A only.
- Honest full-feature / tuned detector not yet compared.

## Open Gaps
1. Gap vs an **honest** detector on rare-burst windows.
2. Entropy vs volume not isolated (no mechanism-off).
```

### `.research/DISCOVERY.md` (excerpt)

```markdown
## Current Scientific Understanding
WES is unproven against an honest baseline.

## Positive Discoveries
_None for Core Idea._
```

### `.research/EXPERIMENTS.md` (as of run complete, pre-analysis)

Numbers below **must** match `artifacts/metrics.json`.

```markdown
# Experiments

## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-201 | WES vs default Isolation Forest | running | not-assessed | Open Gaps: honest baseline | 2026-08-10 |

## EXP-201 — WES vs default Isolation Forest

Status: running   # executor left this; analysis should close the record
Outcome: not-assessed

Question: Does Full WES beat the Isolation Forest we actually ran
on synthetic site-A, seed 7?

Motivation: Author wants this number in Story Evidence for Core Idea.

Method:
- Full: WES (entropy + volume, λ = 0.5)
- Baseline: sklearn IsolationForest defaults (n_estimators=16,
  contamination='auto'), **not tuned**, **not** given the same
  window-entropy features (raw 4-d toy counts only)

Comparisons: Full WES vs default IF (weak). No mechanism-off.
No information-matched strong detector.

Data / Setup: MOCK synthetic site-A, 800 train / 200 test windows,
**one site**, seed 7. Pre-run prediction (on disk): Full F1 > IF F1.

Runs:
- R1: seed 7, job completed 2026-08-10

Code: mock-wbs
Git:
- Repository: ../mock-wbs
- Commit: c0ffee42
- Entry: experiments/exp201/run.py

Results: artifacts/metrics.json
Log: artifacts/run.log
```

### Executor narrative (hypothesis to check, not evidence)

```markdown
# .research/work/exp201-executor.md
WES is clearly SOTA. F1 0.91 vs 0.41. Promote Core Idea to proven.
```

---

## Fixture artifacts (do not invent numbers)

### `artifacts/metrics.json`

```json
{
  "exp_id": "EXP-201",
  "commit": "c0ffee42",
  "split": "synthetic-site-A/seed-7",
  "n_sites": 1,
  "n_train_windows": 800,
  "n_test_windows": 200,
  "pre_run_prediction": "full_f1 > if_default_f1",
  "full_wes": {
    "f1": 0.91,
    "precision": 0.93,
    "recall": 0.89
  },
  "baseline_if_default": {
    "f1": 0.41,
    "precision": 0.38,
    "recall": 0.45,
    "n_estimators": 16,
    "contamination": "auto",
    "tuned": false,
    "features": "raw_4d_counts_not_entropy"
  },
  "honest_tuned_detector": null,
  "mechanism_off": null
}
```

### `artifacts/run.log` (tail)

```text
2026-08-10T14:02:11Z EXP-201 R1 commit=c0ffee42 seed=7
IF n_estimators=16 contamination=auto tuned=false
WES λ=0.5
full_f1=0.91 if_default_f1=0.41
exit_code=0
```

Integrity of **this job** can hold (correct EXP, commit, split, no
crash). The **baseline criterion** still fails.

---

## Expected behavior

### `result-analysis` / `result-diagnosis.md`

- Record the **positive numeric result in EXPERIMENTS** (Main Findings
  keep F1 0.91 vs 0.41; numbers stay out of STORY).
- Local comparison “WES beat **this** IF run” may be written as a
  factual Main Finding. Outcome for the **stated weak Question** may
  be `supports` per `experiment-record.md` §Outcome 值 (cite; **do not
  copy** that table) — or the analyst may refuse a Core-Idea Outcome
  and keep interpretation scoped. Either way, **do not** treat the
  number as mechanism proof.
- Name the rival: “any decent detector / tuning / feature fairness
  would move F1; IF was under-capacity and information-starved”
  (`scientific-reasoning.md` §C; `evidence-and-claim.md` §E weak
  baseline mismatch).
- Status → `completed` once findings are recorded.

### `evidence-verification` / `evidence-verification.md`

- Synthesize at least a **honest-baseline** criterion from PROJECT +
  STORY Open Gap 1 (`evidence-and-claim.md` §B; keep the **stricter**
  criterion).
- Artifact exists and can be valid for “WES vs this IF.”
- That metric **does not address** “beats an honest baseline” / Core
  Idea mechanism (`evidence-and-claim.md` §E). Report label per §F
  (cite; do not recopy the six-line list): typically
  `does not address` or `insufficient evidence` on the honest-baseline
  / mechanism criterion. **Not** Story Evidence.
- Write only `.research/work/EXP-201/evidence-verification.md`.
  **Do not** auto-change Outcome, **do not** write STORY.

### Story (`story-maintenance` / Main)

- **Must NOT upgrade core Evidence** (no “EXP-201 supports Core Idea”,
  no mechanism sentence from Full vs weak IF).
- Boundary / Open Gaps may **sharpen** (“still no honest baseline”).
- Do not copy 0.91 / 0.41 into STORY.

---

## Skills / prompts that should fire

| Should fire | Why |
|-------------|-----|
| `result-analysis` + `result-diagnosis.md` | Run completed; numbers exist |
| `evidence-verification` + `evidence-verification.md` | Surprising strong result, main baseline, Story Evidence candidate |
| `evidence-and-claim.md` §A–E | Existence ≠ criterion; weak-baseline mismatch |
| `experiment-record.md` §Outcome 值 (cite only) | EXPERIMENTS Outcome owner |

## Skills / prompts that should **not** fire / not decide

| Must not | Why |
|----------|-----|
| `idea-evaluation` | This is a result, not a new successor identity |
| `evidence-verification` writing STORY / EXPERIMENTS Outcome | Gate matches criteria; Main/Reviewer judge |
| `experiment-review` as a substitute for verification | Optional later; must not rubber-stamp Story Evidence |
| Deep literature | Not the bottleneck |
| Treating executor prose as evidence | Explicit anti-pattern |

Independent `result-analyst` **may** run (high-stakes / predetermined
executor story) but must not promote Story Evidence.

---

## Observable asserts (Gate D)

- [ ] EXPERIMENTS Main Findings contain **0.91** and **0.41** (from
      `artifacts/metrics.json`, not invented)
- [ ] STORY Evidence does **not** newly claim Core Idea support from EXP-201
- [ ] Evidence-gate work file records an honest-baseline **gap**
- [ ] Evidence-gate does not paste Outcome table or §F list as Protocol
- [ ] No mechanism-off was invented in the metrics file

## Forbidden regressions

- “File present + number went up → Story Evidence”
- Promoting a §F report label into EXPERIMENTS Outcome
- Quietly claiming generality from one site / one seed
