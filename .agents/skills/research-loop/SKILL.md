---
name: research-loop
description: >-
  Top-level Story-driven research orchestrator. Use when continuing autonomous
  research, deciding the next scientific move, routing Literature vs Experiment
  vs Review, or after workspace-resume when the next action is not already
  fixed. Triggers include 下一步研究什么, 继续科研循环, run research loop,
  what should we do next.
---

# Research Loop

Highest-level orchestrator. **Schedules only** — delegate literature, execution,
and review to matching Skills or Subagents.

References: [story-loop.md](../../references/story-loop.md),
[state-files.md](../../references/state-files.md),
[experiment-record.md](../../references/experiment-record.md).

## When to use

- Autonomous research should advance one iteration, or `workspace-resume` left
  the route open.
- User asks next step, gap closure, or to keep going; new evidence arrived.
- Parallel Literature + Experiment + Review fits one gap.

Not for cold start (`workspace-resume`), compaction (`research-memory`), or
obvious single-Skill tasks.

## Goal

One iteration:

```text
Read Story → largest gap → Literature / Experiment / Review
→ invoke Skill/Subagent → new evidence → update memory → continue or stop
```

Single current Story in `STORY.md`. Prefer experiments that **change judgment**,
not parameter sweeps ([story-loop.md](../../references/story-loop.md)).

## Default flow

### 1. Anchor on Story

Read `STORY.md` (六段见 `STORY.md` / `story-maintenance`) and `STATE.md`.
Unreadable or contradictory → `research-memory` first.

### 2. Anti-duplication check

Judge per [story-loop.md](../../references/story-loop.md) §反重复. This Skill
only routes.

### 3. Largest Story gap

Judge per [story-loop.md](../../references/story-loop.md) §Gap 优先级. One focal
gap per iteration unless parallel subagents warranted.

### 4. Choose route

| Gap nature | Route | Delegate |
| --- | --- | --- |
| Prior work, novelty, lit conflict | Literature | `literature-research` / scout |
| Untested mechanism, empirical answer | Experiment | `experiment-design` → `experiment-execution` → `result-analysis` |
| High-stakes method, anomaly, big Story change | Review | `experiment-review` / reviewer |
| Wording only | Story | `story-maintenance` |

Parallel Experiment work: use `experiment-agent` / `result-analyst`; handoff via
[subagent-handoff.md](../../prompts/subagent-handoff.md). Do not expand those
Skills' flows here. Combinations allowed — see story-loop reference.

### 5. Invoke and integrate

- Simple: run Skill in context. Parallel/heavy: Subagent per `AGENTS.md`;
  handoff via [subagent-handoff.md](../../prompts/subagent-handoff.md).
- Independent next-step judgment: dispatch `research-lead` (reads STORY / STATE /
  DISCOVERY, writes `.research/work/` only).
- Executors use [experiment-record.md](../../references/experiment-record.md)
  and [git-linking.md](../../references/git-linking.md).

After evidence, follow [state-files.md](../../references/state-files.md) §更新顺序.

### 6. Continue, stagnate, or stop

If still advancing autonomously, return to gap judgment; skipping steps is
allowed. Stop on user blocker, Story completion per `PROJECT.md`, or Reviewer
control signal `ATTENTION_REQUIRED` (Use reviewer.md §Verdict;
[reviewer.md](../../subagents/reviewer.md) — do not recopy the Verdict list).

Stagnation signals (no move on Problem / Core Idea / main gap) →
[story-loop.md](../../references/story-loop.md) §停滞处理.

## Reads

**Minimum each iteration:** `STORY.md`, `STATE.md`.

**As needed:** `PROJECT.md`, `DISCOVERY.md`, `EXPERIMENTS.md` (sections),
`LITERATURE.md`, `REVIEWS.md`, `RESOURCES.md`. Load delegated Skill bodies at
delegation time, not here.

## Updates

Does not own formats. Ensure executors updated per
[state-files.md](../../references/state-files.md) §更新顺序, plus
`LITERATURE` / `REVIEWS` when those routes ran. May lightly touch `STATE` (next
focus) only before any executor runs.

## Deviation allowed

- User-specified Skill → re-enter at integrate.
- Parallel scout + experiment + reviewer for one gap.
- Defer Literature when cheap decisive experiment exists.
- Skip iteration after small Story tweak without new evidence.
- Pause for `research-memory` when routing blocked; or dispatch `research-lead`
  (read STORY / STATE / DISCOVERY; write `.research/work/` only).
- Create/stop/reorder experiments; MCP; skip inapplicable steps.

Do **not** use a fixed state machine, hard-code EXP IDs, duplicate sub-Skill
prose, silently delete history, or advance Story without evidence.
