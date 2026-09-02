---
name: workspace-resume
description: >-
  Quickly recover project context from durable workspace files and continue
  research without restating the project. Use when a new session starts, the
  user says continue/resume/接着做/恢复科研, an unfamiliar Agent opens the
  workspace, chat history is missing, or cold-start after switching Harness.
---

# Workspace Resume

Thin bootstrap for a fresh Agent or session. Reconstruct context from disk,
report a compact recovery packet, then **continue executing** in the same turn.

Authoritative definitions: [state-files.md](../../references/state-files.md),
[story-loop.md](../../references/story-loop.md),
[experiment-record.md](../../references/experiment-record.md).

## When to use

- First turn in a new chat or Harness.
- User asks to continue, resume, pick up, or 接着研究.
- No reliable chat history, but `.research/` exists.
- Handoff to another Agent without re-reading the whole project.
- Before `research-loop` when context is unknown or stale.

Not for file hygiene (`research-memory`) or route choice (`research-loop`).

## Goal

Within one turn, answer then act on:

```text
Current Story
Current Gap
Latest Relevant Evidence
Active Experiment
Recommended Next Action
```

Chat history is not research memory — workspace files are (`AGENTS.md`).
`PROJECT + STORY + STATE` should let a stranger grasp position in minutes.

## Default flow

### 1. Confirm workspace identity

Verify: `AGENTS.md`, `.research/PROJECT.md`, `.research/STORY.md`,
`.research/STATE.md`, `.agents/skills/`. Missing core files → concrete blocker;
do not invent state.

### 2. Load boot set

Read in order: `PROJECT.md` → `STORY.md` → `STATE.md`.

On demand only: targeted `DISCOVERY.md`, STATE-linked EXP sections (see
[experiment-record.md](../../references/experiment-record.md)), `RESOURCES.md`.
No full-scan of `LITERATURE.md` or all experiments at startup.

### 3. Reconstruct the five answers

| Answer | Sources |
| --- | --- |
| Current Story | `STORY.md` six sections |
| Current Gap | Open Gaps, Boundary, `STATE` focus |
| Latest Relevant Evidence | `DISCOVERY`, EXP in STATE/STORY |
| Active Experiment | `STATE`, `running`/`planned` EXP |
| Recommended Next Action | `STATE` next or gap inference |

If STATE contradicts STORY or is bloated → note conflict; prefer
`research-memory` before large work.

### 4. Emit packet and continue

Report all five compactly, then **same turn**:

- execute `STATE` next step or matching Skill;
- route unclear → `research-loop`;
- files disagree → `research-memory` first;
- narrow user command with clear STATE anchor → dedicated Skill directly.

Never end with only “已恢复” when an actionable step exists.

## Reads

**Always:** `PROJECT.md`, `STORY.md`, `STATE.md`, `AGENTS.md` (routing).

**On demand:** `DISCOVERY.md`, `EXPERIMENTS.md` (index + sections),
`RESOURCES.md`, `LITERATURE.md`, `REVIEWS.md`.

## Updates

Read-mostly. Update only when:

- `RESOURCES.md` stale after successful re-location;
- `STATE.md` clearly wrong (non-trivial fixes → `research-memory`).

After substantive work, chain per [state-files.md](../../references/state-files.md):

```text
EXPERIMENTS → DISCOVERY → STORY (if needed) → STATE
```

## Deviation allowed

- Skip optional reads when boot set suffices.
- One EXP section instead of full `EXPERIMENTS.md`.
- Start inside active experiment when STATE and user agree.
- `research-loop` on explicit file-consistent tasks.

Do **not** guess Story, replay completed work, treat chat as evidence, or load
every state file “just in case.”
