# Experiment Review Prompt

Main Agent: when to review and how to dispatch — see the
[experiment-review](../skills/experiment-review/SKILL.md) Skill. This file only
provides the decision matrix and the `REVIEWS.md` synthesis template.
Independence and five-section headings:
[reviewer.md](../subagents/reviewer.md).

## Task fields

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence>
Relevant files:
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - .research/DISCOVERY.md → <relevant sections>
  - .research/RESOURCES.md
  - <code / config / result paths>
  - .research/work/<executor>.md
  - .research/work/<analyst>.md (if any)
Required output:
  - .research/reviews/<EXP-ID>/method-review.md (if needed)
  - .research/reviews/<EXP-ID>/result-review.md (if needed)
  - Main Agent later: summary in .research/REVIEWS.md
```

Subagents **read** these paths from disk; do not paste contents into dispatch messages.

## Review decision matrix

| Situation | Dispatch |
|-----------|----------|
| Design untested, no results yet | method-review only |
| Results exist, method never checked | method-review then result-review |
| Trusted method, questionable interpretation | result-review only |
| Both design and results contested | both, separate fresh contexts if possible |

## Synthesis template (Main Agent → REVIEWS.md)

```text
### <EXP-ID> — <short title>
- Story gap: ...
- Method verdict: <one line>
- Result verdict: <one line>
- Story impact: <one line>
- Next: <recommended move>
- Full reviews: .research/reviews/<EXP-ID>/
```

Subagents do **not** write `REVIEWS.md` — Main Agent synthesizes the summary.

## Quality bar

- One EXP-ID per review folder; do not mix experiments.
- If reviews disagree with analyst, document why in REVIEWS summary.
- Review does not replace updating EXPERIMENTS / DISCOVERY — it informs those updates.
