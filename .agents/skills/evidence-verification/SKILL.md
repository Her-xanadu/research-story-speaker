---
name: evidence-verification
description: >-
  Criterion-level Evidence Gate for one EXP-xxx: match artifacts to claims
  without treating a file as Story Evidence. Writes
  .research/work/EXP-xxx/evidence-verification.md. Prefer when a result is
  ready for Story Evidence, high-stakes result review, main baseline
  comparison, a surprising strong result, a mechanism claim, a Story-core
  change, or a READY_FOR_WRITING candidate. Do not use for ordinary
  exploratory results, running code, primary interpretation, or Reviewer
  critique.
---

# Evidence Verification

Thin Skill for **claim ↔ evidence matching** on one `EXP-xxx`.
Operators: [evidence-and-claim.md](../../references/research-intelligence/evidence-and-claim.md).
Claim kinds as needed:
[scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md).
Outcome values: [experiment-record.md](../../references/experiment-record.md)
§Outcome 值 — cite only; do not copy that table.
Verdict: [reviewer.md](../../subagents/reviewer.md) — cite only; do not copy
the Verdict list.
Claim-calibration words in evidence-and-claim.md are **report labels**, not
Outcome.

## When to use

Prefer — not every EXP. This is an **Evidence Gate**, not a universal blocker.

- Result ready for Story Evidence
- High-stakes result review (recommended before `experiment-review` result-review)
- Main baseline comparison
- Surprising strong result
- Mechanism claim
- Story-core change (Problem / Key Observation / Core Idea)
- READY_FOR_WRITING candidate

Do **not** use for: ordinary exploratory or sanity results (not mandatory),
running code (`experiment-execution`), primary interpretation and Outcome
writes (`result-analysis`), independent method/result critique
(`experiment-review`), or idea-level mechanism identity (`idea-evaluation`).

## Goal

Synthesize criteria from disk, check integrity then match, and record gaps.
Existence ≠ validity ≠ criterion satisfied ≠ claim supported
([evidence-and-claim.md](../../references/research-intelligence/evidence-and-claim.md)
§A). Output is a work artifact only:

```text
.research/work/EXP-xxx/evidence-verification.md
```

## Default flow

1. **Confirm trigger** — Ordinary exploratory result → stop; do not run this gate.
2. **Gather** — Read `.research/PROJECT.md`, `.research/STORY.md`, the `EXP-xxx`
   section, raw artifacts, code at the recorded Git commit, configs, and
   existing reviews. Trace via [git-linking.md](../../references/git-linking.md).
   Executor/analyst narratives are hypotheses to check, not evidence.
3. **Synthesize criteria** — Dynamically from PROJECT, STORY, EXP Question /
   Comparisons / pre-run prediction, and review conditions
   ([evidence-and-claim.md](../../references/research-intelligence/evidence-and-claim.md)
   §B). Do **not** create `RUBRIC.md` or freeze criteria across EXPs.
4. **Per criterion** — Integrity first (§D); then whether the artifact answers
   *this* criterion (§E); then Satisfaction using §F **report labels** (not
   Outcome). Keep scope as tested (§G). Cite §G Development, randomness,
   confirmation when the claim is effect / generality / replication /
   independent confirmation — do not recopy.
5. **Write one block per criterion** with exactly:

```text
Criterion
Source
Required evidence
Artifact
Evidence found?
Integrity status
Evidence match
Satisfaction
Evidence gap
Required action
```

6. **Stop** — Recommend `experiment-review` and/or Main integration. Main /
   Reviewer make the final scientific judgment.

## Reads

| Priority | Files |
|----------|-------|
| Required | `.research/PROJECT.md`, `.research/STORY.md`, `.research/EXPERIMENTS.md` (`EXP-xxx`), raw artifacts, code at Git commit, configs |
| Often | existing reviews under `.research/reviews/EXP-xxx/`, `.research/REVIEWS.md` |
| Layer 2 | [evidence-and-claim.md](../../references/research-intelligence/evidence-and-claim.md); [scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md) as needed |
| Protocol | [experiment-record.md](../../references/experiment-record.md) §Outcome 值 (cite only), [git-linking.md](../../references/git-linking.md) |
| Subagent | [reviewer.md](../../subagents/reviewer.md) Verdict (cite only) |

## Updates

| File | What to update |
|------|----------------|
| `.research/work/EXP-xxx/evidence-verification.md` | Per-criterion blocks only |

Do **not** write `.research/STORY.md`, DISCOVERY, EXPERIMENTS Outcome/Index, or
Reviewer files. Do **not** create `RUBRIC.md`, `CLAIMS.md`, or other canonical
state files.

## Forbidden

Must **not** auto-change Outcome, auto-write Story, or auto-approve claims.
Do not treat file presence as support, copy the Outcome table or Verdict list
into the report, or promote §F report labels into EXPERIMENTS as Outcome.

## Deviation allowed

- Skip entirely for ordinary exploratory results.
- Method-review conditions may become criteria; still write the work artifact,
  not a review file.
- If sources conflict, keep the stricter criterion and record the gap.
- When this Skill does not apply, a light this-EXP / this-commit check belongs
  to `result-analysis`, not this gate.
