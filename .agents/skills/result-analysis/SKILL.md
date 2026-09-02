---
name: result-analysis
description: >-
  Interpret experiment results: reliability, support or refutation, alternative
  explanations, discoveries, Story impact, and next experiments. Updates
  EXPERIMENTS, DISCOVERY, STORY when warranted, and STATE. Use after runs
  complete, when analyzing EXP-xxx outcomes, or separating interpretation from
  execution. Do not use for running code (experiment-execution) or independent
  review (experiment-review).
---

# Result Analysis

Thin Skill for **scientific interpretation** after artifacts exist.
Experiment fields: [experiment-record.md](../../references/experiment-record.md).
State roles and update order: [state-files.md](../../references/state-files.md).

## When to use

- `experiment-execution` finished and raw results exist for an `EXP-xxx`.
- You need interpretation separate from whoever ran the code (fresh judgment).
- Experiment failed, was null, or contradicted expectations — still analyze.
- `research-loop` integrated an experiment and needs DISCOVERY / Story updates.
- `result-analyst` subagent dispatched for supported interpretation and Story impact.

Do **not** use for: implementing or re-running (`experiment-execution`), adversarial
review (`experiment-review`), or cross-project file compaction (`research-memory`).

## Goal

Answer what the evidence means for the current Story, record durable discoveries,
and set the next research move. Analysis is **strong guidance**, not a hard gate —
but major Story changes should trigger or suggest `experiment-review`.

## Default flow

1. **Gather evidence** — Read `EXP-xxx` in `.research/EXPERIMENTS.md` (Results, Runs,
   Git, Code). Inspect raw artifacts (logs, plots, metrics) when available; do not rely
   only on executor summaries. Follow Git commit to code if method questions arise.
2. **Answer analysis questions** — Work through explicitly:
   - 发生了什么？（客观 Main Findings）
   - 结果是否可靠？（方差、泄漏、实现 bug、样本量）
   - 支持什么？（对 Story 哪一段有证据）
   - 不支持什么？（预期未出现、对照不利）
   - 是否存在替代解释？（混淆因素、选择偏差、度量问题）
   - 产生了什么新的 Discovery？（正/负/null/失效路线）
   - Story 是否需要改变？（小改 vs 核心机制动摇）
   - 下一实验是什么？（指向 `experiment-design` 或 supersede）
3. **Update EXPERIMENTS** — Main Findings, Interpretation, Discovery Impact,
   Story Impact, Next; sync Index Status (`completed` / `failed` / `abandoned`) and
   Updated date. See [experiment-record.md](../../references/experiment-record.md).
4. **Update DISCOVERY** — Add findings under Positive / Negative / Null /
   Invalidated / Open Contradictions with `Evidence: EXP-xxx`. Explain why the Story
   view shifted if it did. Do not paste full experiment text — link by EXP-ID.
5. **Update STORY if needed** — Small edits (Evidence, Boundary, Open Gaps): follow
   `story-maintenance`. Large edits (Problem, Key Observation, Core Idea): use
   `story-maintenance` and **suggest** `experiment-review` on the triggering EXP.
6. **Update STATE** — Current gap, active/next experiment, blockers, file pointers.
7. **Chain** — Clear next test → `experiment-design` or `experiment-execution`;
   contested evidence → `experiment-review`; routine compaction → `research-memory`.

Default update order ([state-files.md](../../references/state-files.md)):

```text
EXPERIMENTS → DISCOVERY → STORY (if needed) → STATE
```

## Reads

| Priority | Files |
|----------|-------|
| Required | `.research/EXPERIMENTS.md` (`EXP-xxx`), `.research/STORY.md`, raw artifacts |
| Often | `.research/DISCOVERY.md`, `.research/STATE.md`, `.research/PROJECT.md` |
| Reference | [experiment-record.md](../../references/experiment-record.md), [state-files.md](../../references/state-files.md), [story-loop.md](../../references/story-loop.md) |
| Subagent | [result-analyst.md](../../subagents/result-analyst.md) |

## Updates

| File | What to update |
|------|----------------|
| `.research/EXPERIMENTS.md` | Main Findings, Interpretation, Discovery Impact, Story Impact, Next, Status |
| `.research/DISCOVERY.md` | New or revised scientific findings |
| `.research/STORY.md` | When evidence warrants (via `story-maintenance` rules) |
| `.research/STATE.md` | Gap, next action, blockers |

Do **not** write Reviewer files here — use `experiment-review`.

## Deviation allowed

- Defer STORY edits if evidence is weak — note open contradiction in DISCOVERY instead.
- Skip numeric detail in Story; keep numbers in EXPERIMENTS only
  ([state-files.md](../../references/state-files.md) anti-duplication).
- Request `experiment-review` before large Story edits even when not mandatory.
- Trivial exploratory runs: merge with execution in one session — still fill separate
  Interpretation fields in the record.
- Mark EXP `superseded` when a new EXP explicitly replaces the same scientific question.

## Boundaries

- Run success ≠ scientific success; `completed` does not mean hypothesis confirmed.
- Failed experiments: retain full `EXP-xxx`; add negative discovery; shrink Story claims
  if warranted.
- Core Idea overturned: record discovery, suggest Reviewer, revise Story, propose new loop.
- Do not delete valuable negative or null results.
- Reviewer artifacts live under `.research/reviews/` — not in this skill.
