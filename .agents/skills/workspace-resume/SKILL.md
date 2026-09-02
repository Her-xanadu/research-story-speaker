---
name: workspace-resume
description: >-
  Quickly recover project context from durable workspace files and continue
  research without restating the project. Use when a new session starts, the
  user says continue/resume/接着做/恢复科研, an unfamiliar Agent opens the
  workspace, chat history is missing, cold-start after switching Harness, or
  PROJECT.md Project Status is UNINITIALIZED (first-time materialize).
---

# Workspace Resume

Thin bootstrap: reconstruct from disk, emit a compact packet, then **continue
in the same turn**. If Project Status is `UNINITIALIZED`, initialize first.
Definitions: [state-files.md](../../references/state-files.md),
[story-loop.md](../../references/story-loop.md),
[experiment-record.md](../../references/experiment-record.md).

## When to use

- First turn in a new chat or Harness; continue / resume / initialize / 接着研究.
- `.research/PROJECT.md` **Project Status** is `UNINITIALIZED`.
- No reliable chat history, but `.research/` exists; Agent handoff; context stale.
- Before `research-loop` when context is unknown or the project is not yet `ACTIVE`.

Not for file hygiene (`research-memory`) or route choice (`research-loop`) once
the project is `ACTIVE` and the next step is already clear.

## Goal

Within one turn, answer then act on:

```text
Current Story
Current Gap
Latest Relevant Evidence
Active Experiment
Recommended Next Action
```

`PROJECT + STORY + STATE` should let a stranger grasp position in minutes
(`AGENTS.md` Research Memory; [state-files.md](../../references/state-files.md)).

UNINITIALIZED: **materialize** the eight `.research/` files already on disk into
an `ACTIVE` project, then enter `research-loop`. Do not generate a new set.

## Default flow

### 0. Initialization (UNINITIALIZED only)

If `PROJECT.md` **Project Status** is `UNINITIALIZED`:

1. Collect the minimum: research goal, code/data locations, constraints. Ask
   **only** for missing items. Do not invent the rest.
2. **Materialize** the eight files already on disk — `PROJECT.md`, `STORY.md`,
   `STATE.md`, `DISCOVERY.md`, `EXPERIMENTS.md`, `LITERATURE.md`, `REVIEWS.md`,
   `RESOURCES.md` — from user input and `.agents/templates/`. Do not create a
   new eight-file set. EXPERIMENTS and REVIEWS stay empty (index only; no
   `EXP-` sections, no reviews).
3. Project Status → `ACTIVE`. STATE Story Status → `IN_PROGRESS`.
4. STORY six sections are **structure, not required facts**. Missing evidence:
   `_Not established yet._` or `Current hypothesis: ...` /
   `To be determined through literature and initial experiments.`
   **Never fabricate** Key Observation / Core Idea / Evidence. Inside the
   existing Key Observation section, tag `External premise` /
   `Internal observation` / `Current hypothesis` (no new headings).
5. Suggest a workspace git commit, then enter `research-loop` same turn.

If information is insufficient, stop after asking; do not write `ACTIVE` state.

### 1. Confirm workspace identity

Verify: `AGENTS.md`, `.research/PROJECT.md`, `.research/STORY.md`,
`.research/STATE.md`, `.agents/skills/`. Missing core files → concrete blocker;
do not invent state.

### 2. Load boot set

Read: `PROJECT.md` → `STORY.md` → `STATE.md`. On demand only: targeted
`DISCOVERY.md`, STATE-linked EXP sections
([experiment-record.md](../../references/experiment-record.md)), `RESOURCES.md`.
No full-scan of `LITERATURE.md` or all experiments at startup.

### 3. Reconstruct the five answers

| Answer | Sources |
| --- | --- |
| Current Story | `STORY.md`（六段见 `STORY.md` / `story-maintenance`） |
| Current Gap | Open Gaps, Boundary, `STATE` focus |
| Latest Relevant Evidence | `DISCOVERY`, EXP in STATE/STORY |
| Active Experiment | `STATE`, `running`/`planned` EXP |
| Recommended Next Action | `STATE` next or gap inference |

If STATE contradicts STORY or is bloated → note conflict; prefer
`research-memory` before large work. Just-initialized STORY may still say
`_Not established yet._` — report that honestly and route to `research-loop`.

### 4. Emit packet and continue

Report all five compactly, then **same turn**: execute `STATE` next or the
matching Skill; route unclear → `research-loop`; files disagree →
`research-memory` first; narrow command with a clear STATE anchor → that Skill.

Never end with only “已恢复” when an actionable step exists.

## Reads

**Always:** `PROJECT.md`, `STORY.md`, `STATE.md`, `AGENTS.md` (routing).

**On demand:** `DISCOVERY.md`, `EXPERIMENTS.md` (index + sections),
`RESOURCES.md`, `LITERATURE.md`, `REVIEWS.md`.

**Init:** `.agents/templates/` as the shape to materialize into existing files.

## Updates

Read-mostly after `ACTIVE`. During initialization, edit the eight existing
`.research/` files in place. EXPERIMENTS and REVIEWS stay empty. Otherwise:

- `RESOURCES.md` stale after successful re-location;
- `STATE.md` clearly wrong (non-trivial fixes → `research-memory`).

After substantive work, follow
[state-files.md](../../references/state-files.md) §更新顺序.

## Deviation allowed

- Skip optional reads when the boot set suffices; one EXP section instead of
  full `EXPERIMENTS.md`; start inside the active experiment when STATE and user
  agree; `research-loop` on explicit file-consistent tasks; init may leave STORY
  as `_Not established yet._`.

Do **not** guess Story, fabricate Key Observation / Core Idea / Evidence, replay
completed work, treat chat as evidence, load every state file “just in case,”
or create a second set of `.research/` files.
