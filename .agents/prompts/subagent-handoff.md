# Subagent Handoff

Main Agent: use this template when dispatching any subagent. Point the subagent at `.agents/subagents/<role>.md` for role rules.

## Handoff block (copy and fill)

```text
Role: <research-lead | literature-scout | experiment-agent | result-analyst | reviewer>
EXP-ID: <e.g. EXP-031, or N/A for research-lead / literature-scout>
Story gap: <one sentence — STORY Open Gap or Boundary item>
Relevant files:
  - .research/STORY.md
  - <other paths the subagent must read>
Required output:
  - .research/work/<task-slug>.md
  - OR .research/reviews/<EXP-ID>/<review-type>.md (reviewer only)
Task slug: <short-kebab-name for work file>
Additional context: <optional — hypotheses, constraints, deadline; keep brief>
```

**Critical:** List paths only. Subagents must **READ workspace files from disk** — never paste full `.research/*.md` contents into the handoff.

## Role → typical files

| Role | Read | Write |
|------|------|-------|
| research-lead | STORY, STATE, DISCOVERY (+ EXPERIMENTS index) | `.research/work/<slug>.md` |
| literature-scout | STORY, LITERATURE, DISCOVERY | `.research/work/<slug>.md` |
| experiment-agent | STORY, EXPERIMENTS, RESOURCES, DISCOVERY | `.research/work/<slug>.md` |
| result-analyst | EXPERIMENTS, STORY, DISCOVERY, raw results, work reports | `.research/work/<slug>.md` |
| reviewer | STORY, EXPERIMENTS, direct artifacts + active prompt | `.research/reviews/<EXP-ID>/` |

## Write rules

- Subagents deliver **only** to paths in `Required output`.
- Subagents **must not** edit canonical state: `PROJECT`, `STORY`, `STATE`, `DISCOVERY`, `EXPERIMENTS`, `LITERATURE`, `REVIEWS`, `RESOURCES`.
- Code changes happen in linked repos per `RESOURCES.md` and `git-linking.md`.
- Parallel subagents: use distinct `task-slug` values; never write the same work file.

## Reviewer dispatch

Add prompt path to handoff:

```text
Review prompt: .agents/prompts/method-review.md
# or
Review prompt: .agents/prompts/result-review.md
```

Fill Task fields inside that prompt (EXP-ID, Story gap, Relevant files, Required output).

## After subagent returns

1. Read the work file or review file from disk.
2. Integrate findings into the research loop (`research-loop` skill).
3. Update canonical state via appropriate skills, following
   [state-files.md](../references/state-files.md) §更新顺序.
   LITERATURE / REVIEWS are updated by Main Agent from scout/reviewer output.
4. Do not ask the subagent to patch state files retroactively.

## Anti-patterns

- Pasting STORY or EXPERIMENTS into chat instead of path pointers
- Multiple agents editing the same canonical file in one turn
- Skipping `result-analyst` when executor also interpreted results
- Treating reviewer approve/reject as sufficient without reading review files
