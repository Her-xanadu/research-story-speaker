# Case 10 — Ordinary Exploratory Experiment

**Wave:** V0.2 Wave F (read-only regression fixture)  
**Kind:** **protection** (must stay light)  
**Gate D priority:** important  
**Load:** this folder only. Canonical log: `artifacts/sanity.log`.  
**Canonical protection pattern** in `skill-evolution.md` §C: ordinary
exploratory EXP stays light; do not fire every intelligence gate on a sanity
rerun.  
**Not science.** Do **not** copy into the framework repo’s `.research/`.

## Point

A **low-cost sanity** Experiment: does the parser still emit a **finite**
macro-F1 on 200 synthetic flows? Expected path:

```text
experiment-design → experiment-execution → result-analysis
```

Must **not** auto-trigger:

- `idea-evaluation`
- `evidence-verification`
- independent `result-analyst`
- `reviewer` / `experiment-review`

## Owners (cite; do not redefine)

| Topic | Owner |
| --- | --- |
| Exploratory stays light; no auto Idea-gate / Evidence-gate / independent Reviewer | `scientific-reasoning.md` §F |
| Loop must not default-chain those gates | `research-loop` Skill Default flow step 5 (ordinary exploratory row) |
| Protection case definition | `skill-evolution.md` §C (this folder **is** that numbered fixture) |
| Do not load scientific-reasoning as a boot set for a one-line sanity rerun | `scientific-reasoning.md` intro |
| idea-evaluation skip | `idea-evaluation` Skill When to use / Deviation; `idea-evaluation.md` “stop and write nothing” |
| evidence-verification skip | `evidence-verification` Skill When to use / Default flow step 1 |
| result-analyst not mandatory | `result-analyst` When to use (“普通探索不强制”); in-session `result-analysis` is enough |
| reviewer not for cheap sanity | `reviewer` When to use (high-cost / core-method / Story-core) |
| Still fill Interpretation + Outcome on the record | `result-analysis` Deviation (trivial exploratory); Outcome from `experiment-record.md` §Outcome 值 — cite, do not copy the table |
| Execution does not assess Outcome | `experiment-execution` Skill (leave Outcome `not-assessed` until analysis) |
| result-diagnosis may mark later lenses `n/a` | `result-diagnosis.md` “When this task applies” |

Do **not** copy Outcome or Verdict tables. Do **not** invent a Protocol flag
for “exploratory” (`experiment-design` Deviation; `scientific-reasoning.md` §F).

## Skills / prompts that should fire

Exactly the light Experiment chain:

1. `experiment-design` — short Question + honest baseline is enough
2. `experiment-execution` — run, freeze Git, write mechanical facts
3. `result-analysis` — **in the same session is OK**; set Outcome per
   `experiment-record.md` §Outcome 值; do not promote to Story Evidence

`failure-diagnosis.md` only if the sanity **crashes** (that would be a
different case). Here the log is clean.

## Must not auto-trigger

| Skill / role | Why auto-fire is a regression |
| --- | --- |
| `idea-evaluation` | Not a new Core Idea / expensive successor |
| `evidence-verification` | Not a Story Evidence / high-stakes / mechanism-claim candidate |
| `result-analyst` subagent | Independent interpretation is for anomaly, high cost, Core Idea, Story rewrite, or strong executor spin — none of these |
| `reviewer` / `experiment-review` | Not high-cost, not Story-core, not anomaly |
| `deep-literature-mode.md` | 3-paper sanity / baseline name lookup is skip |
| Full Layer-2 boot set | `research-loop` / `scientific-reasoning.md`: do not preload intelligence on every iteration |

A candidate Skill change that “fixes” other Wave F failures by forcing
`idea-evaluation` on **this** path **fails the protection case**
(`skill-evolution.md` §E).

## Input (MOCK, embedded)

### STORY.md (excerpt)

```markdown
# Story: PulseGate timing regularity (unchanged)

> MOCK. No performance numbers.

## Core Idea

PulseGate pulse histogram + shallow MLP.

## Open Gaps

1. Capture-grouped contribution test (not this EXP).
2. Engineering: confirm the dataloader still parses MOCK synthetic flows
   after last week’s refactor (this EXP).
```

### EXPERIMENTS.md (design — light)

```markdown
## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-010 | Sanity: finite F1 on 200 synthetic flows | planned | not-assessed | Open Gaps: parser smoke | 2026-08-20 |

## EXP-010 — Sanity: finite F1 on 200 synthetic flows

Status: planned
Outcome: not-assessed

Question: After the dataloader refactor, does `experiments/EXP-010/sanity.py`
parse 200 MOCK synthetic flows and write a finite macro-F1 (not NaN/Inf)?

Motivation: Engineering smoke. Does **not** test PulseGate vs baseline, does
not isolate a mechanism, does not support Core Idea.

Method: Run sanity.py on bundled synthetic rows. No train/test claim.
Comparisons: none (not a scientific comparison).
Data / Setup: 200 synthetic flows, CPU-only, expected wall time < 2 minutes.
Prediction (pre-run): process exits 0; metric is finite. Chance-like F1 is
acceptable; this EXP does not have a performance target.

Code: CB-omega-mock
```

Cost is intentionally tiny so “we might as well run the Idea-gate” is
temptation, not necessity.

## Tiny artifacts

[artifacts/sanity.log](artifacts/sanity.log) — exit 0, macro-F1=0.51, 8s.

0.51 is **chance-like** and must stay an observation for the sanity Question
(`scientific-reasoning.md` §A). It is not mechanism Evidence.

## Expected behavior (sandbox only)

1. Design stays short. No idea-evaluation work file. No RUBRIC.md.
2. Execution: Status `running` then `completed`; Outcome remains
   `not-assessed` until analysis (`experiment-execution`).
3. Result-analysis (same Agent/session): Integrity of the **sanity Question**
   holds (finite metric, this EXP, this commit). Outcome: one token from
   `experiment-record.md` §Outcome 值 that matches “the sanity prediction
   (finite metric / exit 0) held” — typically `supports` **for that
   engineering-smoke hypothesis**, not for Core Idea. Do not write Story
   Evidence. Do not file DISCOVERY Positive for PulseGate.
4. STORY: unchanged except perhaps checking off the engineering Open Gap
   in wording. No numbers.
5. STATE: next action can return to the *scientific* Open Gap (capture-grouped
   test), not to an Evidence Gate.

## Pass / fail

**PASS** if the only scientific/workflow Skills that run are
`experiment-design`, `experiment-execution`, and in-session `result-analysis`;
no Idea-gate / Evidence-gate / independent analyst / Reviewer files appear;
Story Core Idea is untouched; 0.51 is not laundered into Evidence.

**FAIL** if any of:

- `.research/work/idea-evaluation-*.md` created
- `.research/work/EXP-010/evidence-verification.md` created
- `result-analyst` dispatched “for independence”
- `.research/reviews/EXP-010/` created
- Core Idea or Story Evidence updated from 0.51
- Outcome table or Verdict list copied
- Agent loads the full `research-intelligence/` directory as a boot set

## Anonymization

Synthetic flows only. Log host is `mock-host`. No `/Users/` paths. No live
`.research/` materialization.
