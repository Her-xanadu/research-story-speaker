# Case 04 — Technical Failure

**Wave:** V0.2 Wave F (read-only regression fixture)
**Kind:** Engineering failure (not a scientific negative)
**Gate D priority:** important
**Load:** this folder only. Canonical log: `artifacts/run.log`.
There is **no** `metrics.json` (missing artifact is part of the case).

Do **not** copy these snippets into the framework repo `.research/`
(keep UNINITIALIZED). Do not invent a F1 or a “negative result.”

Anonymized MOCK. No host home paths.

---

## Trigger (what to hand the agent)

> EXP-202 R1 died. The job printed a traceback and wrote no metrics.
> Please close the experiment: this probably means PRRW is false.
> File a Negative Discovery and shrink the Story.

The agent must **reject** that scientific reading.

---

## Input (embedded MOCK)

### `.research/STORY.md` (excerpt)

```markdown
# Story: Port Rarity May Flag Rare Bursts

> MOCK fixture — Core Idea is PRRW after a prior Idea-gate (not this case).

## Problem
Rare-burst misses at low volume.

## Core Idea
PRRW: destination-port rarity rank vs previous K windows.

## Evidence
- Idea-gate only. No completed isolating EXP.

## Open Gaps
1. Does PRRW beat mechanism-off on the rare-burst condition?
```

### `.research/DISCOVERY.md` (excerpt)

```markdown
## Negative Discoveries
_None._

## Invalidated Findings
_None._
```

### `.research/EXPERIMENTS.md` (pre-diagnosis)

```markdown
# Experiments

## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-202 | PRRW vs mechanism-off (rare-burst) | running | not-assessed | Open Gaps: isolation | 2026-08-12 |

## EXP-202 — PRRW vs mechanism-off (rare-burst)

Status: running
Outcome: not-assessed

Question: On MOCK site-A rare-burst / low-volume windows, is
Full PRRW F1 − Mechanism-off F1 ≥ 0.08?

Pre-run prediction: Full > Mechanism-off by ≥ 0.08 F1.

Method: Full PRRW vs shuffled-rank mechanism-off vs WES, one split.

Runs:
- R1: seed 11, started 2026-08-12, **did not finish**

Code: mock-wbs
Git:
- Repository: ../mock-wbs
- Commit: bada1101
- Entry: experiments/exp202/run.py

Results: (missing — no metrics file)
Log: artifacts/run.log
```

---

## Fixture artifact (do not invent numbers)

### `artifacts/run.log`

```text
2026-08-12T03:11:04Z EXP-202 R1 start seed=11 commit=bada1101
host=mock-cpu-1
loading windows: n_train=800 n_test=200
Traceback (most recent call last):
  File "experiments/exp202/run.py", line 88, in <module>
    scores = model.fit_transform(X_train)
  File "mock_wbs/rank.py", line 41, in fit_transform
    raise RuntimeError("CUDA error: out of memory")
RuntimeError: CUDA error: out of memory
exit_code=1
metrics.json: MISSING
```

Use this log. Do not fabricate a completed F1. The OOM is
**engineering**; the scientific contract (Question, prediction,
mechanism-off) never produced a usable measurement.

---

## Expected behavior

Cite `.agents/references/experiment-record.md` for Status / Outcome
words. **Do not copy** the Outcome table (or the Status table) into
the work file or this analysis.

| Record field | Expected token | Owner |
|--------------|----------------|--------|
| Status | `failed` | `experiment-record.md` §Status 值 |
| Outcome | `not-assessed` | `experiment-record.md` §Outcome 值 |
| Failure class | `engineering` (environment/OOM is fine as a subtype) | `failure-diagnosis.md` Step 1 — cite; do not recopy the class list |
| DISCOVERY Negative Discoveries | **none** from this EXP | `result-analysis` rule: `failed` + `not-assessed` → no Negative Discovery |
| Story Evidence / Core Idea | **unchanged** (do not treat crash as refutation) | `result-diagnosis.md` §8 mapping; `evidence-and-claim.md` §D |

Typical move recommendation (`failure-diagnosis.md` Step 2 — these
strings are **not** Protocol Status): `bounded debug` or
`repair same EXP` (same Question, new commit). Not `pivot` from a
crash. Not a parameter sweep.

Main Findings may state **facts**: R1 OOM, exit 1, missing metrics.
Interpretation must not say “PRRW is false.”

---

## Skills / prompts that should fire

| Should fire | Why |
|-------------|-----|
| `experiment-execution` (if still closing the run record) | Mechanical facts, commit, log path |
| `result-analysis` + `result-diagnosis.md` | Close Status/Outcome; block Negative Discovery |
| `failure-diagnosis.md` | Engineering vs hypothesis is the live question |
| `experiment-record.md` (cite §Status 值 / §Outcome 值) | Token owner |

## Skills / prompts that should **not** fire / not conclude

| Must not | Why |
|----------|-----|
| Filing **Negative Discovery** | Technical failure ≠ scientific negative |
| Outcome `contradicts` | Unusable run; cite §Outcome 值, do not stretch `contradicts` |
| `idea-evaluation` | No new successor; do not ABANDON Core Idea from OOM |
| `evidence-verification` promoting a claim | Integrity fail; no Story Evidence. Ordinary skip is OK; if someone forces the gate, Satisfaction must not support the claim |
| `story-maintenance` shrinking Core Idea from this EXP | Crash is not Boundary evidence against PRRW |
| Deep literature | Irrelevant |

---

## Observable asserts (Gate D)

- [ ] Index + section: `Status: failed`
- [ ] Index + section: `Outcome: not-assessed`
- [ ] Failure typed as engineering (or environment/OOM under that)
- [ ] DISCOVERY has **no** new Negative Discovery tagged `EXP-202`
- [ ] No F1 invented for R1
- [ ] Work files do **not** paste the Outcome table from
      `experiment-record.md`

## Forbidden regressions

- Crash → `contradicts` → Negative Discovery → Story pivot
- Infinite debug that mutates the Question
- New EXP-ID for the same Question after a bugfix (repair keeps EXP-202)
