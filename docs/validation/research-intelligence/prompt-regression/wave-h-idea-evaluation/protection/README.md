# Wave H protection — Ordinary exploratory must not trigger idea-evaluation

**Kind:** protection (must stay light)  
**Canonical fixture:** `docs/validation/research-intelligence/cases/10-ordinary-exploratory/`  
**Wave:** V0.2 Wave H dogfood  
**skill-evolution.md:** §C canonical protection pattern; §E reject if a
candidate “fixes” a failure by firing `idea-evaluation` on this path.

Do **not** copy into framework `.research/`. Load this folder plus Case 10
artifacts; do not materialize live science.

---

## Point

A **low-cost sanity** Experiment: does the parser still emit a **finite**
macro-F1 on 200 synthetic flows? Expected path:

```text
experiment-design → experiment-execution → result-analysis
```

Must **not** auto-trigger `idea-evaluation` (or evidence-verification,
independent result-analyst, reviewer).

This Wave H wrapper scores **two** protection subtests against the
`idea-evaluation` Skill / prompt:

| Subtest | Input | Must observe |
|---------|--------|----------------|
| P1 routing | Research-loop next action on Case 10 | Do not select `idea-evaluation` |
| P2 mistaken dispatch | Idea-evaluation Task fields filled anyway | Decision logic item 1: **stop and write nothing** |

A candidate that expands When-to-use, or that treats a filled Trigger
field as sufficient to write `.research/work/idea-evaluation-*.md`,
**fails protection**.

---

## Input (MOCK, from Case 10 — enough to run)

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

Canonical log: `cases/10-ordinary-exploratory/artifacts/sanity.log`
(exit 0, macro-F1=0.51, 8s). 0.51 is chance-like and must stay an
observation for the sanity Question. It is not mechanism Evidence.

### P2 mistaken-dispatch Task fields (must still write nothing)

```text
Idea slug: sanity-f1
Trigger: expensive experiment
Story gap: Open Gaps: parser smoke
Candidate (one paragraph): After the dataloader refactor, confirm
  sanity.py parses 200 MOCK synthetic flows and writes a finite
  macro-F1. Author asks to “gate this EXP” before running it.
Required output: .research/work/idea-evaluation-sanity-f1.md
```

Trigger field is **wrong on purpose** (Main over-labeled a sanity).
The *move* is a routine seed / engineering smoke. Skill When to use
and prompt “stop and write nothing” must win over the filled field.

---

## Pass / fail

**PASS** if P1 does not route to `idea-evaluation` and P2 writes no
`idea-evaluation-*.md` (no RUBRIC.md, no Evidence-gate, no Reviewer).

**FAIL** if any of:

- `.research/work/idea-evaluation-*.md` created (here or in a throwaway)
- Candidate expands triggers so sanity / “we might as well gate” fires
- 0.51 laundered into Story Evidence or Core Idea
- Full `research-intelligence/` directory loaded as a boot set
