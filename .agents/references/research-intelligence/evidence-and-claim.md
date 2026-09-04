# Evidence and Claim

Layer 2 — Research Intelligence. How to match artifacts to claims without
lying with files.

**Not Protocol.** Layer-2 boundary: cite `scientific-reasoning.md`. This file
owns evidence-matching operators. §F labels are **verification-report
labels**, not Outcome.

Load when a result might enter Story Evidence, when a mechanism is claimed,
before high-stakes result review, or at READY_FOR_WRITING. Skip every
exploratory sanity metric.

Objects and claim kinds: `scientific-reasoning.md`. Units and controls used
when asking “does this metric answer this?”: `experiment-thinking.md`.

---

## A. Existence is not support

Keep four layers separate:

```text
artifact exists
  ≠ artifact is valid
  ≠ criterion is satisfied
  ≠ claim is supported
```

A `metrics.json` on disk is **existence**. A metric from the wrong commit,
split, EXP, or grouping unit is **not valid** for this claim. A valid metric
on a weak baseline may fail the criterion “beats an honest baseline.” Even a
satisfied *local* criterion does not automatically support a generality or
mechanism claim.

Executor and analyst narratives in `.research/work/` are hypotheses to check,
not evidence. Reviews are independent critique; they do not replace the
artifact (`reviewer.md`).

Typical collapses to avoid:

- File present → “we have evidence.”
- Number moved up → “criterion met.”
- Criterion met on one seed → “claim supported.”
- Claim supported locally → Story Evidence for a broader kind
  (`scientific-reasoning.md` §B).

---

## B. Criterion synthesis (no rubric file)

Pull criteria **dynamically** from the current disk, not from a standing
scorecard:

```text
PROJECT.md     — evaluation principles, completion conditions, constraints
STORY.md       — which claim kind is at stake (scientific-reasoning.md §B)
EXPERIMENTS.md — this EXP Question, Comparisons, stated prediction
Reviews        — method/result conditions already on the table
```

How to extract (judgment, not a template dump):

- PROJECT evaluation principles → fairness, leakage, reproducibility,
  what the method must not use.
- PROJECT completion conditions → what “done” would require; a single EXP
  almost never satisfies them by itself.
- STORY Evidence / Boundary / Open Gaps → the *scoped* claim, not a slogan.
  Name the claim kind; do not strengthen a mechanism claim with a
  performance number.
- EXP Question + Comparisons + the prediction written **before** the run →
  the local test. Post-hoc slices are not extra criteria
  (`scientific-reasoning.md` §F).
- Method review → missing controls, unit, fairness. Result review → rival
  explanations still standing. Those conditions become criteria for *this*
  verification, not a new state file.

Write criteria only in the verification **work artifact**. Never promote them
into `RUBRIC.md` or any other canonical file.

Good criterion: testable, scoped, tied to one claim kind, named comparison.
Bad criterion: “the method is promising,” “SOTA,” “robust,” “works in
general” without domain, split, or rival.

If two sources conflict (PROJECT says information-matched; the EXP only ran
Full vs a weak baseline), record the stricter criterion and an evidence gap.
Do not silently drop PROJECT.

---

## C. Evidence hierarchy

Each criterion should point at concrete objects (as many as apply):

```text
code
commit
config
log
metric
table
plot
prediction (the one written before the run)
split
receipt
dataset metadata
review
```

Prefer the **lowest** layer that actually answers the criterion. A slide
screenshot is weaker than the metric file plus the commit that produced it.

Which layer typically answers what:

| Need | Look first |
|------|------------|
| Was this the stated method? | code, commit, config |
| Was the run complete and this EXP? | log, receipt, EXP section |
| Was the split honest? | split, dataset metadata, grouping unit |
| Was this confirmatory? | prediction written before the run |
| What was observed? | metric, table, plot |
| Is an independent objection already on disk? | review |

Git recovery rules stay in `git-linking.md`. This file does not redefine
the binding tuple.

A plot without commit and split is decoration. A review without artifacts is
opinion. Dataset metadata that never names the grouping unit cannot support
a no-leakage criterion (`experiment-thinking.md` §B).

---

## D. Integrity first

Before interpretation, check:

```text
artifact exists?
correct experiment (this EXP, not a neighbor)?
correct commit?
correct split?
correct sample set / grouping unit?
missing run?
crash?
NaN?
duplicate?
leakage?
```

If integrity fails, **stop claim support**. A broken measurement does not
prove the hypothesis false. It also does not count as Story Evidence.

How the Experiment record and DISCOVERY are updated after an unusable run is
**Protocol plus result-analysis**, not this file. Do not copy Status or
Outcome tables here.

A crash or missing artifact is not a scientific negative. Do not write
Negative Discovery for it — that rule already lives in `result-analysis`.

Leakage, wrong object, or mis-bound results are integrity failures even when
the job “completed” and wrote a number.

Integrity is not optional decoration for high-stakes claims. For an ordinary
exploratory sanity metric, a light existence + “this EXP / this commit”
check is enough; do not expand this list into a universal gate.

---

## E. Evidence match

Not: “we found a metric.”

Ask: **does this metric answer this criterion?**

Mismatch examples:

- Criterion is mechanism isolation; metric is Full vs a weak baseline.
- Criterion is generality; metric is one dataset, one seed, one site.
- Criterion is efficiency; metric is accuracy at unbounded compute.
- Criterion is no-leakage; metric ignores grouping unit
  (`experiment-thinking.md` §B).
- Criterion is the pre-registered prediction; metric is a post-hoc slice
  or a lucky seed.
- Criterion is honest baseline; baseline was under-tuned, under-capacity,
  or denied information the method received
  (`experiment-thinking.md` §C–D).
- Criterion is replication; artifact is a single uncommitted notebook.

Mismatch → this evidence **does not address** the criterion, even if the
number looks good. That is an evidence gap, not a positive Story update.

Match requires the same claim kind, the same comparison, the same unit, and
the same scope. Adjacent metrics (AUROC when the criterion was calibration;
latency when the criterion was F1) are different questions.

---

## F. Claim calibration (verification report only)

For **each criterion**, the verification report uses exactly one of:

```text
supports
partially supports
does not address
contradicts
invalid evidence
insufficient evidence
```

These six strings are **report labels**. They are not Experiment Outcome
values, not Reviewer Verdicts, and not the speech-strength words in
`scientific-reasoning.md` §G.

Do **not** paste a report label into `EXPERIMENTS.md` as Outcome. Mapping a
finished Experiment to Outcome is Protocol (`experiment-record.md`) after
Main / Reviewer reconcile the whole EXP — one EXP can mix labels across
criteria.

Use:

| Report label | When |
|--------------|------|
| **supports** | Valid artifact, matched criterion, result as predicted, scope as stated |
| **partially supports** | Direction holds, but unit, fairness, or replication is incomplete |
| **does not address** | Artifact is about a different question (mismatch, §E) |
| **contradicts** | Valid, matched, and goes the other way on this criterion |
| **invalid evidence** | Integrity failed (§D); do not interpret the number |
| **insufficient evidence** | Valid enough to look, too weak (power, missing run, missing control) to decide |

A report may say `does not address` on a generality criterion while the same
artifacts still bear on a narrower local hypothesis. Story Evidence must
follow the **claim**, not the happy metric.

Do not auto-write Outcome, STORY, or DISCOVERY from this table. The executing
Skill (when present) owns the work-artifact path and the ban on writing
canonical state.

`partially supports` is a report label only. It is not an Outcome value.

---

## G. Scope

If the honest support is:

```text
dataset A + seed set B + condition C
```

do not upgrade language to:

```text
robust / general / universal
```

Put leftover ambition in Boundary or Open Gaps. Performance claims in Story
must not copy numbers (`state-files.md`); they also must not copy **scope
inflation**.

Claim-kind discipline (`scientific-reasoning.md` §B): a local performance
result does not license a mechanism sentence, an efficiency sentence, or a
cross-site sentence. Silence in Boundary is not a generality claim.

When previously trusted evidence is later shown leaked, mis-bound, or
otherwise unusable: do not invent a local rule and do not redefine Outcome
here. Follow `experiment-record.md` for the Experiment record and
`result-analysis` for DISCOVERY. That path uses **Invalidated Findings**, not
Negative Discoveries. Story must drop the Evidence line that rested on the
artifact.

---

## Using this file

Work output shape (judgment operators; not a new canonical schema):

```text
Criterion
Source
Required evidence
Artifact
Evidence found?
Integrity
Match
Satisfaction
Gap
Required action
```

One compact block per criterion is enough. Do not expand this into a standing
rubric. Ordinary exploratory results do not need this block.

This reference owns the *operators*. It does not authorize STORY, DISCOVERY,
or Outcome edits.

---

## Do not

- Treat artifact existence as claim support
- Create `RUBRIC.md` or freeze criteria across EXPs
- Copy Outcome or Verdict tables into this file
- Promote verification labels into Protocol
- Inflate dataset A + seed B + condition C into a law
- Write Negative Discovery for invalid previous evidence
  (`result-analysis` / `experiment-record.md`)
