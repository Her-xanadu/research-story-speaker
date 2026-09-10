# Experiment Thinking

Layer 2 — Research Intelligence. How to design a test that can change belief.

**Not Protocol.** Layer-2 boundary: cite `scientific-reasoning.md`. This file
teaches *design reasoning*. Canonical EXP fields, Status, Outcome, and
Experiment vs Run: `experiment-record.md`. Git binding: `git-linking.md`.
It does not add state files, columns, or enums.

Load when specifying a new EXP, a serious ablation, or a control that must
isolate a mechanism. Skip a one-line sanity rerun whose Question is already
on disk.

Objects and rivals: `scientific-reasoning.md`. Mechanism identity and the
smallest Idea-level test: `idea-and-mechanism.md`. After artifacts exist:
`evidence-and-claim.md`.

---

## A. Scientific question first

Write these **before** model, config, seed, or compute:

```text
Question
Hypothesis
Rival hypothesis
Prediction
```

Then, and only then:

```text
model / config / seed / compute
```

If you cannot name a rival and a prediction, you are not ready to spend
compute — unless the EXP is explicitly **exploratory** (a pilot that may
generate hypotheses and must not be sold as confirmatory). Mark that in the
EXP section in language. Do not invent a Protocol flag for it.

Exploratory vs confirmatory is a reasoning distinction
(`scientific-reasoning.md` §F), not a new Status.

`NEW` EXP-ID is the design of one scientific question, not a training job.
How that question is stored, and how one Experiment relates to its Runs, is
Protocol (`experiment-record.md`).

Persist into the existing EXP section without new columns:

```text
Question              → Question
Why this gap          → Motivation
Controls / baselines  → Comparisons
Prediction matrix     → Expected outcomes (design note; under Motivation
                        if the section has no dedicated field)
Unit, split, budget   → Data / Setup
```

Hypothesis, rival, and prediction must appear in that prose. They are not
new index-table fields.

A later model choice that cannot be derived from the Question is a different
experiment, not a clever config.

---

## B. Unit of analysis

Name four units. They are often *not* the same:

```text
sample unit         — one row / packet / frame / token / request / image
experimental unit   — what you actually randomize, assign, or hold out
analysis unit       — what the metric is computed on
grouping unit       — what must not leak across train / test / folds
```

**Pseudoreplication:** many samples that are not independent given the grouping
unit. Inflated *n* is not more evidence. Confidence intervals computed as if
samples were iid will look decisive and still be wrong.

In ML, systems, and network security, check at least the groupings that exist
in *this* dataset:

```text
subject
device
source
capture
pcap
session
day
hospital / site
user
```

Typical failures (teach the pattern; do not chant all nine every time):

- 10k flows from 3 captures → experimental unit closer to 3 than to 10k.
- 8k images from 12 patients → split by patient, not by slice.
- 1M requests from 4 services → grouping is service (or tenant), not request.
- Same user / device / session in both train and test → identity leakage,
  not generalization.
- Random row split inside one pcap or one hospital day → capture/site
  memorization dressed as accuracy.

If the grouping unit is what the Story claims to generalize over (new site,
new capture, new user), that unit belongs in the **split**, not only in a
footnote after a pretty interval.

Fix the unit **in the design**. Do not “correct” it after the number looks
good. A split that leaks the grouping unit cannot be repaired by a larger
model.

---

## C. Confounders

Ask which of these could produce the same metric movement **without** the
claimed mechanism:

```text
data split
seed
initialization
training budget
model capacity
augmentation
optimizer
preprocessing
extra information
hyperparameter tuning opportunity
```

These are **design-time knobs**. Competing *explanations* of a finished result
are owned by `scientific-reasoning.md` §C. Do not paste that rival list here.
Use this list to decide what to equalize **before** the run.

You do not need to equalize everything. Equalize the confounders that are the
**best rival** for this Question. If the favorite method received twice the
tuning budget, “it won” is not a mechanism result. If it saw a side channel
the baseline did not, “it won” is an information result — say so, or add an
information-matched control.

A seed sweep that never equalizes budget or split is still a confounder
study, not a mechanism study.

---

## D. Controls taxonomy

Teach the *kinds*. Use the smallest set that kills the main rivals.

| Control | Isolates | Typical when |
|---------|----------|--------------|
| **baseline control** | Any gain over the current honest default? | Almost every confirmatory EXP |
| **mechanism-off control** | Same scaffold, claimed component removed or zeroed | Core Idea / mechanism claims |
| **sham control** | Look-alike that should *not* help if the identity is real | Remove would also change cost or capacity |
| **oracle control** | Upper bound with information the method must not use in deployment | Need to know whether the signal exists at all |
| **negative control** | A setting where the mechanism predicts *no* gain | Specificity; “it should be quiet here” |
| **budget-matched control** | Same compute / steps / data volume | Training budget is a plausible rival |
| **information-matched control** | Same features or side-channel access | Extra information is a plausible rival |
| **architecture-matched control** | Same capacity / depth / width without the mechanism | Size or depth is a plausible rival |

Not every EXP needs all eight. An ordinary exploratory sanity check may need
only a baseline. A Core Idea claim needs at least a **mechanism-off** or
**information-matched** control, plus fairness on budget or capacity if those
are plausible rivals.

Pick controls from the rival, not from a completeness fetish:

- Rival is extra features → information-matched, not another learning-rate.
- Rival is “bigger net” → architecture-matched or capacity cap.
- Rival is “more steps” → budget-matched.
- Rival is “the gadget looks fancy” → sham (keep cost/appearance, break the
  information flow).
- Rival is “any detector would fire” → negative control on traffic or labels
  where the mechanism predicts silence.

If a positive result cannot be attributed because the design lacks the control
that would split target vs best rival, revise the **method** before results —
not after. That is a design judgment. It is not a Reviewer Verdict.

Oracle numbers are bounds, not Story Evidence for the deployable method.

### Manipulation check

When a new mechanism is **first executed**, its implementation
**materially changes**, or a control's **meaning changes**, do a
minimal check before costly attribution:

```text
off   — closing the component restores the promised baseline
on    — opening it produces the intended operation
sham  — the look-alike interrupts the intended relation
```

The check may be a formula, an existing artifact, a few samples, or one
update step. Record reuse in the design or in Run notes. If no cheap
pre-check exists, constrain the claim and inspect the first valid
artifacts. This is not a Protocol gate and not a new table.

Whole-method comparison ≠ mechanism attribution. Same algorithm or same
config does not automatically mean same exposure or same intervention.

If the mechanism did not activate, or the sham did not interrupt the
relation: first delimit **which questions are still validly tested**.
That failure does not negate the target mechanism. Do not stamp the
whole round uniformly invalid, and do not stamp it uniformly null.

```text
Before costly mechanism attribution, check that the intended intervention occurred and the chosen control changes the intended relation. Execution success does not establish a valid manipulation. Reuse unchanged checks; test only the contrast relevant to the claim.
```

If turning a component off still changes the base budget, or within-group
scores are constant so shuffle-before and shuffle-after are identical:
the comparison is not established. Name that and give the smallest fix.
Do not expand into extra repeats. Do not claim the target mechanism has
been negated.

---

## E. Prediction matrix

Before running, write result patterns → interpretations for the controls you
will actually run:

```text
Full > Mechanism-off > Baseline
  → mechanism may contribute (still check confounders)

Full ≈ Mechanism-off > Baseline
  → gain likely from shared design, not the named mechanism

Full ≈ Baseline
  → no detectable contribution under this test

Full < Baseline
  → mechanism or policy may be harmful here
```

Add rows that match the other controls you chose:

```text
Full ≈ Sham > Baseline
  → appearance or cost, not the named information flow

Full < Mechanism-off
  → claimed component may be hurting; do not rescue with extra knobs yet

Oracle ≫ Full > Baseline
  → headroom remains; incomplete mechanism, not automatically a false one

Negative-control: Full ≈ Baseline (as predicted)
  → specificity still plausible

Negative-control: Full > Baseline (against prediction)
  → some other channel is doing the work
```

If the matrix has only “we hope Full wins,” the EXP cannot change mechanism
belief. A confirmatory EXP whose every cell is compatible with both the
hypothesis and the best rival is decorative.

The matrix is design reasoning, not a second Outcome vocabulary. How a
*completed* Experiment is recorded remains `experiment-record.md`.

Cells are readable only if the intended intervention occurred and the
chosen control changed the intended relation (§D Manipulation check).
A completed run does not by itself fill a cell.

---

## F. Must / Nice / Cut

Split the plan **before** the matrix grows:

```text
must-run       — the discriminating comparison (target vs best rival)
nice-to-have   — extra seeds, extra datasets, extra ablations
cut            — sweeps that do not change the Question
```

Default bias of agents is to grow grids. **Cut is a success.**

Must-run owns the budget that can change the Question. Nice-to-have must not
delay or starve that comparison. Cheap nice-to-have in parallel is allowed
when it does not steal the discriminating run. Cut items do not run.

Do not invent a Protocol gate that serializes every seed behind a formal
Outcome. The point is scientific: extra cells are optional confirmation, not
a new Core Idea.

If RESOURCES cannot fund must-run, shrink the Question or park the Idea
(`idea-and-mechanism.md`). Do not promote a nice-to-have remnant into
“the experiment.”

---

## G. Ablation

Each ablation answers both:

```text
What does this test?
What should happen if the component matters?
```

If you cannot fill both lines, you do not yet have an ablation. You have a
knob.

Prefer, in order:

```text
remove    — delete the component
replace   — swap for a sham or a simpler rule
sham      — keep cost / appearance, break the information flow
```

Parameter sweeps are last, and only when the Question is about a threshold or
budget, not “maybe another learning rate will save the Story.”

Anti-patterns:

- Turning a weight from 1.0 to 0.9 and calling it “off.”
- Removing a component that the rest of the net immediately substitutes,
  with no prediction of that substitution.
- Ablating a cosmetic extra (`idea-and-mechanism.md` §G) and treating a null
  as proof that the Core Idea is strong.
- Sweeping widths when the identity claim is an information source.

If ablation *must-run* is just “try more widths,” the mechanism has not been
identified.

A mechanism-off control is often the same comparison as a remove/replace
ablation. Do not duplicate it as two EXP-IDs unless the Question changed.

---

## H. Cost / information

Prefer:

```text
low cost + high discrimination
```

Not:

```text
more runs = better research
```

A cheap mechanism-off on the **failure condition** (the data, split, or regime
where the current Story actually breaks) beats a 50-seed leaderboard that
cannot split rivals.

If RESOURCES cannot afford the discriminating test, do not substitute a
cheaper **invalid** test and call it evidence. An underpowered or leaked
comparison is not “something.” It is noise with a plot.

Stop condition (design note, not a new state file): once must-run can change
the target claim, stop. Nice-to-have is optional confirmation. Further cells
that cannot change Problem, Core Idea, or route are Cut
(`story-loop.md` gap priority).

Cost includes human and Agent attention. A design that requires every control
in §D on a sanity EXP is a process failure, not rigor.

---

## Using this file

Walk A → H when the EXP must isolate a mechanism or spend serious compute.
For an ordinary exploratory probe, A plus an honest baseline may be enough;
say so. Do not fire idea-evaluation, evidence-verification, or independent
review because this file exists.

This file does not:

- define Status or Outcome
- distinguish Experiment from Run (Protocol)
- authorize writing STORY, DISCOVERY, or a new canonical file
- turn Cut lists into a standing TASK_PLAN
