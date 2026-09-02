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
Experiment fields and Outcome values:
[experiment-record.md](../../references/experiment-record.md).
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

Run success ≠ scientific success; `completed` Status does not mean hypothesis
confirmed. Set **Outcome** per [experiment-record.md](../../references/experiment-record.md)
§Outcome 值 (do not copy that table here).

## Default flow

1. **Gather evidence** — Read `EXP-xxx` in `.research/EXPERIMENTS.md` (Results, Runs,
   Git, Code, current Outcome). Inspect raw artifacts; do not rely only on executor
   summaries. Follow Git commit to code if method questions arise.
2. **Answer analysis questions** — Work through explicitly:
   - 发生了什么？（客观 Main Findings）
   - 结果是否可靠？（方差、泄漏、实现 bug、样本量）
   - 支持什么？（对 Story 哪一段有证据）
   - 不支持什么？（预期未出现、对照不利）
   - 是否存在替代解释？（混淆因素、选择偏差、度量问题）
   - Outcome 应是什么？（引用 experiment-record.md §Outcome 值）
   - 是否写入 DISCOVERY？（见下方规则，不是每次失败都写 Negative）
   - Story 是否需要改变？（小改 vs 核心机制动摇）
   - 下一实验是什么？（指向 `experiment-design` 或 supersede）
3. **Update EXPERIMENTS** — Main Findings, Interpretation, Discovery Impact,
   Story Impact, Next, **Outcome**; sync Index Status, **Index Outcome**, and
   Updated date.
4. **Update DISCOVERY** — Only when the EXP is scientifically usable:
   - `Status=failed` 且 `Outcome=not-assessed` → **不产生** Negative Discovery。
   - `Status=completed` 且 `Outcome=contradicts` 或 `null` → 写入 DISCOVERY
     （Negative / Null）。
   - `completed` + `supports` → Positive；`invalid` 不可用于推断，不当 Negative Discovery。
   Tag `Evidence: EXP-xxx`. Do not paste full experiment text.
5. **Update STORY if needed** — Small edits: `story-maintenance`. Large edits
   (Problem, Key Observation, Core Idea): `story-maintenance` and **suggest**
   `experiment-review` on the triggering EXP.
6. **Update STATE** — Current gap, active/next experiment, blockers, file pointers.
7. **Chain** — Clear next test → `experiment-design` or `experiment-execution`;
   contested evidence → `experiment-review`; routine compaction → `research-memory`.

Follow [state-files.md](../../references/state-files.md) §更新顺序.

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
| `.research/EXPERIMENTS.md` | Findings, Interpretation, Impacts, Next, Status, Outcome (section + Index) |
| `.research/DISCOVERY.md` | Scientific findings only, per Default flow step 4 |
| `.research/STORY.md` | When evidence warrants (via `story-maintenance` rules) |
| `.research/STATE.md` | Gap, next action, blockers |

Do **not** write Reviewer files here — use `experiment-review`.

## Deviation allowed

- Defer STORY edits if evidence is weak — note open contradiction in DISCOVERY instead
  (only when Outcome is a scientific finding, not technical failure).
- Skip numeric detail in Story; keep numbers in EXPERIMENTS only.
- Request `experiment-review` before large Story edits even when not mandatory.
- Trivial exploratory runs: merge with execution in one session — still fill
  Interpretation and Outcome in the record.
- Mark EXP `superseded` when a new EXP explicitly replaces the same scientific question.
- Retain every `EXP-xxx` section, including `failed` + `not-assessed`; do not delete
  valuable `contradicts` or `null` results.
- Core Idea overturned: record discovery, suggest Reviewer, revise Story, propose new loop.
