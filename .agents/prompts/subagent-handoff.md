# Subagent Handoff

Main Agent: use this template when dispatching any subagent. Point the subagent at `.agents/subagents/<role>.md` for role rules.

## Handoff block (copy and fill)

```text
Role: <research-lead | literature-scout | experiment-agent | result-analyst | reviewer>
EXP-ID: <e.g. EXP-031, or N/A for research-lead / literature-scout>
Story gap: <one sentence — STORY Open Gap or Boundary item>
Focal Scientific Question:
  <the one method question this task serves>
Decision This Task Can Change:
  <which scientific / method judgment moves if the task succeeds>
Return Condition:
  <when to stop and return — tests pass / artifact produced / diagnosis written>
Relevant files:
  - .research/STORY.md
  - <other paths the subagent must read>
Required output:
  - .research/work/<task-slug>.md
  - OR .research/reviews/<EXP-ID>/<method|result>-review-r<N>.md (reviewer only)
Task slug: <short-kebab-name for work file>
Additional context: <optional — hypotheses, constraints, deadline; keep brief>
```

If **Decision This Task Can Change** cannot be answered, **do not dispatch**
unless the user explicitly asked for this task.

**Critical:** List paths only. Subagents must **READ workspace files from disk** — never paste full `.research/*.md` contents into the handoff.

`EXP-ID: NEW` 只用于设计提案。真正开始改代码或执行前，必须由 Main Agent 在 EXPERIMENTS.md 分配/预留具体 EXP-ID 后再 dispatch experiment-agent。

## Role → typical files

| Role | Read | Write |
|------|------|-------|
| research-lead | STORY, STATE, DISCOVERY (+ EXPERIMENTS index) | `.research/work/<slug>.md` |
| literature-scout | STORY, LITERATURE, DISCOVERY | `.research/work/<slug>.md` |
| experiment-agent | STORY, EXPERIMENTS, RESOURCES, DISCOVERY | `.research/work/<slug>.md` |
| result-analyst | EXPERIMENTS, STORY, DISCOVERY, raw results, work reports | `.research/work/<slug>.md` |
| reviewer | STORY, EXPERIMENTS, direct artifacts + active prompt | `.research/reviews/<EXP-ID>/` |

## Layer 2 / task prompt by role

Pointer only — not an orchestrator. Load `.agents/subagents/<role>.md` first,
then **this dispatch's** task prompt. Load Layer 2 when that prompt says so.
Do **not** load every file under `research-intelligence/` on every handoff.
When a task prompt is attached and it disagrees with
`.agents/subagents/<role>.md` on work-artifact **shape** (Required output
headings), the **task prompt** wins for this dispatch. The role file still
wins on **write permissions** (work/reviews only; Main owns the canonical
eight).

| Role | Task prompt | Layer 2 (as relevant) |
|------|-------------|----------------------|
| research-lead | [next-research-move.md](next-research-move.md) | [scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md) |
| literature-scout | [literature-synthesis.md](literature-synthesis.md) | [deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md) **when requested** (novelty / conflict / field convention) |
| experiment-agent | [experiment-proposal.md](experiment-proposal.md), [failure-diagnosis.md](failure-diagnosis.md) | [experiment-thinking.md](../references/research-intelligence/experiment-thinking.md) |
| result-analyst | [result-diagnosis.md](result-diagnosis.md) | [evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md), [scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md) |
| reviewer | [method-review.md](method-review.md) **or** [result-review.md](result-review.md) | [scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md), [evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md); [idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md) when the claim is a mechanism |

Reviewer: pick **one** active review prompt. Progressive disclosure — method
review loads idea-and-mechanism + experiment-thinking only as that prompt
says; result review loads evidence-and-claim + scientific-reasoning (and
idea-and-mechanism only for a mechanism claim). Do not dump all intelligence
files into the reviewer context.

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
- Dispatching a subagent that cannot name Decision This Task Can Change
- Skipping independent `result-analyst` on high-risk results. 对异常、高成本、核心 Story 相关、准备形成正式结论、或执行者有强烈既定解释的结果，优先独立 result-analyst；普通探索允许执行和初步分析由同一 Agent 完成。
- Treating reviewer approve/reject as sufficient without reading review files
