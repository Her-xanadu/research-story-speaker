# Method Review Prompt

Hand this prompt to the `reviewer` subagent (or any fresh-context reviewer with no designer history).
Independence header, Verdict, and Required output headings:
[reviewer.md](../subagents/reviewer.md).
**Model relation** is relative to the **method designer**.
Write `Verdict: <per reviewer.md>` — do not recopy the Verdict list.

## Task fields

Main Agent fills in before dispatch. **Do not paste full state files** — the reviewer reads from disk.

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence — which Open Gap or Boundary item>
Review round: N
Relevant files:
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - .research/RESOURCES.md → <codebase entry>
  - <repo path> @ <commit hash>
  - <config / script paths>
  - .research/work/<design-or-executor-report>.md (if any)
Required output: .research/reviews/<EXP-ID>/method-review-r<N>.md
```

## Instructions for reviewer

1. **Read** all listed workspace files and code artifacts from disk.
2. Determine whether the **experimental method** can answer the Story gap — not whether results look good.
3. Inspect **direct evidence**: design doc, code diff at commit, configs, data pipeline, baseline definitions.
4. Cross-check executor claims against implementation; note any design–implementation drift.
5. **Write** the full review to `Required output` path, starting with the header in reviewer.md.
6. **Do not** edit `STORY.md`, `EXPERIMENTS.md`, `REVIEWS.md`, or other canonical state files.
7. **Do not** polish this `raw` body after writing it.

## Review questions (internal checklist)

- Is the scientific question aligned with the stated Story gap?
- Are baselines and controls appropriate and fairly implemented?
- Leakage, split strategy, metric choice — any threats to validity?
- Reproducibility: can another agent rerun from commit + config?
- Cost proportionate to information gained?

## Required review body

Header, then `## Verdict` (`Use reviewer.md §Verdict`), then the five sections
in [reviewer.md](../subagents/reviewer.md), filled from the **method** perspective:

- strongest evidence — what makes the method credible for this gap
- main weakness — most serious methodological flaw or gap
- alternative explanation — if method succeeds, could it measure the wrong thing?
- story impact — how method quality affects trust in Evidence / Boundary / Open Gaps
- recommended next move — fix design, add control, rerun, or proceed to result review

## Return to caller

After writing the file, return Verdict plus the same five sections (concise summary acceptable).

## Escalation

Recommend a separate result review once method is adequate and raw results exist.
If method is fatally flawed, say so explicitly — do not defer to results.
