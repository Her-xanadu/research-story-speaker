# Evidence Verification Prompt

Hand this prompt with the
[evidence-verification](../skills/evidence-verification/SKILL.md) Skill.
When-to-use, per-criterion block headings, and the ban on canonical writes:
that Skill.
Operators: [evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md).
Claim kinds as needed:
[scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md).

§F labels are **verification-report labels**. They are not Outcome.
Write `Satisfaction: <per evidence-and-claim.md §F>` — do not recopy the
six-line list. Do **not** paste a report label into `EXPERIMENTS.md` as
Outcome.

Outcome values: [experiment-record.md](../references/experiment-record.md)
§Outcome 值 — cite only; do not copy that table.
Verdict: [reviewer.md](../subagents/reviewer.md) — cite only; do not copy
the Verdict list.

## Task fields

Main Agent fills in before dispatch. **Do not paste full state files or raw
result dumps.**

```text
EXP-ID: <e.g. EXP-031>
Trigger: <Story Evidence candidate | high-stakes result review | main baseline | surprising strong result | mechanism claim | Story-core change | READY_FOR_WRITING>
Claim under test: <one sentence, scoped; name the claim kind>
Story gap: <one sentence>
Relevant files:
  - .research/PROJECT.md
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - <raw artifact paths: metrics, logs, plots, receipts, splits>
  - <code repo> @ <commit recorded for this EXP>
  - <configs>
  - .research/reviews/<EXP-ID>/ (if any)
  - .research/REVIEWS.md (if any)
  - .research/work/<executor-or-analyst>.md (hypotheses to check, not evidence)
Required output: .research/work/<EXP-ID>/evidence-verification.md
```

If the result is an ordinary exploratory or sanity metric: **stop and write
nothing.** This is an Evidence Gate, not a universal blocker. Do not use this
prompt to run code, write primary interpretation, or replace Reviewer
critique.

## When this applies

Prefer — not every EXP — when a result might enter Story Evidence, before
high-stakes result review, on a main baseline comparison, a surprising strong
result, a mechanism claim, a Story-core change, or a READY_FOR_WRITING
candidate.

Do **not** use for: running code (`experiment-execution`), Outcome writes
(`result-analysis`), independent method/result critique (`experiment-review`
+ [method-review.md](method-review.md) / [result-review.md](result-review.md)),
or idea-level mechanism identity (`idea-evaluation`).

## Scientific context to read

Read listed paths from disk. Trace commits via
[git-linking.md](../references/git-linking.md). Units and “does this metric
answer this?”: [experiment-thinking.md](../references/research-intelligence/experiment-thinking.md).

Synthesize criteria **dynamically** from current disk, not from a standing
scorecard (evidence-and-claim.md §B). For this gate: pull from PROJECT,
STORY, this EXP section, and any on-disk reviews — §B says what each source
is for. Do **not** create `RUBRIC.md`, `CLAIMS.md`, or `FINDINGS.md`. Do not
freeze criteria across EXPs. If two sources conflict, keep the **stricter**
criterion and record a gap. Do not silently drop PROJECT.

## Research reasoning lenses

Keep four layers separate (evidence-and-claim.md §A). Cite §A; do not treat
this prompt as a second owner. For each Task criterion, walk existence →
validity → criterion-met → claim-supported, and stop at the first layer that
fails. A path on disk is not support for the Claim under test.

Treat `.research/work/` executor/analyst prose as claims to check against
artifacts, not as evidence. A review is independent critique; it does not
stand in for the file.

Apply claim-kind discipline (scientific-reasoning.md §B) to the Claim under
test: Satisfaction on one kind does not license a sentence of another kind.
Apply scope (evidence-and-claim.md §G) in the work file: report what was
actually tested; do not inflate the sentence. If the Claim under test is
effect / generality / replication / independent confirmation, name the
evidence role (development screening / same-condition randomness recheck /
independent confirmation) — evidence-and-claim.md §G Development,
randomness, confirmation; scientific-reasoning.md §F. A seed change on an
already-used evaluation set is a randomness recheck, not new independent
generality confirmation.

## Per-criterion operators

For each synthesized criterion, walk existence → validity → match → scope →
gap → action. Integrity **before** interpretation (evidence-and-claim.md §D).
If §D fails, this gate must not mark the claim supported and must not treat
the number as Story Evidence. Status / Outcome / DISCOVERY after an unusable
run are Protocol plus result-analysis — cite
[result-diagnosis.md](result-diagnosis.md) §8; do not restate that mapping.

### Criterion source

Where on disk this test came from (PROJECT principle, STORY claim kind, EXP
Question / comparison / pre-run prediction, method-review condition, result
review rival still standing). Reject unscoped slogans as criteria. Require a
testable, scoped, one-kind comparison.

### Required evidence

Name the lowest layer that actually answers this criterion
(evidence-and-claim.md §C). For this gate: the Actual artifact line needs
path + commit + EXP-ID. A plot or review that cannot be bound to this EXP
is not usable here.

### Actual artifact

Concrete path + commit + EXP-ID. Neighbor EXP files do not count.

### Existence / validity

- Existence: the object is on disk (`Evidence found?`).
- Validity / integrity: fill `Integrity status` from evidence-and-claim.md
  §D for *this* EXP. A completed job that wrote a number can still fail
  this line.

### Match

Not “we found a metric.” Ask whether this artifact answers **this**
criterion (evidence-and-claim.md §E). If it answers a different question,
record a gap — not a positive Story update. Adjacent metrics are different
questions.

### Claim scope

State the support actually earned (dataset + seeds + condition). Name the
claim kind. An empty Boundary section does not earn a generality sentence.

### Gap and required action

What is missing (control, unit, replication, commit bind, prediction that
was never written). What Main / Reviewer should do next — verify, rerun,
method-review, narrow the claim, or refuse Story Evidence. This prompt does
not perform those writes.

## Critical questions

- Which claim kind is at stake, and is this metric even about that kind?
- Was the prediction on disk *before* the run (scientific-reasoning.md §F)?
- Would an information-matched or mechanism-off control still be required
  for this criterion to be matchable?
- Are we about to promote a report label into Outcome?
- If we deleted the narrative in the executor report, would the artifacts
  still support the sentence?

## Fatal flaws / anti-patterns

**Forbidden:**

- Auto Outcome (do not change EXPERIMENTS Outcome / Index)
- Auto Story (do not write STORY Evidence or rewrite Core Idea)
- Auto-approve claim (Satisfaction is a report label, not a rubber stamp)

Also refuse:

- Treating `Evidence found? = yes` as `Satisfaction` supports
- Treating a matched local metric as Story Evidence
- Treating one seed as a generality claim
- Treating a seed change on an already-used evaluation set as independent
  generality confirmation (evidence-and-claim.md §G Development,
  randomness, confirmation)
- Copying the Outcome table or Verdict list into the report
- Promoting §F labels into Protocol
- Creating `RUBRIC.md` or freezing criteria across EXPs
- Writing Negative Discovery for invalid previous evidence (that path is
  Invalidated Findings via result-analysis / experiment-record.md)

## Evidence requirements

Prefer the lowest layer that answers the criterion. Point at files, commits,
splits, and pre-run predictions. Do not invent numeric confidence scores.
Qualitative strength of *support kinds* (if needed) is scientific-reasoning.md
§E — cite; do not turn that ladder into percentages.

One EXP may mix §F labels across criteria. Mapping the whole Experiment to
Outcome is Protocol after Main / Reviewer reconcile — not this gate.

## Decision logic

1. Trigger is ordinary exploratory → stop; no file.
2. Synthesize criteria from disk; keep the stricter when sources conflict.
3. Per criterion: existence → integrity. Integrity fail → `Satisfaction` per
   §F for a measurement that cannot be used; do not interpret the number.
4. Valid artifact: match vs mismatch. Mismatch → the §F label for a different
   question; record gap.
5. Matched: calibrate with **one** §F report label (cite §F; do not recopy).
6. Scope the sentence to what was tested. Do not inflate.
7. Write the work file. Recommend experiment-review and/or Main integration.
   **Stop.** Do not edit canonical science files.

## Required output

Write only:

```text
.research/work/EXP-xxx/evidence-verification.md
```

One compact block per criterion, headings exactly as the Skill (Criterion,
Source, Required evidence, Artifact, Evidence found?, Integrity status,
Evidence match, Satisfaction, Evidence gap, Required action).

`Satisfaction` is exactly one §F report label. Say they are report-only.

When re-verifying the same EXP, update this file (or add a clearly dated
section in the same path). Do not write `.research/reviews/` — that path is
Reviewer-only.

## Handoff / state impact

- Writer writes `.research/work/EXP-xxx/` only.
- **Do not** edit `STORY.md`, `DISCOVERY.md`, EXPERIMENTS Outcome/Index, or
  Reviewer files.
- Main / Reviewer make the final scientific judgment. This gate matches
  artifacts to criteria; it does not close the Story.
- After a clean high-stakes verification, Main may still dispatch
  result-review. Verification is not a Verdict.

Return: EXP-ID, criterion count, any integrity failures, and the list of
Satisfaction labels **as report labels**. Do not polish the work file after
writing it.

## Stop / escalation

- Ordinary exploratory result → stop with no file.
- Integrity failed → stop claim support; recommend repair or rerun, not
  Story update.
- Sources conflict → stricter criterion + gap; escalate to Main, do not
  drop PROJECT.
- Method never reviewed and flaws affect match → note it; recommend
  method-review. Still write this work artifact, not a review file.
- READY_FOR_WRITING with inflated scope → required action is to narrow
  claims, not to approve the draft.
