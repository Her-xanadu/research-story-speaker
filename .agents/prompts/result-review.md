# Result Review Prompt

Hand this prompt to the `reviewer` subagent (or any fresh-context reviewer who did not run the experiment).
Independence policy and Required output headings:
[reviewer.md](../subagents/reviewer.md).

## Task fields

Main Agent fills in before dispatch. **Do not paste full state files or raw result dumps** — read from disk.

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence>
Relevant files:
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - <result location from experiment record>
  - <log / metric / plot paths>
  - .research/work/<executor-report>.md
  - .research/work/<analyst-report>.md (if any)
  - <code @ commit> (if interpretation depends on implementation)
Required output: .research/reviews/<EXP-ID>/result-review.md
```

## Instructions for reviewer

1. **Read** experiment record, raw results, and any work reports from listed paths.
2. Judge **reliability** (run integrity, missing data, obvious bugs) before **support** (does evidence address the gap?).
3. Evaluate analyst interpretation — agree, refine, or reject with evidence.
4. Consider **alternative explanations** the data do not rule out.
5. **Write** the full review to `Required output` path.
6. **Do not** edit canonical state files; Main Agent merges into `REVIEWS.md`.

## Review questions (internal checklist)

- Do numbers in reports match raw artifacts?
- Is the effect size meaningful for the Story claim (not only statistically present)?
- Failure cases, variance, and negative subsets examined?
- Does outcome support, narrow, or contradict Core Idea / Boundary?
- Is a Story update justified, or are results null/inconclusive?

## Required review body

Use [reviewer.md](../subagents/reviewer.md) Required output headings. Fill each
heading from the **result** perspective:

- strongest evidence — most convincing result with file/metric pointers
- main weakness — biggest threat to trusting these results
- alternative explanation — best rival reading of the same outputs
- story impact — specific STORY segments affected; avoid rewriting full Story
- recommended next move — follow-up EXP, replication, abandon route, or accept and update DISCOVERY

## Return to caller

After writing the file, return the same five sections to Main Agent.

## Coordination

- If method was never reviewed and flaws affect interpretation, note that and recommend `method-review.md` retroactively.
- Distinguish **technical failure** (`failed` EXP) from **scientific null** (valid negative result).
