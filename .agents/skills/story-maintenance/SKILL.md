---
name: story-maintenance
description: >-
  Update STORY.md six sections from DISCOVERY, Reviewer feedback, and experiment
  evidence. Use when Evidence, Boundary, or Open Gaps change after
  result-analysis, experiment-review, or reconciling Story with DISCOVERY. No
  performance numbers in Story; large changes to Problem, Key Observation, or
  Core Idea should suggest Reviewer involvement. Do not use for initial Story
  drafting, literature search, or file compaction (research-memory).
---

# Story Maintenance

Thin Skill for keeping `.research/STORY.md` aligned with current scientific
belief. Authoritative semantics: [state-files.md](../../references/state-files.md)
§ STORY.md. Loop and gap context: [story-loop.md](../../references/story-loop.md).
On large Story changes:
[idea-and-mechanism.md](../../references/research-intelligence/idea-and-mechanism.md),
[story-audit.md](../../prompts/story-audit.md).

## When to use

- `result-analysis` or `experiment-review` concluded with Story impact fields.
- New `DISCOVERY.md` entries change what we believe, not just what we ran.
- `Open Gaps`, `Evidence`, or `Boundary` need revision after an `EXP-xxx`.
- Reviewer output in `.research/reviews/EXP-xxx/` recommends Story edits.
- `research-memory` flagged Story–evidence drift or contradictory sections.
- `research-loop` routed here because belief changed but wording is the blocker.

Do **not** use for: external paper search (`literature-research`), designing or
running experiments, first-pass result interpretation (`result-analysis`), or
compressing state files (`research-memory`).

## Goal

Maintain **one** current Story in six fixed sections (~one page, **no
performance numbers**). Reflect durable scientific belief, not experiment logs.

```text
Problem → Key Observation → Core Idea → Evidence → Boundary → Open Gaps
```

**Frozen vs mutable** (see [AGENTS.md](../../../AGENTS.md) Main 三条常驻规则):

| Frozen | Mutable |
|--------|---------|
| `PROJECT.md` Research Goal | Key Observation |
| The program-level aim in STORY `Problem` (针对什么、完成一类什么研究) | Core Idea (which method / combination) |
| User-authorized goal only | Evidence / Boundary / Open Gaps |

Do **not** replace the research goal with a different project. Methods and
findings may change; the Goal does not.

Core Idea 可写成「当前正在检验的新假设」，不必等成功。更新假设 ≠ 提升
Evidence。大改仍是 Level 2；不能把大改藏成 Level 1。旧机制已被替代时，
更新当前假设表述，不得写成已证明机制
([state-files.md](../../references/state-files.md) §STORY.md / §DISCOVERY.md)。

Classify every edit (**Story Impact Level**, [story-loop.md](../../references/story-loop.md)):

| Level | Sections | Agent action | Next Workflow Position |
| --- | --- | --- | --- |
| **0** | None — do not edit STORY | N/A (`result-analysis` only) | `W2 TEST` if Next clear |
| **1** | Evidence, Boundary, Open Gaps, wording | Apply directly (was Small) | `W2 TEST` if Next clear |
| **2** | Key Observation / Core Idea（方法可变）；不得改 PROJECT Goal | 谨慎改；建议 Reviewer | `W1 FRAME` |

## Default flow

1. **Load context** — Read `.research/STORY.md`, relevant `DISCOVERY.md`
   blocks, triggering `EXP-xxx` in `.research/EXPERIMENTS.md` (Interpretation /
   Story Impact only), and any linked review under `.research/reviews/EXP-xxx/`.
2. **Classify change size** — Use table above. If large and stakes are high,
   recommend `experiment-review` **before** rewriting core mechanism.
   On large changes, cite
   [idea-and-mechanism.md](../../references/research-intelligence/idea-and-mechanism.md)
   and [story-audit.md](../../prompts/story-audit.md). Story must not grow more
   complex merely to rescue a method. Check story creep, mechanism creep,
   post-hoc explanation, and claim expansion.
3. **Draft section edits** — Update only sections evidence warrants. In
   `Evidence`, cite `EXP-xxx` with mechanism-level conclusions; never paste runs,
   tables, or metrics.
4. **Sharpen gaps** — Tighten `Boundary` when claims shrink; rewrite `Open Gaps`
   to the weakest link the loop should attack next (per
   [story-loop.md](../../references/story-loop.md) gap priority).
5. **Apply to STORY.md** — Single Story only; no parallel competing narratives.
6. **Update STATE** — Focus, gap, next action, and **Workflow Position**
   (`W2 TEST` after Level 1 with clear Next; `W1 FRAME` after Level 2).
7. **Hand off** — Level 1 + clear Next → `experiment-design` (inner loop).
   Level 2 or reframed gap → `research-loop`. Literature only when W1 needs it.

Follow [state-files.md](../../references/state-files.md) §更新顺序; this Skill
owns the STORY ring.

## Reads

| Priority | Files |
| --- | --- |
| Required | `.research/STORY.md` |
| Often | `.research/DISCOVERY.md` (target sections), `.research/EXPERIMENTS.md` (`EXP-xxx` Story Impact / Interpretation), `.research/STATE.md` |
| Sometimes | `.research/reviews/EXP-xxx/*.md`, `.research/PROJECT.md` (scope check), `.research/REVIEWS.md` (summary) |
| Reference only | [state-files.md](../../references/state-files.md), [story-loop.md](../../references/story-loop.md), [idea-and-mechanism.md](../../references/research-intelligence/idea-and-mechanism.md) |
| Large-change prompt | [story-audit.md](../../prompts/story-audit.md) |

Do **not** load full experiment sections or chat history as belief sources.

## Updates

| File | What to update |
| --- | --- |
| `.research/STORY.md` | One or more of the six sections |
| `.research/STATE.md` | Current gap, next action, blockers (brief) |

Do **not** update `EXPERIMENTS.md`, `DISCOVERY.md`, or `LITERATURE.md` here —
those are upstream or sibling Skills.

Anti-duplication rules: [state-files.md](../../references/state-files.md) §
反重复规则. Story completion criteria: same file § Story 完成条件.

## Deviation allowed

- Touch only `Open Gaps` when the rest of the Story is stable.
- Defer Story edits if evidence is contested — record tension in `DISCOVERY.md`
  and leave Story unchanged until `result-analysis` or Review resolves it.
- Invoke `experiment-review` **before** applying a large change when cost or
  stakes are high (strong guidance, not a hard lock).
- Merge small post-review tweaks in the same session as `result-analysis` when
  the analyst already wrote Story Impact fields.
- Skip `STATE` update when Story wording changed but focus and next action did not.

Hard boundaries (not deviations):

- **No numbers in Story** — metrics stay in `EXPERIMENTS.md` only.
- **No experiment dumps** — cite `EXP-xxx`; do not copy Method, Runs, or Results.
- **No multi-Story** — one current belief state per project.
- **Research Goal frozen** — do not rewrite `PROJECT.md` Research Goal or the
  matching aim sentence in `Problem` unless the user amends the goal.
- **Large mechanism changes** — reframing `Core Idea` / Key Observation should trigger
  or suggest Review; see [story-loop.md](../../references/story-loop.md) §停滞处理.
  Story must not grow more complex merely to rescue a method
  ([story-audit.md](../../prompts/story-audit.md);
  [idea-and-mechanism.md](../../references/research-intelligence/idea-and-mechanism.md)).
  Changing method ≠ changing the research goal. Do not hide a Level 2
  rewrite as Level 1 wording.
