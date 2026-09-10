# Subagent Handoff

Main Agent: use this template when dispatching any subagent. Point the subagent at `.agents/subagents/<role>.md` for role rules.

## Handoff block (copy and fill)

```text
Role: <research-lead | literature-scout | experiment-agent | result-analyst | reviewer>
Model class: <workhorse | strongest>
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

`Model class` 必须与 `AGENTS.md` §模型分档一致：`reviewer` / `research-lead` / 高风险（full）`result-analyst` → `strongest`；`experiment-agent` / `literature-scout` / 普通（compact）`result-analyst` → `workhorse`。Main 自选具体模型，不得把 strongest 工作交给 fast 模型。

If **Decision This Task Can Change** cannot be answered, **do not dispatch**
unless the user explicitly asked for this task.

Each consult has **one** main decision. Reuse **Focal Scientific Question**,
**Decision This Task Can Change**, and **Return Condition**. Do not dispatch
several advisors for “read the whole project history and propose the final
method.” Narrow that request to the current decision and the limited evidence
it needs.

**Relevant files** point at the current relevant *paragraphs*, the original
evidence, and already-settled conclusions — not at a full history directory.
**Additional context** may add: premises still in dispute; work this task is
**not** responsible for.

### Two-tier product (every subagent)

A subagent produces **two** things at different detail levels — this is how the
main session stays clean while true depth lives in the sub-context:

1. **Detailed report on disk** — the full work under `Required output`
   (`.research/work/<slug>.md`, or `.research/reviews/<EXP-ID>/…` for reviewer).
   High-output work (code survey, logs, metric parsing, per-candidate fields,
   full diagnosis) lives **here**, in the sub-context — **not** in the return
   to Main. Do not enforce a fixed line budget on this report.

2. **Short decision summary to Main** — a few lines Main can act on without
   re-reading the whole report:

```text
Task: <role + EXP-ID / gap in one line>
Completion: complete | incomplete (why)
Finding: <main conclusion or recommended action, 1–3 lines>
Evidence pointer: <work-file path + the key artifact paths/metrics — not pasted dumps>
Limitation: <most critical uncertainty, or "none material">
Recommended next: <one most discriminating next experiment or analysis>
Proposed state changes: <which canonical files Main should touch — Main writes them>
```

Main reads the **summary**, integrates the decision, and opens the disk report
only on demand. The **next** subagent reads the upstream report **from disk**
(true context isolation) — Main does not paste it forward. Do not return the
whole report body to Main.

Do not make “review all history” the default prerequisite for proposing the
next experiment. A Main-Agent summary is not independent verification.
Local absolute paths are not assumed readable by an external Agent.

One consult defaults to **one trackable task**. Duration does not prove
depth and is not an anomaly by itself. If the body text or required work
artifact is missing, record the consult as **incomplete** — do not claim it
finished, do not invent progress, do not auto-redispatch the same question,
and do not silently downgrade a key judgment.

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

1. Read the work file or review file from disk. Missing body or required
   artifact → consult **incomplete**; keep the question unknown; do not
   invent progress.
2. Short Main integration **before adopting** (no new Reviewer): which
   advice is adopted; whether its key premises hold; which substitutions
   would change the question under test. A Main summary is not independent
   verification.
3. Integrate findings into the research loop (`research-loop` skill).
4. Update canonical state via appropriate skills, following
   [state-files.md](../references/state-files.md) §更新顺序.
   LITERATURE / REVIEWS are updated by Main Agent from scout/reviewer output.
5. Do not ask the subagent to patch state files retroactively.
   Do not auto-redispatch the same question. Do not silently downgrade a
   key judgment.

## Anti-patterns

- Pasting STORY or EXPERIMENTS into chat instead of path pointers
- Pointing Relevant files at a full history directory instead of the
  current paragraphs / original evidence / settled conclusions
- Multiple agents editing the same canonical file in one turn
- Dispatching a subagent that cannot name Decision This Task Can Change
- Dispatching several advisors because the user asked for a “final method”
  from the whole history
- Treating elapsed time as proof of depth, or as an anomaly
- Claiming the consult finished when no body or work artifact returned
- Skipping `result-analyst` on results that already have terminal artifacts. 默认由独立 result-analyst 承接结果解读（普通 compact / 高风险 full）；异常、高成本、核心 Story 相关、准备形成正式结论、或执行者有强烈既定解释 → full 模式。只有无终态产物 / 只看到中途 epoch 时不派。
- Treating reviewer approve/reject as sufficient without reading review files
