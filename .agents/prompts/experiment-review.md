# Experiment Review Prompt

Main Agent orchestrates **independent review** for one experiment. Use the `experiment-review` skill when available.

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

## When to review

Prioritize review when any apply:

- High-cost or long-running experiment before scaling up
- New core method or pipeline change
- Anomalous or Story-threatening results
- Closing an important route (completed / abandoned)
- Story change to Problem, Key Observation, or Core Idea

## Orchestration steps

1. Read EXP section and Story gap; decide **method**, **result**, or **both**.
2. Dispatch `reviewer` subagent with:
   - `.agents/subagents/reviewer.md` as role definition
   - `.agents/prompts/method-review.md` and/or `result-review.md` as task prompt
   - filled Task fields (EXP-ID, Story gap, Relevant files, Required output)
3. Ensure each reviewer inspects **direct evidence**, not only work reports.
4. Collect outputs from `.research/reviews/<EXP-ID>/`.
5. **Main Agent** synthesizes a short entry into `REVIEWS.md` — subagents do not edit it.

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

## Quality bar

- One EXP-ID per review folder; do not mix experiments.
- If reviews disagree with analyst, document why in REVIEWS summary.
- Review does not replace updating EXPERIMENTS / DISCOVERY — it informs those updates.
