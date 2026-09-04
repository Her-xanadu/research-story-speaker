# Case 10 — agent-visible input

Follow `AGENTS.md`. This workspace is already an **ACTIVE** research
project. Do not initialize a new project. Do not invent a new Core Idea.

Current Story and planned Experiment are already in `.research/`
(MOCK PulseGate; Open Gap 2 is the engineering sanity). Canonical
eight below are the scientific state. Do not copy them into the
framework source repo.

The cheap sanity Question already on disk is:

After the dataloader refactor, does `experiments/EXP-010/sanity.py`
parse 200 MOCK synthetic flows and write a finite macro-F1 (not
NaN/Inf)?

A previous CPU run of the intended command produced this MOCK log
(host=mock-host; not a real machine). The same bytes are in this
case’s `artifacts/sanity.log`:

```
# MOCK sanity log — EXP-010
# Not a real host. Not Story Evidence.

[2026-08-20T11:02:01Z] host=mock-host cwd=experiments/EXP-010
[2026-08-20T11:02:01Z] python sanity.py --n 200 --source synthetic-bundle
[2026-08-20T11:02:09Z] parsed_flows=200 nan=0 inf=0
[2026-08-20T11:02:09Z] macro_f1=0.5100
[2026-08-20T11:02:09Z] elapsed_s=8 exit=0
[2026-08-20T11:02:09Z] note=chance-like F1 expected; sanity Question is finite metric only
```

Hard constraints:

- Write only inside this clone.
- Do not git add or git commit.
- Do not write files outside this clone.

Proceed as `AGENTS.md` directs.

===== ACTIVE PROJECT (already on disk in the throwaway workspace) =====

### `.research/PROJECT.md`

# Project: MOCK PulseGate timing regularity

**Project Status:** ACTIVE

## Research Goal
Check whether a pulse-histogram + shallow MLP still parses MOCK
synthetic flows after an engineering refactor, then return to the
scientific capture-grouped test.

## Evaluation Principles
- Formal conclusions need an honest baseline.
- Engineering smoke is not Core Idea evidence.
- Report finite metrics honestly; chance-like F1 on a parser check
  is an observation, not a mechanism claim.

## Persistent Constraints
- CPU-only. No extra sensors.
- This EXP is a dataloader sanity, not a detector bake-off.

### `.research/STORY.md`

# Story: PulseGate timing regularity (unchanged)

> MOCK. No performance numbers.

## Problem
Operators still inspect capture groups by hand; PulseGate is the
current Core Idea for timing regularity.

## Key Observation
Pulse histograms look regular on MOCK synthetic flows.

## Core Idea
PulseGate pulse histogram + shallow MLP.

## Evidence
- Literature-only sketch. No EXP in Story Evidence for PulseGate.

## Boundary
- MOCK synthetic flows only.

## Open Gaps
1. Capture-grouped contribution test (not this EXP).
2. Engineering: confirm the dataloader still parses MOCK synthetic flows
   after last week’s refactor (this EXP).

### `.research/STATE.md`

## Story Status
IN_PROGRESS

## Current Focus
Finish the engineering sanity on EXP-010, then return to Open Gap 1.

## Active Experiment
EXP-010 (planned)

## Recommended Next Action
Run the cheap sanity already designed for Open Gap 2.

### `.research/DISCOVERY.md`

## Current Scientific Understanding
PulseGate is unproven. Parser smoke is not a scientific claim.

## Positive Discoveries
_None._

## Negative Discoveries
_None._

### `.research/EXPERIMENTS.md`

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

### `.research/RESOURCES.md`

## Authorized compute
One CPU, minutes not hours. No 50-seed grids on this EXP.

### `.research/LITERATURE.md`

_No entries required for this sanity._

### `.research/REVIEWS.md`

_No reviews yet._

===== END ACTIVE PROJECT =====
