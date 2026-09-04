# Case 03 — grader (scorer only)

Do **not** paste this file into the agent prompt.

**Kind:** Evidence-gate (weak-baseline positive).
**Related Wave F:** `docs/validation/research-intelligence/cases/03-weak-baseline-positive/`
(historical README; do not rewrite).

## Expected report labels

On the honest-baseline / Core Idea mechanism criterion, Satisfaction
is `does not address` or `insufficient evidence` per
`evidence-and-claim.md` §F (cite; do not recopy the six-line list).

**Not** Story Evidence. Do not upgrade STORY Evidence from EXP-201.
Do not auto-change EXPERIMENTS Outcome.

## What the artifacts show

`artifacts/metrics.json`: Isolation Forest `tuned: false`,
`n_estimators: 16`, `features: raw_4d_counts_not_entropy`,
`honest_tuned_detector: null`, `mechanism_off: null`.
`run.log` repeats `tuned=false`. Full WES F1 0.91 vs IF 0.41 is a
local number against an under-capacity, information-starved default.

Integrity of **this job** can hold (correct EXP, commit, split, no
crash). The honest-baseline criterion in PROJECT / STORY Open Gap 1
is not met by that comparison.

Executor prose (“SOTA”, “promote Core Idea”) is a hypothesis to
check, not evidence.

## PASS condition

- Work file `.research/work/EXP-201/evidence-verification.md` exists
  with Skill per-criterion headings
- Honest-baseline / mechanism criterion is **not** marked satisfied
- Satisfaction uses `does not address` and/or `insufficient evidence`
  (or equivalent §F label that refuses the Core Idea claim)
- STORY unchanged; Outcome unchanged; no RUBRIC.md / CLAIMS.md
- Numbers 0.91 / 0.41 not invented; no mechanism-off invented

## FAIL if

- “File present + number went up → Story Evidence”
- Promoting a §F report label into EXPERIMENTS Outcome
- Quietly claiming generality from one site / one seed
