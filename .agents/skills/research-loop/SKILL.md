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

- Autonomous research should advance one iteration.
- `workspace-resume` done and route remains open.
- User asks next step, gap closure, or to keep going.
- New evidence arrived; cycle should continue.
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

Read `STORY.md` (six sections) and `STATE.md`. Unreadable or contradictory →
`research-memory` first.

### 2. Anti-duplication check

Scan: Open Gaps, STATE Next/active EXP, EXPERIMENTS index + sections,
DISCOVERY Negative/Invalidated/Contradictions. No redo of invalidated routes
without new mechanism. Merge duplicate parallel EXP when found.

### 3. Largest Story gap

Priority ([story-loop.md](../../references/story-loop.md)):

1. Would change Problem, Core Idea, or route viability.
2. Blocks Story completion (PROJECT + Boundary).
3. Verifiable with current RESOURCES.
4. Low-cost, high-information over sweeps.

One focal gap per iteration unless parallel subagents warranted.

### 4. Choose route

| Gap nature | Route | Delegate |
| --- | --- | --- |
| Prior work, novelty, lit conflict | Literature | `literature-research` / scout |
| Untested mechanism, empirical answer | Experiment | design → execution → analysis |
| High-stakes method, anomaly, big Story change | Review | `experiment-review` / reviewer |
| Wording only | Story | `story-maintenance` |

Combinations allowed — see story-loop reference.

### 5. Invoke and integrate

- Simple: run Skill in context.
- Parallel/heavy: Subagent per `AGENTS.md`; handoff via
  `.agents/prompts/subagent-handoff.md`.
- Executors use [experiment-record.md](../../references/experiment-record.md)
  and [git-linking.md](../../references/git-linking.md).

After evidence:

```text
EXPERIMENTS → DISCOVERY → STORY (story-maintenance) → STATE
```

Run success ≠ scientific success ([state-files.md](../../references/state-files.md)).

### 6. Continue, stagnate, or stop

Default: loop to step 1 if autonomy continues. Stop on user blocker, Story
completion per `PROJECT.md`, or Reviewer `ATTENTION_REQUIRED`.

Stagnation (no move on Problem/Core Idea/main gap): re-rank gap, abandon
low-value EXP, literature, Reviewer, `research-memory` — see story-loop reference.

## Reads

**Minimum each iteration:** `STORY.md`, `STATE.md`.

**As needed:** `PROJECT.md`, `DISCOVERY.md`, `EXPERIMENTS.md` (sections),
`LITERATURE.md`, `REVIEWS.md`, `RESOURCES.md`.

Load delegated Skill bodies at delegation time, not here.

## Updates

Does not own formats. Ensure executors updated:

```text
EXPERIMENTS, DISCOVERY, STORY (if needed), STATE
LITERATURE / REVIEWS when those routes ran
```

May lightly touch `STATE` (next focus) only before any executor runs.

## Deviation allowed

- User-specified Skill → re-enter at integrate.
- Parallel scout + experiment + reviewer for one gap.
- Defer Literature when cheap decisive experiment exists.
- Skip iteration after small Story tweak without new evidence.
- Pause for `research-memory` when routing blocked.
- Create/stop/reorder experiments; MCP; skip inapplicable steps.

Do **not** fixed state machine, hard-code EXP IDs, duplicate sub-Skill prose,
silently delete history, or advance Story without evidence.
