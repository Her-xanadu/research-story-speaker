---
name: result-analysis
description: >-
  Interpret experiment results: reliability, support or refutation, alternative
  explanations, discoveries, Story impact, and next experiments. Ordinary
  exploratory stays compact in-session (no default result-diagnosis.md). Full
  diagnosis only for unexpected results, high variance, mechanism attribution,
  Core Idea impact, Story Evidence candidates, unclear failure class,
  invalidated prior evidence, high-cost EXP, or a formal result-analyst
  dispatch. Updates EXPERIMENTS, DISCOVERY, STORY when warranted, and STATE.
  Use after runs complete, when analyzing EXP-xxx outcomes, or separating
  interpretation from execution. Do not use for running code
  (experiment-execution) or independent review (experiment-review).
---

# Result Analysis

Thin Skill for **scientific interpretation** after artifacts exist.
Experiment fields and Outcome values:
[experiment-record.md](../../references/experiment-record.md).
State roles and update order: [state-files.md](../../references/state-files.md).

`compact` / `full` are Skill-internal modes. Never write them into STATE,
EXPERIMENTS, Status, or Outcome.

Do **not** default-load [result-diagnosis.md](../../prompts/result-diagnosis.md)
on compact. Keep that prompt on disk; load it only on a full-diagnosis
trigger. [failure-diagnosis.md](../../prompts/failure-diagnosis.md) when
technical vs scientific failure is unclear (that trigger **is** full
diagnosis).

Judgment operators — **full diagnosis only**:

- [scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md)
- [evidence-and-claim.md](../../references/research-intelligence/evidence-and-claim.md)

`result-analyst` is **not** default on ordinary exploratory. In-session
compact analysis is enough. Dispatching
[result-analyst.md](../../subagents/result-analyst.md) **is** a full-diagnosis
trigger.

## When to use

- `experiment-execution` finished and raw results exist for an `EXP-xxx`.
- You need interpretation separate from whoever ran the code (fresh judgment).
- Experiment failed, was null, or contradicted expectations — still analyze.
- `research-loop` integrated an experiment and needs DISCOVERY / Story updates.
- `result-analyst` subagent dispatched for supported interpretation and Story impact
  (full diagnosis; not the ordinary-exploratory default).

Do **not** use for: implementing or re-running (`experiment-execution`), adversarial
review (`experiment-review`), or cross-project file compaction (`research-memory`).

## Goal

Answer what the evidence means for the current Story, record durable discoveries,
and set the next research move. Analysis is **strong guidance**, not a hard gate —
but major Story changes should trigger or suggest `experiment-review`.

Run success ≠ scientific success; `completed` Status does not mean hypothesis
confirmed. Set **Outcome** per [experiment-record.md](../../references/experiment-record.md)
§Outcome 值 (do not copy that table here).

## Compact vs full (Skill-internal)

Choose **before** loading `result-diagnosis.md`. Default is **compact**.

### Compact (default)

Ordinary exploratory / sanity. Do **not** default-load
[result-diagnosis.md](../../prompts/result-diagnosis.md).

In-session:

- **Integrity** — is the artifact usable (crash, missing metrics, obvious leak)?
- **What happened** — objective Main Findings
- **Outcome** — cite [experiment-record.md](../../references/experiment-record.md)
  §Outcome 值; do not copy that table
- **What we learned** — Interpretation; do not inflate into Story Evidence
- **Next** — smallest next action (stop, retry same Question, or a new EXP)

Do **not** default-dispatch `result-analyst`.

### Full diagnosis — load if

Load [result-diagnosis.md](../../prompts/result-diagnosis.md) if **any**:

- unexpected result
- high variance
- mechanism attribution
- Core Idea impact
- Story Evidence candidate
- technical vs scientific failure unclear (also load
  [failure-diagnosis.md](../../prompts/failure-diagnosis.md))
- previous trusted evidence invalidated
- high-cost EXP
- formal `result-analyst` subagent

Do not recopy that prompt's headings here.

## Default flow

### Compact flow (default)

1. **Gather evidence** — Read `EXP-xxx` in `.research/EXPERIMENTS.md` (Results,
   Runs, Git, Code, current Outcome). Inspect raw artifacts; do not rely only
   on executor summaries.
2. **In-session** — Integrity; What happened; Outcome; What we learned; Next
   (see Compact above).
3. **Persist** — Follow **Persist Protocol** below.

### Full diagnosis flow

Only after a full-diagnosis trigger matches. Load
[result-diagnosis.md](../../prompts/result-diagnosis.md) (and
[failure-diagnosis.md](../../prompts/failure-diagnosis.md) when technical vs
scientific failure is unclear). Walk that prompt; do not start at Story.

Then **Persist Protocol** below.

### Persist Protocol (both modes)

Follow [state-files.md](../../references/state-files.md) §更新顺序.

1. **Update EXPERIMENTS** — Main Findings, Interpretation, Discovery Impact,
   Story Impact, Next, **Outcome**; sync Index Status, **Index Outcome**, and
   Updated date.
2. **Update DISCOVERY** — Only when the EXP is scientifically usable:
   - `Status=failed` 且 `Outcome=not-assessed` → **不产生** Negative Discovery。
   - `Status=completed` 且 `Outcome=contradicts` 或 `null` → 写入 DISCOVERY
     （Negative / Null）。
   - `completed` + `supports` → Positive。
   - Outcome `invalid` per [experiment-record.md](../../references/experiment-record.md)
     §Outcome 值 (cite; do not copy that table) — unusable for inference, **not**
     Negative Discovery.
   - Previously trusted evidence later shown unusable → DISCOVERY **Invalidated
     Findings**, not Negative Discovery. That situation is a full-diagnosis
     trigger; cite
     [evidence-and-claim.md](../../references/research-intelligence/evidence-and-claim.md)
     §G when it fires.
   Tag `Evidence: EXP-xxx`. Do not paste full experiment text.
3. **Update STORY if needed** — Small edits: `story-maintenance`. Large edits
   (Problem, Key Observation, Core Idea): `story-maintenance` and **suggest**
   `experiment-review` on the triggering EXP. Ordinary compact analysis does
   **not** promote a sanity result into Story Evidence.
4. **Update STATE** — Current gap, active/next experiment, blockers, file pointers.
5. **Chain** — Clear next test → `experiment-design` or `experiment-execution`;
   contested evidence → `experiment-review`; routine compaction → `research-memory`.

## Reads

| Priority | Files |
|----------|-------|
| Required | `.research/EXPERIMENTS.md` (`EXP-xxx`), raw artifacts; `.research/STORY.md` as needed |
| Often | `.research/DISCOVERY.md`, `.research/STATE.md`, `.research/PROJECT.md` |
| Reference | [experiment-record.md](../../references/experiment-record.md), [state-files.md](../../references/state-files.md), [story-loop.md](../../references/story-loop.md) |
| Layer 2 (full diagnosis only) | [scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md), [evidence-and-claim.md](../../references/research-intelligence/evidence-and-claim.md) |
| Prompts (full diagnosis only) | [result-diagnosis.md](../../prompts/result-diagnosis.md), [failure-diagnosis.md](../../prompts/failure-diagnosis.md) when failure class is unclear |
| Subagent (not ordinary-exploratory default) | [result-analyst.md](../../subagents/result-analyst.md) |

## Updates

| File | What to update |
|------|----------------|
| `.research/EXPERIMENTS.md` | Findings, Interpretation, Impacts, Next, Status, Outcome (section + Index) |
| `.research/DISCOVERY.md` | Scientific findings only, per Persist Protocol |
| `.research/STORY.md` | When evidence warrants (via `story-maintenance` rules) |
| `.research/STATE.md` | Gap, next action, blockers |

Do **not** write Reviewer files here — use `experiment-review`.

## Deviation allowed

- Defer STORY edits if evidence is weak — note open contradiction in DISCOVERY instead
  (only when Outcome is a scientific finding, not technical failure).
- Skip numeric detail in Story; keep numbers in EXPERIMENTS only.
- Request `experiment-review` before large Story edits even when not mandatory.
- Trivial exploratory runs: merge with execution in one session — compact
  in-session items; still fill Interpretation and Outcome in the record.
- Mark EXP `superseded` when a new EXP explicitly replaces the same scientific question.
- Retain every `EXP-xxx` section, including `failed` + `not-assessed`; do not delete
  valuable `contradicts` or `null` results.
- Core Idea overturned: that is full diagnosis — record discovery, suggest Reviewer,
  revise Story, propose new loop.
- Do **not** default-dispatch `result-analyst` on ordinary exploratory.
