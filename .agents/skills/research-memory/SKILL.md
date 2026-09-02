---
name: research-memory
description: >-
  Maintain consistency and readability of the 8 research state files.
  Use after long autonomous runs, when files are bloated or contradictory,
  when a new Agent cannot parse project state, when EXPERIMENTS and DISCOVERY
  duplicate, STATE is stale, or Story drifts from evidence. Triggers include
  整理科研状态, 压缩 STATE, 修复状态文件, clean up research memory.
---

# Research Memory

Hygiene and reconciliation for durable memory. Does not replace
`story-maintenance`, `result-analysis`, or Reviewers.

References: [state-files.md](../../references/state-files.md),
[experiment-record.md](../../references/experiment-record.md),
[story-loop.md](../../references/story-loop.md),
[git-linking.md](../../references/git-linking.md).

## When to use

- Long run left partial updates; STATE ≠ STORY or evidence.
- EXPERIMENTS and DISCOVERY duplicate narratives.
- STORY > ~one page or embeds numbers; routing fails.
- Broken EXP paths, RESOURCES links, review index drift.
- `workspace-resume` / `research-loop` blocked by file chaos.

Not for routine post-experiment updates (`result-analysis`) or next-step
choice (`research-loop`).

## Goal

Restore single source of truth ([state-files.md](../../references/state-files.md) §20)
**without deleting** negative, null, or invalidated results:

```text
PROJECT | STORY | STATE | DISCOVERY | EXPERIMENTS | LITERATURE | REVIEWS | RESOURCES
```

Preserve: EXPERIMENTS = what happened; DISCOVERY = what we learned;
STORY = what we believe. Chat is never a fact source.

## Default flow

### 1. Diagnose and plan

Read `PROJECT`, `STORY`, `STATE`; then symptomatic files. Note duplication,
stale pointers, STORY detail in wrong file, broken RESOURCES, REVIEWS drift.
Write a short edit plan before touching files. No science by compression.

### 2. Compress STATE and STORY

**STATE** — dozens of lines: focus, active `EXP-xxx`, recent completions
(pointers), next action, blockers, file pointers. No history log.

**STORY** — 六段见 `STORY.md` / `story-maintenance`; ~one page, no numbers.
Move detail to DISCOVERY or EXPERIMENTS. Problem / Core Idea changes →
`story-maintenance` + Reviewer.

### 3. Organize DISCOVERY

Sections per [state-files.md](../../references/state-files.md): Understanding,
Positive/Negative/Null/Invalidated, Contradictions, Evolution. Merge dupes;
keep distinct negatives. `Evidence: EXP-xxx` not experiment paste. No debug
noise (paths, syntax).

### 4. Tighten EXPERIMENTS (no deletion)

Every `EXP-xxx` section retained per
[experiment-record.md](../../references/experiment-record.md). Compress old
prose; refresh index Status/Updated. Never remove failed, abandoned, or
superseded ledger entries.

### 5. Fix RESOURCES and indexes

Refresh pointers per [git-linking.md](../../references/git-linking.md); re-locate
moved repos. Align LITERATURE and `REVIEWS.md` with `.research/reviews/EXP-xxx/`.

### 6. Verify; rewrite STATE last

Valid DISCOVERY cites; STATE EXP IDs exist; reviews match index. Final STATE
so `workspace-resume` and `research-loop` can route immediately.

## Reads

**Always:** `PROJECT.md`, `STORY.md`, `STATE.md`.

**Typically:** `DISCOVERY.md`, `EXPERIMENTS.md`, `RESOURCES.md`.

**Symptomatic:** `LITERATURE.md`, `REVIEWS.md`, `.research/reviews/**/*`.

## Updates

May edit all 8 `.research/` files. Prefer move/compress/cross-reference over delete.
压缩/搬移细节，不改 Problem/Core Idea 的科学主张.

**Never delete:** negatives/nulls, any `EXP-xxx` section, review artifacts,
invalidated findings (mark in DISCOVERY).

**Defer:** Story mechanism → `story-maintenance`; interpretation →
`result-analysis`; new runs → `experiment-execution`.

If new facts emerged during cleanup, follow
[state-files.md](../../references/state-files.md) §更新顺序.

## Deviation allowed

- Quick fix: `STATE` + `RESOURCES` only when rest healthy.
- Standing hygiene every N iterations if user requests.
- STORY handoff to `story-maintenance`.
- Compress old EXPERIMENTS body while keeping index rows.

Do not drop EXP sections, erase failed runs, invent evidence, or paste long
experiment text into DISCOVERY or STORY.
