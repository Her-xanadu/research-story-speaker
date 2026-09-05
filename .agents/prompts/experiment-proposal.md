# Experiment Proposal Prompt

This is a **design** prompt: specify a test that can change belief.
Canonical EXP fields, Status, and Outcome live only in
[experiment-record.md](../references/experiment-record.md) — cite those
sections; **do not redefine Status or Outcome** and do not copy those tables.
Git binding: [git-linking.md](../references/git-linking.md).

Main Agent persists a chosen design with the
[experiment-design](../skills/experiment-design/SKILL.md) Skill into
`EXPERIMENTS.md`. **This prompt does not write `EXPERIMENTS.md`.**

Judgment operators:

- [experiment-thinking.md](../references/research-intelligence/experiment-thinking.md)
- [scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)

Objects and rivals: scientific-reasoning.md. Units, controls, matrix, must /
nice / cut, cost: experiment-thinking.md. Mechanism identity and the smallest
Idea-level test, if this proposal follows an Idea-gate:
[idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
§H (cite; do not recopy the four-line glossary).

## Task fields

Main Agent fills in before dispatch. **Do not paste full state files.**

```text
EXP-ID: <EXP-NNN already reserved, or NEW for a design-only proposal>
Story gap: <one sentence — Open Gap or Boundary>
Confirmatory or exploratory: <per scientific-reasoning.md §F; not a Status>
Idea-gate file: <.research/work/idea-evaluation-<slug>.md or none>
Relevant files:
  - .research/STORY.md
  - .research/STATE.md
  - .research/DISCOVERY.md
  - .research/EXPERIMENTS.md → index + related sections
  - .research/RESOURCES.md
  - .research/PROJECT.md
  - .research/LITERATURE.md (if baseline / closest method is in play)
Required output: .research/work/experiment-proposal-<slug>.md
```

`EXP-ID: NEW` means design only. Do not edit code, create result directories,
or run anything. A concrete EXP-ID is assigned later by Main in EXPERIMENTS.md
([subagent-handoff.md](subagent-handoff.md)).

Skip this prompt when the Task is a one-line sanity rerun and EXPERIMENTS
already has the Question. Ordinary exploratory probes may use a short
Question + honest baseline; say so. Do not fire idea-evaluation or
evidence-verification because this file exists.

## When this applies

- New EXP, serious ablation, or a control that must isolate a mechanism
- After Idea-gate ADVANCE, to turn the minimum decisive test into a design
- Before high-cost compute, when the Question is not yet honest

Do **not** use to interpret finished results, assign Outcome, or update Story.

## Scientific context to read

Read listed paths from disk. Check EXPERIMENTS index and DISCOVERY
Invalidated / Negative so this proposal does not redo a negated route without
a new mechanism ([story-loop.md](../references/story-loop.md) §反重复).

Walk experiment-thinking.md A → H when this design must isolate a mechanism
or will spend serious compute. Persist mapping of design prose into existing
EXP section fields is experiment-thinking.md §A — Main/experiment-design
owns that write. Do not add index-table columns.

## Research reasoning lenses

Write **Question / Hypothesis / Rival / Prediction before** model, config,
seed, or compute (experiment-thinking.md §A). For this proposal: those four
must appear under the design headings below before any Compute line — unless
Task fields already mark the EXP explicitly exploratory
(scientific-reasoning.md §F). Exploratory vs confirmatory is a reasoning
distinction, not a new Status.

Keep scientific objects distinct (scientific-reasoning.md §A). If the model
line cannot be derived from Scientific Question, split it into a second
proposal.

Pick the **best rival** for this Question (scientific-reasoning.md §C), then
choose the **smallest control set** that would make that rival less tenable
(experiment-thinking.md §D). Do not chant every control kind. When the claim
kind is mechanism, Controls must include one isolating control from §D
(typically mechanism-off or information-matched); add budget or capacity
fairness only if those are the named rival.

Fix the **unit of analysis in the design** (experiment-thinking.md §B). Do
not leave a leaked split to be “fixed” after a pretty number.

## Method-First questions (required; no new schema)

Answer these **inside the existing headings below**. Do not add canonical
fields or index columns. If any of the five cannot be answered, **do not
propose** this Experiment ([experiment-record.md](../references/experiment-record.md)
§What deserves a new EXP-ID?):

1. **Current method hypothesis** — 我们当前相信哪个机制？
2. **Strongest rival** — 还有什么更简单解释？
3. **Discriminating observation** — 哪个结果能区分两者？
4. **Method consequence** — 各种结果分别会怎样改变方法？（keep / simplify / delete component / change mechanism / abandon）
5. **Smallest useful experiment** — 最小做到什么程度就足以做决定？

`What method decision will this experiment change?` must be answerable.
A seed-count matrix is not a reason. Prefer deletion (`Full` vs
`Full - Component A`) when the method is growing complex.

## Required design fields

The work file **must** contain all of the following headings, filled with
judgment — not empty labels. Teach operators under each heading; cite the
owner section instead of recopying its tables.

### Scientific Question

The thing we need to know next, tied to the named Story gap. Not a method
preference disguised as a question (scientific-reasoning.md §A–B). One
scientific question per proposal; do not smuggle a second claim kind into
the same EXP.

### Hypothesis

A testable claim about how the world behaves, stated so a result could go
the other way. A method slogan with no predicted pattern is not enough for
this heading.

### Rival Hypothesis

The best alternative account of the **same** future observation — simpler
mechanism, extra information, capacity, budget, leakage, capture/site
identity, shared scaffold, measurement. Name one primary rival; mention
others only if they could actually produce this pattern.

### Discriminating Prediction

What should happen *if* the hypothesis is true, written **before** seeing
results, and what should happen if the rival is true instead. If every
outcome can be narrated as support, stop and rewrite
(scientific-reasoning.md §D). This is not an Outcome value.

### Unit of Analysis

Name four units from experiment-thinking.md §B; they need not coincide:

```text
sample unit
experimental unit
analysis unit
grouping unit
```

State which grouping must not leak across train / test / folds — only
groupings that exist in *this* dataset (§B owns the kinds). Many rows that
are not independent given the grouping unit are not more evidence. If the
Story claims generality over a grouping, that unit belongs in the **split**.

### Primary Comparison

The discriminating comparison: target vs the best rival, on the failure
condition, at the experimental unit. This is must-run. Extra datasets and
seed sweeps are not the primary comparison unless the Question is about
those.

### Controls

List only the controls this Question needs. Pick the kinds from
experiment-thinking.md §D, driven by the named rival — cite §D; do not
recopy that taxonomy as a second owner. Do not treat an oracle cell as
deployable Story Evidence.

### Confounders

Design-time knobs that could move the metric without the claimed mechanism
(experiment-thinking.md §C). Pick from §C; do not recopy that list. Equalize
the knobs that are the **best rival**. Competing *explanations of a finished
result* stay in scientific-reasoning.md §C; do not paste that list here. If
the design only varies seeds while budget or split stay unequal, this
proposal is not yet a mechanism test.

### Expected Interpretation Matrix

Fill this heading with pattern → reading cells for the controls listed
under Must-run (experiment-thinking.md §E). Cite §E; do not recopy that
matrix as a second vocabulary. Every cell in *this* work file must split
hypothesis vs primary rival. Do not ship a matrix whose only story is that
Full should win. The matrix is design reasoning, not Outcome.

Split Must-run / Nice-to-have / Cut **before** the matrix grows
(experiment-thinking.md §F). The work file needs three separate headings.

### Must-run

The discriminating comparison (target vs the best rival). Put here the
comparison that can change the Question; it owns this proposal's compute
budget.

### Nice-to-have

Extra seeds, datasets, or ablations. Must not delay or starve Must-run.
Cheap extras in parallel are allowed only when they do not steal the
discriminating run.

### Cut

Sweeps that do not change the Question. Name the cells this proposal will
not run — cutting them is the successful design. If Must-run does not fit
RESOURCES, shrink this proposal's Question or send the Idea back. Leftover
Nice-to-have is not the EXP.

### Compute

Estimate data, steps, seeds, wall-clock, and Agent attention against
RESOURCES.md. Prefer low cost + high discrimination
(experiment-thinking.md §H). More runs ≠ better research. An underpowered
or leaked comparison does not count as evidence.

### Stop Condition

Write a stop line in this work file: when Must-run has enough to move the
target claim, remaining cells are Nice-to-have or Cut
([story-loop.md](../references/story-loop.md) gap priority).

## Critical questions

- Which claim kind is this EXP actually testing (scientific-reasoning.md §B)?
  Do not prop a mechanism claim with a performance number a weaker baseline
  could also produce.
- What result would *lower* belief?
- Are sample unit and grouping unit being confused?
- If Full wins, which control would still leave the rival standing?
- What is Cut, named explicitly?

## Fatal flaws / anti-patterns

- Model / seed / compute chosen before Question and rival
- Confirmatory language on an EXP that never stated a prediction
- All eight controls on a sanity probe (process failure, not rigor)
- Parameter sweep sold as ablation (experiment-thinking.md §G: if you cannot
  say what the test tests *and* what should happen if the component matters,
  you have a knob)
- Weight 1.0 → 0.9 called “mechanism-off”
- Duplicate mechanism-off as two EXP-IDs without a new Question
- Redoing an Invalidated route with no new mechanism
- Writing Status or Outcome in this work file as if inventing enums
- Treating this proposal as Story Evidence

## Decision logic

1. No rival, no prediction, and not explicitly exploratory → do not propose
   compute; rewrite Question or stop.
2. Grouping unit would leak in the planned split → fix the design; do not
   proceed.
3. Mechanism claim without mechanism-off or information-matched control →
   revise method before any run.
4. Must-run unaffordable → shrink Question or PARK the Idea; do not
   substitute an invalid cheap test.
5. Matrix cells compatible with both hypothesis and best rival → rewrite
   comparisons.
6. Honest Must-run specified → write the work file and stop. Main assigns
   EXP-ID (if NEW) and writes EXPERIMENTS.md via experiment-design.

## Required output

Write only:

```text
.research/work/experiment-proposal-<slug>.md
```

Include every required design heading above, plus:

```text
Story gap:
EXP-ID: <concrete id or NEW>
Exploratory or confirmatory: <reasoning label only>
Recommended persist: Main / experiment-design → EXPERIMENTS.md
```

Optional: Method sketch, Data/Setup, planned `experiments/EXP-xxx/` and
`results/EXP-xxx/` *paths* (git-linking.md). Do not invent commits or results.

## Handoff / state impact

- Writer (Main in-session, or `experiment-agent` with `EXP-ID: NEW`) writes
  `.research/work/` only.
- **Do not** edit `STORY.md`, `DISCOVERY.md`, `EXPERIMENTS.md`, or other
  canonical state from this prompt.
- Main / [experiment-design](../skills/experiment-design/SKILL.md) maps
  Question, Why, Comparison, Expected outcomes, Codebase, Compute into the
  existing EXP section (experiment-thinking.md §A persist mapping;
  experiment-record.md for field names). Index Status/Outcome on create:
  use experiment-record.md — do not restate those enums here.
- High-cost or mechanism-changing designs: Main may dispatch
  [method-review.md](method-review.md) after the EXP section exists.
  This proposal is not a Reviewer artifact and has no Verdict.

Return a concise summary: Question, primary comparison, must-run vs cut,
compute, stop condition. Do not polish the work file after writing it.

## Stop / escalation

- Question already on disk as a sanity rerun → stop; do not expand.
- Idea identity still a costume → send back to idea-evaluation; do not
  launder it into an EXP.
- Baseline choice needs literature → pair with literature-research; do not
  guess closest work.
- After writing the proposal, stop. Execution waits for a reserved EXP-ID
  and experiment-execution.
