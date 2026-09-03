# Result Review Prompt

Hand this prompt to the `reviewer` subagent (or any fresh-context reviewer who did not run the experiment).
Independence header, Verdict, and Required output headings:
[reviewer.md](../subagents/reviewer.md).
**Model relation** is relative to the **experiment executor / primary analyst**.
Write `Verdict: <per reviewer.md>` — do not recopy the Verdict list.

## Task fields

Main Agent fills in before dispatch. **Do not paste full state files or raw result dumps** — read from disk.

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence>
Review round: N
Relevant files:
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - <result location from experiment record>
  - <log / metric / plot paths>
  - .research/work/<executor-report>.md
  - .research/work/<analyst-report>.md (if any)
  - .research/work/<EXP-ID>/evidence-verification.md (if present)
  - <code @ commit> (if interpretation depends on implementation)
Required output: .research/reviews/<EXP-ID>/result-review-r<N>.md
```

## Optional Layer 2 (this task only)

Load when interpreting support, mechanism, or Story impact. Skip for a crashed
run whose integrity already fails. **Do not** load the rest of
`research-intelligence/` for this review.

- [evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)
  — integrity first, match, scope; §F labels are report-only, not Outcome
- [scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
  — claim kinds, rivals, exploratory vs confirmatory speech
- [idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
  — only if the result is used to claim a mechanism identity

An `evidence-verification` work file, if listed, is a hypothesis to check.
Still read **direct artifacts**. That Skill is not mandatory for every review.

## Instructions for reviewer

1. **Read** experiment record, raw results, and any work reports from listed paths.
2. Judge **reliability** (run integrity, missing data, obvious bugs) before **support** (does evidence address the gap?).
3. Evaluate analyst interpretation — agree, refine, or reject with evidence.
4. Consider **alternative explanations** the data do not rule out.
5. **Write** the full review to `Required output` path, starting with the header in reviewer.md.
6. **Do not** edit canonical state files; Main Agent merges into `REVIEWS.md`.
7. **Do not** polish this `raw` body after writing it.

## Judgment lenses (internal)

Integrity first, then support. Apply the lenses that threaten **this** result.
Do not score every row. Cite files, commits, and metrics.

- **Integrity first** — artifact exists; this EXP; this commit; this split / grouping unit; missing run, crash, NaN, duplicate, leakage. If integrity fails, stop claim support. (`evidence-and-claim.md` §D)
- **Effect size** — meaningful for the Story claim, not only statistically present.
- **Variance** — seed / run / site spread; a single draw is not a law.
- **Heterogeneity** — does the effect hold across the units the Story claims, or only a lucky slice?
- **Baseline validity** — was the comparison honest (tuned, informed, capacity- and budget-matched)?
- **Claim-to-evidence matching** — does this metric answer *this* criterion / claim kind? Adjacent numbers are a different question. (`evidence-and-claim.md` §E)
- **Mechanism attribution** — does the comparison isolate the named process, or only show Full beat a weak default?
- **Alternative explanations** — best rival still standing for the same outputs. (`scientific-reasoning.md` §C)
- **Selection bias** — reported cells vs the full matrix; dropped seeds, quiet ablations, unpublished failures.
- **Post-hoc rescue** — slices, metrics, or hypotheses named after seeing the number sold as confirmatory. (`scientific-reasoning.md` §F)
- **Scope inflation** — dataset A + seed B + condition C upgraded to robust / general / universal. (`evidence-and-claim.md` §G)
- **Negative subsets** — failure cases examined, not only the happy mean.
- **What evidence would change the Verdict** — name the artifact, control, or replication that would flip it. Use reviewer.md §Verdict — do not recopy the list.
- **Numbers vs reports** — do work-file numbers match raw artifacts?
- **Story move** — support, narrow, or contradict Core Idea / Boundary? Is a Story update justified, or are results null / inconclusive?

## Required review body

Header, then `## Verdict` (`Use reviewer.md §Verdict`), then the five sections
in [reviewer.md](../subagents/reviewer.md), filled from the **result** perspective:

- strongest evidence — most convincing result with file/metric pointers
- main weakness — biggest threat to trusting these results
- alternative explanation — best rival reading of the same outputs
- story impact — specific STORY segments affected; avoid rewriting full Story
- recommended next move — follow-up EXP, replication, abandon route, or accept and update DISCOVERY

## Return to caller

After writing the file, return Verdict plus the same five sections to Main Agent.

## Coordination

- If method was never reviewed and flaws affect interpretation, note that and recommend a method review retroactively.
- Distinguish **technical failure** (`Status=failed`, Outcome stays `not-assessed`) from a **scientific** `null` / `contradicts` on a `completed` EXP. Outcome values: [experiment-record.md](../references/experiment-record.md) §Outcome 值.
