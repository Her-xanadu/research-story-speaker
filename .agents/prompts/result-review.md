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
  - <code @ commit> (if interpretation depends on implementation)
Required output: .research/reviews/<EXP-ID>/result-review-r<N>.md
```

## Instructions for reviewer

1. **Read** experiment record, raw results, and any work reports from listed paths.
2. Judge **reliability** (run integrity, missing data, obvious bugs) before **support** (does evidence address the gap?).
3. Evaluate analyst interpretation — agree, refine, or reject with evidence.
4. Consider **alternative explanations** the data do not rule out.
5. **Write** the full review to `Required output` path, starting with the header in reviewer.md.
6. **Do not** edit canonical state files; Main Agent merges into `REVIEWS.md`.
7. **Do not** polish this `raw` body after writing it.

## Review questions (internal checklist)

- Do numbers in reports match raw artifacts?
- Is the effect size meaningful for the Story claim (not only statistically present)?
- Failure cases, variance, and negative subsets examined?
- Does outcome support, narrow, or contradict Core Idea / Boundary?
- Is a Story update justified, or are results null/inconclusive?

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
