---
name: experiment-design
description: >-
  Design a new EXP-xxx or refine a planned experiment from the current Story gap.
  Prioritize high-impact experiments that maximally change scientific judgment,
  not parameter sweeps. Ordinary exploratory / sanity stays compact in-session
  (no default experiment-proposal.md or experiment-thinking A→H). Full design
  only for mechanism claim, new Core Idea, high compute, important ablation,
  isolating control, publication-facing comparison, or high-risk split.
  Document in EXPERIMENTS.md without running code. Use before
  experiment-execution or when planning EXP-002+. Triggers include 设计实验,
  design experiment, plan EXP.
---

# Experiment Design

Thin Skill for scientific experiment **specification** before code runs.

**Inner loop W2:** compact design does not re-open `story-loop` or re-frame the
Story. Only full design (mechanism claim, new Core Idea, etc.) may need W1 context.

`compact` / `full` are Skill-internal modes. Never write them into STATE,
EXPERIMENTS, Status, or Outcome.

## EXP eligibility (before any new ID)

Before minting a new `EXP-xxx`, the task must satisfy
[experiment-record.md](../../references/experiment-record.md)
§What deserves a new EXP-ID?: a scientific question that can change a
scientific decision, with at least two result-dependent judgments.

**Do not mint EXP** for parser / UUID / schema / path / runner / logging /
serialization / CLI / ordinary refactor. Those are support work on the
current science EXP (`experiment-record.md` §Support-task rule).

**What method decision will this experiment change?** If that cannot be
answered, **do not register** this Experiment.

Do not register an EXP “to complete the matrix” (e.g. 3 seeds → 5 seeds)
unless current variance still blocks a go/no-go judgment.

**Default: one seed.** Ordinary sanity / exploratory / mechanism-off /
deletion tests use **one run, one seed**. Do not launch 3/5 seeds up front.
Multi-seed confirmation only after a **clear positive** (the hypothesis
direction is supported and stability is now the question).

When the method is growing complex, prefer a **deletion experiment**
(`Full` vs `Full - Component A`) over `Full + B + C + D`.

## Compact (default)

Ordinary exploratory / sanity. Do **not** open `experiment-proposal.md`,
`experiment-thinking.md` (A→H), `scientific-reasoning.md`, `git-linking.md`,
`story-loop.md`, or `experiment-record.md`.

Already-planned sanity: the five in-session items are already in the EXP
section — verify, do not rewrite.

| In-session | Maps to EXPERIMENTS section |
| --- | --- |
| **Question** | Question |
| **Why this matters** | Motivation |
| **Honest baseline/comparison** | Comparisons |
| **What observation would change next action?** | Expected outcomes (design note; else Motivation) |
| **Data / Setup / Codebase** | Data / Setup, Code — as needed |

Do **not** auto-upgrade for: single sanity, basic reproduction, extra seed
before a clear positive, logging check, known-question retry, small
diagnostic probe.

If this Question is already `planned` in EXPERIMENTS: skip a new EXP-ID; go to
`experiment-execution` only if code must run, else compact `result-analysis`
of supplied artifacts.

Compact persist: map the five items into the existing EXP section; no new
canonical files, index columns, or Protocol enums. Status `planned`, Outcome
`not-assessed`. Update STATE: Workflow Position `W2 TEST`, recommended next
(`experiment-execution` if a run is needed). Required reads: `.research/EXPERIMENTS.md` (target EXP section / index lookup only); `STORY.md` /
`STATE.md` only if the Question is not already on disk.

**Full design — continue past the stop line only if any:** mechanism claim,
new Core Idea, high compute, important ablation, control that isolates a
mechanism, publication-facing comparison, high-risk split/grouping.

**Stop. Do not read the rest of this file unless full-mode triggers fire.**

---

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

Produce one experiment specification. Full design should **maximally change
current judgment** if answered — not a parameter sweep (`story-loop.md`;
full design only). Compact exploratory /
sanity may be a cheap probe; still one scientific Question, still honest
about the baseline.

Record in `EXPERIMENTS.md` with index row, Status `planned`, Outcome `not-assessed`.

## Compact vs full (Skill-internal)

Choose **before** loading `experiment-proposal.md` or experiment-thinking
A→H. Default is **compact**. Compact operators are above the stop line.
This remainder is **full design**.

### Full design — load if any

Load `experiment-proposal.md` and walk experiment-thinking A→H if **any**:

- mechanism claim
- new Core Idea
- high compute
- important ablation
- control that isolates a mechanism
- publication-facing comparison
- high-risk split/grouping

Do not auto-upgrade: see **Compact (default)**. Do not invent a Protocol
flag for exploratory (`scientific-reasoning.md` §F on full design).

## Default flow

### Compact flow (default)

See **Compact (default)** above the stop line.

### Full design flow

Only after a full-design trigger matches. Load
`experiment-proposal.md` and `experiment-thinking.md` A→H.

1. **Anchor gap** — Read `.research/STORY.md` (Open Gaps, Boundary),
   `.research/STATE.md`, relevant `DISCOVERY.md` (especially Invalidated /
   Negative), and `.research/PROJECT.md` constraints.
2. **Check duplicates** — Scan `EXPERIMENTS.md` index and DISCOVERY Invalidated;
   avoid redoing negated routes without new mechanism. One scientific question
   per EXP-ID; supersede when replaced.
3. **Assign EXP-ID** — Only after §What deserves a new EXP-ID? and
   **What method decision will this experiment change?** Next sequential
   `EXP-NNN`; optional short title in heading. Parser/UUID/schema/path
   work does **not** get a new ID.
4. **Draft core specification** — Fill these design fields (map to section fields
   in `experiment-record.md`):

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
   root `results/EXP-xxx/` per `git-linking.md`.
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
| Required (compact) | `.research/EXPERIMENTS.md` (target EXP section / index lookup only); `.research/STORY.md` / `.research/STATE.md` only if the Question is not already on disk |
| Do not open (compact) | See **Compact (default)** |
| Often (full design) | `.research/STATE.md`, `.research/DISCOVERY.md`, `.research/LITERATURE.md` |
| Reference (full design only) | `experiment-record.md`, `git-linking.md`, `story-loop.md` |
| Layer 2 (full design only) | `scientific-reasoning.md`, `experiment-thinking.md` |
| Prompt (full design only) | `experiment-proposal.md` (Main still writes EXPERIMENTS) |

## Updates

| File | What to update |
| --- | --- |
| `.research/EXPERIMENTS.md` | Index row + new `EXP-xxx` section (`planned`, Outcome `not-assessed`) |
| `.research/STATE.md` | Active EXP, next step, blockers |

Do **not** write Interpretation, `DISCOVERY.md`, or `STORY.md` here. Numbers and
metrics belong in EXPERIMENTS after runs, not in STORY.

## Deviation allowed

- Extend an existing `planned` EXP instead of creating a new ID when scope matches.
- Sketch multiple comparisons in one EXP when they share one scientific question.
- Ordinary exploratory / sanity: compact in-session items are enough; do not
  auto-upgrade (see **Compact (default)**). Do not invent a Protocol flag.
- Defer Runs detail until execution if setup is exploratory.
- Pair with `literature-research` when baseline choice is uncertain.
- Document Expected outcomes inline under Motivation when section has no dedicated field.
- Mark low-priority sweep as explicitly secondary — still tie to one judgment-changing question.
- Avoid standalone hyperparameter grids unless they test a stated mechanism.
- High-cost designs are full design: note Compute estimate and Review recommendation.
- Do not fabricate Git commits or result artifacts; recording a real existing
  baseline SHA or a planned results directory is allowed.
