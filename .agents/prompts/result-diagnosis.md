# Result Diagnosis Prompt

Used by [result-analysis](../skills/result-analysis/SKILL.md) and
[result-analyst](../subagents/result-analyst.md). This is **scientific
interpretation**, not Reviewer critique.

Outcome values: [experiment-record.md](../references/experiment-record.md)
§Outcome 值 — **cite that section; do not recopy the table.**
Judgment operators:
[evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md),
[scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md).

Dispatch: [subagent-handoff.md](subagent-handoff.md). **Do not write Reviewer
files.**

## Role / Objective

Turn raw artifacts into a **belief-changing diagnosis**: what is usable, what
it actually shows, which rival still stands, which Outcome candidate to
recommend, and the next discriminating move. Observation is not interpretation
([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
§A). A file on disk is not claim support
([evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)
§A).

## When this task applies

- `experiment-execution` finished and artifacts exist (including ugly, null,
  or crashed runs).
- Main needs interpretation separate from the executor, especially on anomaly,
  high cost, Core Idea, or a result that would rewrite Story.
- Previously trusted evidence may have become unusable.

Skip turning this into an Evidence Gate or a Review. Ordinary exploratory
runs still follow the **order** below; they may mark later steps `n/a` once
integrity fails or the Question was only a sanity check. Do **not** skip
Integrity.

## Task fields

Main fills this before dispatch. List paths; **read from disk**.

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence>
Relevant files:
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - .research/DISCOVERY.md
  - <result / log / metric / plot paths>
  - .research/work/<executor-report>.md (if any)
  - <code @ commit> (if attribution depends on implementation)
Required output: .research/work/<task-slug>.md
```

## Scientific context to read

1. EXP section: Question, Comparisons, pre-run prediction, Status, current
   Outcome, Git, Results.
2. Raw artifacts at those paths — not only the executor narrative.
3. STORY Evidence / Boundary / Open Gaps; name the **claim kind** at stake
   ([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
   §B).
4. DISCOVERY Positive / Negative / Null / Invalidated — do not revive a killed
   route without a new mechanism.

Executor and analyst prose in `.research/work/` are hypotheses to check.

## Research reasoning lenses

**Analyze in this order. Do not reorder. Do not start at Story.**

Method-First overlay (no new required headings; answer inside 1–10):

```text
Integrity → What happened? → Which prediction was supported?
→ Which rival remains? → Mechanism diagnosis → Method consequence
→ Story impact → Next discriminating experiment
```

§5 + §7 must name the mechanism diagnosis and the strongest remaining
rival. §9 must state the method consequence (`keep` / `simplify` /
`delete component` / `change mechanism` / `change control` /
`abandon mechanism`). §10 must be a discriminating next experiment, not
more seeds unless variance is the scientific question. Do not stop at
metric movement.

If a step is blocked by an earlier failure, write `n/a — <why>` and continue
the numbering. Integrity failure **stops claim support**; it does not license
skipping the Outcome candidate or the next action.

### 1. Integrity

[evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)
§D, before any effect talk:

```text
artifact exists? this EXP? this commit? this split?
correct sample / grouping unit? missing run? crash? NaN? duplicate? leakage?
```

A broken measurement does **not** prove the hypothesis false. It is also not
Story Evidence.

### 2. Actual effect

What moved, in which direction, relative to the **pre-run** prediction — not
a slice found while browsing
([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
§F). Size and practical meaning first; “statistically present” is not enough.
Metric movement ≠ mechanism
([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
§A).

### 3. Variance

Seeds, sites, captures, grouping unit. Many rows that are not independent are
not more evidence. If variance swallows the effect, say so here — do not wait
for a Story sentence.

### 4. Baseline fairness

Did the favorite method get extra information, budget, capacity, or tuning
the comparison was denied? An unfair win is not support for the claim kind
in STORY. Name the missing match; do not invent a new Outcome word for
“unfair.”

### 5. Mechanism attribution

If the result is positive, can the gain be attributed to the **named**
mechanism, or only to the shared scaffold? If the design never isolated the
component, do not recommend a mechanism claim — recommend a discriminating
control as the next action. Performance on a weak baseline does not prop up
a mechanism claim
([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
§B).

### 6. Heterogeneity

Where did it fail? Negative subsets, sites, classes, or regimes that were
**not** the registered prediction stay exploratory. Do not launder a lucky
slice into confirmatory Evidence.

### 7. Rival explanations

Name the **single best rival** that could produce this same pattern
([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
§C). Skip rivals that cannot. State what control or split would make that
rival less tenable. If none exists, the mechanism is not ready for Story
Evidence.

### 8. Outcome candidate

Look up [experiment-record.md](../references/experiment-record.md) §Outcome 值
and recommend **exactly one** value. Do not recopy that table. Do not write
speech-calibration words from
[scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
§G into EXPERIMENTS as Outcome.

Hard mappings (Protocol already owns the words; this prompt only applies them):

- **Technical failure** (crash, missing artifact, unusable run): recommend
  `Status=failed`, `Outcome=not-assessed`. **No Negative Discovery.**
- **Completed but unusable** (leakage, wrong object, unfair comparison that
  voids inference): recommend Outcome `invalid` per §Outcome 值 — still **not**
  a Negative Discovery.
- **Previously trusted evidence** now shown leaked, mis-bound, or otherwise
  unusable: DISCOVERY **Invalidated Findings**, **not** Negative Discoveries
  ([evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)
  §G). Drop any STORY Evidence line that rested on that artifact (Main writes
  STORY).

A valid `contradicts` or `null` on a `completed` EXP is a scientific finding.
A failed job is not.

### 9. Story implication

Which claim kind is actually evidenced, at what scope
([evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)
§G)? Which of Problem / Key Observation / Core Idea / Evidence / Boundary /
Open Gaps would move? Do not copy metrics into Story. Do not inflate
dataset A + seed B + condition C into “robust / general.”

### 10. Next discriminating action

The **smallest** next test that would split the remaining rival from the
target mechanism — or an explicit stop. “Run more experiments” is not an
action. Parameter sweeps are not a discriminating action unless the Question
is itself a threshold.

## Critical questions

- If Integrity fails, are you still tempted to interpret the number?
- What would you have to see to **lower** belief in the claimed mechanism?
- Is the best rival still standing after this comparison?
- Would a Story update change Core Idea, or only Boundary wording?

## Fatal flaws / anti-patterns

- Starting at “this supports Core Idea” and backfilling Integrity.
- Treating technical failure as scientific `contradicts` or as Negative
  Discovery.
- Filing invalid **previous** evidence as Negative Discovery.
- Post-hoc metric / lucky seed sold as confirmatory.
- Mechanism sentence from Full vs a weak baseline.
- Copying Outcome or Verdict tables into the work file.
- Writing `.research/reviews/` from this prompt.

## Evidence requirements

Cite artifact paths, commit, split/unit, and the pre-run prediction. Prefer
the lowest layer that answers the step
([evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)
§C). If you cannot point at a file, you do not have that evidence.

## Decision logic

```text
Integrity fail on this run
  → stop claim support
  → Outcome candidate not-assessed (technical) or invalid (unusable completed)
  → no Negative Discovery
  → if this used to be trusted evidence → Invalidated Findings
  → next action: repair / bounded debug / redesign — not a sweep
  → if the mess is engineering-shaped, hand to failure-diagnosis.md

Integrity holds
  → walk 2–7 in order
  → one Outcome candidate from §Outcome 值
  → Story implication at honest scope
  → one discriminating next action (or stop)
```

If the run is a technical failure, **do not** complete a scientific rival
story as if the hypothesis were tested.

## Required output

Write `.research/work/<task-slug>.md` with **these headings in this order**:

```text
## 1 Integrity
## 2 Actual effect
## 3 Variance
## 4 Baseline fairness
## 5 Mechanism attribution
## 6 Heterogeneity
## 7 Rival explanations
## 8 Outcome candidate
## 9 Story implication
## 10 Next discriminating action
```

Under §8: recommended Status (if changed), recommended Outcome (one token
from §Outcome 值), DISCOVERY bucket
(`none` | `Positive` | `Negative` | `Null` | `Invalidated`), and a one-line
reason. Under §5/§7: mechanism diagnosis + the best rival, not a list of
ten. Under §9: method consequence (`keep` / `simplify` / `delete
component` / `change mechanism` / `abandon`). Under §10: one
discriminating experiment that could change the method judgment.

Return the same ten sections to the caller (short).

## Handoff / state impact

- **`result-analyst`:** work file only. No canonical state. **No Reviewer
  files.**
- **Main applying `result-analysis`:** after this diagnosis, Main may write
  EXPERIMENTS (including Outcome), DISCOVERY, STORY, STATE per that Skill
  and [state-files.md](../references/state-files.md) §更新顺序. Still **do
  not write Reviewer files** — that is `experiment-review`.
- Do not promote
  [evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)
  §F report labels into EXPERIMENTS as Outcome.

## Stop / escalation

- Integrity failure that looks like engineering or environment →
  [failure-diagnosis.md](failure-diagnosis.md), not a Story rewrite.
- High-stakes mechanism claim or READY_FOR_WRITING candidate → recommend
  `evidence-verification` and/or result review; this prompt does not replace
  them.
- Ordinary exploratory sanity: finish the ten headings quickly; do not
  inflate into a gate.
