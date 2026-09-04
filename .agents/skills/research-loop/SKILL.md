---
name: research-loop
description: >-
  Top-level Story-driven research orchestrator. Use when continuing autonomous
  research, deciding the next scientific move, routing Literature vs Experiment
  vs Review, W1 FRAME reframing, or W4 DECIDE when next step is unclear. Skip
  when STATE is W2 TEST with a named ordinary EXP (inner loop). Triggers include
  下一步研究什么, 继续科研循环, run research loop, what should we do next.
---

# Research Loop

Highest-level orchestrator. **Schedules only** — delegate literature, execution,
and review to matching Skills or Subagents.

Two-layer Workflow: outer `W1` reframes Story/mechanism/route; inner
`W2→W3→W4→W2` runs same-Story experiments. Full rules:
[story-loop.md](../../references/story-loop.md).

References: `story-loop.md`, `state-files.md`, `experiment-record.md`
(open at delegation time, not as a boot set).
Route-time operators (not a boot set): `scientific-reasoning.md`.
Selective gates: `idea-evaluation`, `evidence-verification`.

## When to use

- Autonomous research should advance one iteration, or `workspace-resume` left
  the route open.
- User asks next step, gap closure, or to keep going; new evidence arrived.
- **W1 FRAME** or **W4 DECIDE** when next step, Story route, or Level 2 change
  is unclear.
- Parallel Literature + Experiment + Review fits one gap.

Not for cold start (`workspace-resume`), compaction (`research-memory`), or
**inner loop** when STATE is `W2 TEST` and Recommended Next Action already
names an ordinary sanity/exploratory EXP — use compact
`experiment-design` / `experiment-execution` / `result-analysis` instead.

## Goal

One iteration when FRAME or full DECIDE is needed:

```text
Read STATE Workflow Position + Story → gap or W4 four questions
→ Literature / Experiment / Review → invoke Skill → update memory → set next Position
```

Single current Story in `STORY.md`. Prefer experiments that **change judgment**,
not parameter sweeps ([story-loop.md](../../references/story-loop.md)).
Principle (lens only, not a STATE field):

```text
only investigate uncertainty that changes a decision
```

## Default flow

### 1. Read Position + Story

Read `STATE.md` **Workflow Position** first, then `STORY.md` (六段见
`story-maintenance`). Unreadable or contradictory → `research-memory` first.

### 2. Inner-loop bypass (critical)

If **Workflow Position is `W2 TEST`** and **Recommended Next Action** already
names a concrete ordinary sanity/exploratory EXP (or continues the current
mechanism-isolation line):

- **Stop this Skill.** Do not re-run「最大 gap」or full W1 FRAME.
- Route compact `experiment-design` → `experiment-execution` →
  `result-analysis` per [AGENTS.md](../../../AGENTS.md).
- After `result-analysis`, let compact W4 set next Position (usually stay
  `W2 TEST`).

If Position is **`W3 LEARN`** → delegate `result-analysis` only, then stop.

### 3. Anti-duplication check

Judge per [story-loop.md](../../references/story-loop.md) §反重复. This Skill
only routes.

### 4. FRAME: largest Story gap (W1)

When Position is `W1 FRAME`, `W4 DECIDE` with unclear next step, or stagnation
→ W1 per story-loop. Judge per §Gap 优先级. One focal gap per iteration unless
parallel subagents warranted.

### 5. Internal route stage

Pick **one** internally. Do **not** write these names into `STATE.md`.

```text
scout   — is there a signal worth continuing?
focus   — which mechanism actually produces the effect?
confirm — can results independently reproduce and support Story?
```

Use only to choose the next Skill under W1 FRAME. Not Protocol, Outcome,
Verdict, or Workflow Position.

### 6. Choose route

Selective — **not** a default chain. Ordinary exploratory EXP stays light.

| Gap nature | Route | Delegate |
| --- | --- | --- |
| Prior work, novelty, lit conflict | Literature | `literature-research` / scout |
| Untested mechanism, empirical answer | Experiment | `experiment-design` → `experiment-execution` → `result-analysis` |
| Major new idea (Core Idea, route competition, mechanism replacement, expensive successor) | Idea-gate | `idea-evaluation` ([prompt](../../prompts/idea-evaluation.md)); then `experiment-design` only if the gate says continue |
| High-stakes evidence (Story Evidence candidate, surprising strong result, Story-core change) | Evidence then Review | `evidence-verification` ([prompt](../../prompts/evidence-verification.md)) after `result-analysis` → `experiment-review` |
| High-stakes method, anomaly, big Story change | Review | `experiment-review` / reviewer |
| Wording only | Story | `story-maintenance` |

Ordinary exploratory EXP: `experiment-design` → `experiment-execution` →
`result-analysis` **without** `idea-evaluation`, `evidence-verification`,
`experiment-review` / reviewer, or `result-analyst` by default.

Parallel Experiment work: use `experiment-agent` / `result-analyst`; handoff via
[subagent-handoff.md](../../prompts/subagent-handoff.md).

### 7. Invoke and integrate

- Simple: run Skill in context. Parallel/heavy: Subagent per `AGENTS.md`.
- Independent next-step judgment: dispatch `research-lead` only when Position
  is not `W2 TEST` with a named next EXP. Optional:
  [next-research-move.md](../../prompts/next-research-move.md).
- After evidence, follow [state-files.md](../../references/state-files.md) §更新顺序.

### 8. W4 DECIDE + next Position

When this Skill owns the shift (Level 2, A/B/C back-to-W1, completion, or
unclear next step), answer story-loop §W4 四问:

```text
1. 结果可靠？
2. 改变对 Story 的相信？
3. 下一步同一科学问题？
4. 下一阶段？
```

Write **one** next Workflow Position in STATE:

| Judgment | Position |
| --- | --- |
| Story stable, next EXP clear | `W2 TEST` |
| Result unclear | `W3 LEARN` |
| Core Idea / gap / route needs reframe (A/B/C) | `W1 FRAME` |
| Story complete per PROJECT | `W5 HANDOFF` |

Do **not** force `W1→W2→W3→W4→W1` every Experiment.

Level 0/1 with clear Next may be handled by `result-analysis` / `story-maintenance`
without loading this Skill.

### 9. Continue, stagnate, or stop

Stop on user blocker, Story completion (`W5 HANDOFF`), or Reviewer
`ATTENTION_REQUIRED` ([reviewer.md](../../subagents/reviewer.md)).

Stagnation (no information gain, hyperparameter-only runs) → `W1 FRAME` per
[story-loop.md](../../references/story-loop.md) §停滞处理 / §何时回到 W1 B.

## Reads

**Minimum:** `STATE.md` (Workflow Position), `STORY.md`.

**As needed:** `PROJECT.md`, `DISCOVERY.md`, `EXPERIMENTS.md`, `LITERATURE.md`,
`REVIEWS.md`, `RESOURCES.md`. Load delegated Skills at delegation time.

## Updates

Ensure executors updated per [state-files.md](../../references/state-files.md)
§更新顺序. May set STATE Workflow Position and next focus when owning W4.

## Deviation allowed

- User-specified Skill → re-enter at integrate.
- Parallel scout + experiment + reviewer for one gap.
- Defer Literature when cheap decisive experiment exists.
- Skip iteration after Level 0 with no Story change.
- Pause for `research-memory` when routing blocked.
- Skip Idea-gate / Evidence-gate / Reviewer for ordinary exploratory EXP.

Do **not** use a fixed per-EXP state machine, hard-code EXP IDs, write
`scout`/`focus`/`confirm` into STATE, re-frame on every inner-loop pass,
or force every EXP through idea-eval → evidence → reviewer.
