---
name: experiment-design
description: >-
  Design a new EXP-xxx or refine a planned experiment from the current Story gap.
  Prioritize high-impact experiments that maximally change scientific judgment,
  not parameter sweeps. Document Question, Why, Comparison, Expected outcomes,
  Codebase, and Compute in EXPERIMENTS.md without running code. Use before
  experiment-execution or when planning EXP-002+. Triggers include 设计实验,
  design experiment, plan EXP.
---

# Experiment Design

Thin Skill for scientific experiment **specification** before code runs. Field
definitions, Status, and Outcome:
[experiment-record.md](../../references/experiment-record.md)
(cite §Status 值 / §Outcome 值; do **not** copy those tables).
Git layout and traceability: [git-linking.md](../../references/git-linking.md).
Gap priority: [story-loop.md](../../references/story-loop.md).

Judgment (cite; do not recopy taxonomies or Verdict):

- [scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md)
- [experiment-thinking.md](../../references/research-intelligence/experiment-thinking.md)

Optional work-file prompt:
[experiment-proposal.md](../../prompts/experiment-proposal.md).
Main still writes `EXPERIMENTS.md`.

## When to use

- `research-loop` or `result-analysis` identified a gap needing new evidence.
- `EXPERIMENTS.md` needs a new `EXP-NNN` or a `planned` section expanded.
- Before high-cost runs — design first; optionally `experiment-review` method pass.
- User asks to plan an ablation, baseline, confirmation, or falsification study.
- `literature-research` surfaced a comparison worth empirical test.

Do **not** use for: implementing or running code (`experiment-execution`),
interpreting finished results (`result-analysis`), literature-only work, or
updating Story belief (`story-maintenance`).

## Goal

Produce one experiment specification that **maximally changes current judgment** if
answered — not a parameter sweep. Per
[story-loop.md](../../references/story-loop.md): prefer experiments that could
change Problem, Core Idea, or whether to continue the route.

Record in `EXPERIMENTS.md` with index row, Status `planned`, Outcome `not-assessed`.

## Default flow

1. **Anchor gap** — Read `.research/STORY.md` (Open Gaps, Boundary),
   `.research/STATE.md`, relevant `DISCOVERY.md` (especially Invalidated /
   Negative), and `.research/PROJECT.md` constraints.
2. **Check duplicates** — Scan `EXPERIMENTS.md` index and DISCOVERY Invalidated;
   avoid redoing negated routes without new mechanism. One scientific question
   per EXP-ID; supersede when replaced.
3. **Assign EXP-ID** — Next sequential `EXP-NNN`; optional short title in heading.
4. **Draft core specification** — Fill these design fields (map to section fields
   in [experiment-record.md](../../references/experiment-record.md)):

   | Design field | Maps to EXPERIMENTS section | Content |
   | --- | --- | --- |
   | **Question** | Question | Precise scientific question |
   | **Hypothesis** | Question / Motivation | Testable claim in existing prose — not a new column |
   | **Rival** | Motivation / Comparisons | Best alternative of the same future observation |
   | **Prediction** | Expected outcomes (design note; else Motivation) | Pre-run pattern; not an Outcome value |
   | **Why** | Motivation | Link to Story gap; why this changes judgment |
   | **Unit of analysis** | Data / Setup | Units per experiment-thinking.md §B |
   | **Confounders** | Data / Setup | Design-time knobs to equalize (§C) |
   | **Controls** | Comparisons | Smallest set that isolates the named rival (§D) |
   | **Comparison** | Comparisons | Baselines, ablations, controls |
   | **Interpretation matrix** | Expected outcomes (design note) | Pattern → reading; not Outcome (§E) |
   | **Must / Nice / Cut** | Comparisons / Method | Must-run owns budget; Cut is success (§F) |
   | **Expected outcomes** | (design note) | What result would support / refute / narrow Story |
   | **Codebase** | Code | Codebase ID from `RESOURCES.md` |
   | **Compute** | Data / Setup | Dataset, seeds, metrics, estimated cost / runtime |

   Map into the existing EXP section. Do **not** invent new canonical files,
   index columns, or Protocol enums.

5. **Add method and setup** — Method summary, Data/Setup, planned Runs structure
   (local labels only; no global Run IDs).
6. **Plan traceability** — Planned Entry `experiments/EXP-xxx/` and planned Results
   root `results/EXP-xxx/` per [git-linking.md](../../references/git-linking.md).
   Existing baseline commits and planned result roots **may** be recorded now.
   Do **not** invent commits or results. Formal execution confirms final Git /
   Results values.
7. **Update EXPERIMENTS.md** — Add index row (`planned`, Outcome `not-assessed`,
   Story Gap phrase, Updated date) and full section; leave Main Findings /
   Interpretation empty.
8. **Update STATE** — Active experiment, recommended next (`experiment-execution`).
9. **Optional review** — Suggest `experiment-review` method-review before costly
   or mechanism-changing runs.

## Reads

| Priority | Files |
| --- | --- |
| Required | `.research/STORY.md`, `.research/EXPERIMENTS.md`, `.research/RESOURCES.md` |
| Often | `.research/STATE.md`, `.research/DISCOVERY.md`, `.research/LITERATURE.md` |
| Reference | [experiment-record.md](../../references/experiment-record.md), [git-linking.md](../../references/git-linking.md), [story-loop.md](../../references/story-loop.md) |
| Layer 2 | [scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md), [experiment-thinking.md](../../references/research-intelligence/experiment-thinking.md) |
| Prompt | [experiment-proposal.md](../../prompts/experiment-proposal.md) (optional; Main still writes EXPERIMENTS) |

## Updates

| File | What to update |
| --- | --- |
| `.research/EXPERIMENTS.md` | Index row + new `EXP-xxx` section (`planned`, Outcome `not-assessed`) |
| `.research/STATE.md` | Active EXP, next step, blockers |

Do **not** write Interpretation, `DISCOVERY.md`, or `STORY.md` here. Numbers and
metrics belong in EXPERIMENTS after runs, not in STORY
([state-files.md](../../references/state-files.md)).

## Deviation allowed

- Extend an existing `planned` EXP instead of creating a new ID when scope matches.
- Sketch multiple comparisons in one EXP when they share one scientific question.
- Explicitly exploratory EXP: short Question + honest baseline is enough; do
  not invent a Protocol flag (scientific-reasoning.md §F).
- Defer Runs detail until execution if setup is exploratory.
- Pair with `literature-research` when baseline choice is uncertain.
- Document Expected outcomes inline under Motivation when section has no dedicated field.
- Mark low-priority sweep as explicitly secondary — still tie to one judgment-changing question.
- Avoid standalone hyperparameter grids unless they test a stated mechanism.
- High-cost designs should note Compute estimate and Review recommendation.
- Do not fabricate Git commits or result artifacts; recording a real existing
  baseline SHA or a planned results directory is allowed.
