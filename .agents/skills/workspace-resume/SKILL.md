---
name: workspace-resume
description: >-
  Quickly recover project context from durable workspace files and continue
  research without restating the project. Use when PROJECT.md Project Status
  is UNINITIALIZED (first-time materialize), or an ACTIVE session whose STATE
  does not already name the next EXP. Also use when STATE says running but
  terminal artifacts for that EXP already exist. Use when the user says
  continue/resume/接着做/恢复科研 and context is unknown. Do not use when PROJECT
  is ACTIVE and STATE Recommended Next Action is already an ordinary sanity
  or exploratory EXP whose premises still hold — use compact experiment-design
  and compact result-analysis instead (monitor-experiment if that EXP is already
  running and has no terminal artifact).
---

# Workspace Resume

Thin bootstrap: reconstruct from disk, emit a compact packet, then **continue
in the same turn**. If Project Status is `UNINITIALIZED`, initialize first.
Definitions: [state-files.md](../../references/state-files.md),
[story-loop.md](../../references/story-loop.md),
[experiment-record.md](../../references/experiment-record.md).

## When to use

- First turn in a new chat or Harness **only if** STATE does not already name
  an ordinary sanity / exploratory next EXP; continue / resume / initialize /
  接着研究; context unknown.
- `.research/PROJECT.md` **Project Status** is `UNINITIALIZED`.
- No reliable chat history, but `.research/` exists; Agent handoff; context stale.
- Before `research-loop` when context is unknown or the project is not yet `ACTIVE`.

Not for file hygiene (`research-memory`) or route choice (`research-loop`) once
the project is `ACTIVE` and the next step is already clear **and its premises
still hold**. Not for an `ACTIVE` ordinary sanity / exploratory EXP — use
compact `experiment-design` / `experiment-execution` / `result-analysis`. If
that EXP is already `running` with **no** terminal artifact, use
`monitor-experiment` instead of re-framing. If STATE says `running` but
terminal artifacts are already complete: this Skill **does** apply — fix the
cursor and go to `result-analysis` at `W3 LEARN`; do not re-launch; do not
wait on a finished job. File exists ≠ terminal
([state-files.md](../../references/state-files.md) §更新顺序).

## Goal

Within one turn, answer then act on:

```text
Workflow Position
Current Story
Current Gap
Active Experiment
Recommended Next Action
```

Two-layer Workflow: if Position is `W2 TEST` and Next names an ordinary EXP
**and that Next's premises still hold**, continue the **inner loop** (compact
design/execution/monitor/analysis) — **do not** load `research-loop` to
re-frame. If new evidence invalidates those premises, re-routing is allowed
([story-loop.md](../../references/story-loop.md) §方法转移的失败解释).
If STATE Status=`running` but this run's terminal artifacts are complete:
do **not** re-launch or monitor; hand to `result-analysis`, Position
`W3 LEARN` until analysis exists. See
[state-files.md](../../references/state-files.md) §更新顺序.

`PROJECT + STORY + STATE` should let a stranger grasp position in minutes
(`AGENTS.md` Research Memory; [state-files.md](../../references/state-files.md)).

UNINITIALIZED: **materialize** the eight `.research/` files already on disk into
an `ACTIVE` project; set Workflow Position `W1 FRAME` (Story Status
`IN_PROGRESS`). Then route: `W2 TEST` if Next already names an EXP, else
`research-loop` for FRAME only — do not generate a new file set.

## Decision Frontier

Ask the user **only** for:

```text
UNINITIALIZED (research goal — after workspace-setup)
Goal amendment
major evaluation change
resource authorization change → workspace-setup
```

Triage:

```text
facts → Agent looks up
scientific unknown → Experiment / Literature
user preference / authorization → Ask user
```

When asking, batch then stop:

```text
current frontier
recommended default
reason
```

Stop after key decisions are known. Do **not** grill every `research-loop`
turn. An `ACTIVE` resume with a clear STATE next is not a frontier.

## Default flow

### 0. Initialization (UNINITIALIZED only)

If `PROJECT.md` **Project Status** is `UNINITIALIZED`:

0. **Resource setup first.** Run [workspace-setup](../workspace-setup/SKILL.md)
   unless `.research/RESOURCES.md` already passes its **Ready check** (Compute +
   primary Codebase + External Capabilities). If Compute or code Git layout is
   missing, stop after `workspace-setup`; do **not** materialize `ACTIVE`.
1. Collect the minimum: research goal, remaining data/constraints. Ask **only**
   for missing items (`UNINITIALIZED` is a Decision Frontier). Code/compute
   layout is **not** repeated here — `workspace-setup` owns RESOURCES fields.
   Do not invent the rest.
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

### 2. Load scientific working set

Read, then **stop** and start work:

```text
PROJECT → STORY → STATE → STATE 指向的当前 EXP section
→ 当前 EXP 指向的最新 result/work artifact
```

On demand only: targeted `DISCOVERY.md` sections (especially **Current
Scientific Understanding** when premises matter), STATE-linked EXP
sections ([experiment-record.md](../../references/experiment-record.md)),
`RESOURCES.md`. **Do not** load entire `EXPERIMENTS.md`. **Do not** scan
`.research/work/`. **Do not** read all REVIEWS or historical DISCOVERY.
Follow pointers, not directories. History uses EXP-ID / keyword /
method-name lookup. Expand EXPERIMENTS / DISCOVERY / LITERATURE only on
true `W1 FRAME` reframe. File exists ≠ terminal; confirm the pointer
against this run's direct evidence before treating a job as finished or
still running.

### 3. Reconstruct the packet

| Answer | Sources |
| --- | --- |
| Workflow Position | `STATE.md` §Workflow Position |
| Current Story | `STORY.md`（六段） |
| Current Gap | Open Gaps, Boundary, `STATE` focus |
| Active Experiment | `STATE`, `running`/`planned` EXP |
| Recommended Next Action | `STATE` next or gap inference |

If STATE contradicts STORY or other files → note conflict; `research-memory`
before large work. If STATE is only bloated, inline-compress it in this write
(replace Focus/Next; drop old pointers) — do **not** call `research-memory`
just to get under 40 lines. Just-initialized STORY may still say
`_Not established yet._` — report that honestly and route to `research-loop`.

### 4. Emit packet and continue

Report the packet compactly, then **same turn**: if `W2 TEST` + named ordinary
EXP whose premises still hold → compact inner loop (if Status=`running` with
no terminal artifact → `monitor-experiment`, do not think through the wait;
if terminal artifacts are already complete → `result-analysis` at `W3 LEARN`,
do not re-launch); else execute `STATE` next or matching Skill; route
unclear → `research-loop` (FRAME/W4 only); files disagree → `research-memory` first.

Never end with only “已恢复” when an actionable step exists. Do not open a
questionnaire on an `ACTIVE` resume unless Goal amendment, major evaluation
change, or resource authorization change is actually in play — for the last,
run [workspace-setup](../workspace-setup/SKILL.md) instead of re-asking compute
and Git layout inside this Skill.

## Reads

**Always:** `PROJECT.md`, `STORY.md`, `STATE.md`, `AGENTS.md` (routing).

**On demand (pointers only):** current EXP section, linked artifact,
targeted `DISCOVERY.md` / `RESOURCES.md` / `LITERATURE.md` / `REVIEWS.md`.
Do **not** default-load the full EXPERIMENTS ledger or scan `work/`.

**Init:** `.agents/templates/` as the shape to materialize into existing files.

## Updates

Read-mostly after `ACTIVE`. During initialization, edit the eight existing
`.research/` files in place. EXPERIMENTS and REVIEWS stay empty. Otherwise:

- `RESOURCES.md` stale after successful re-location → prefer `workspace-setup`
  or `research-memory` as appropriate;
- `STATE.md` factually wrong or conflicting (non-trivial → `research-memory`);
  size overflow alone → inline compress.

After substantive work, follow
[state-files.md](../../references/state-files.md) §更新顺序.

## Deviation allowed

- Skip optional reads when the scientific working set suffices; one EXP
  section instead of full `EXPERIMENTS.md`; start inside the active
  experiment when STATE and user agree; `research-loop` on explicit
  file-consistent tasks; init may leave STORY as `_Not established yet._`.

Do **not** guess Story, fabricate Key Observation / Core Idea / Evidence, replay
completed work, treat chat as evidence, load every state file “just in case,”
`cat` the whole EXPERIMENTS ledger, scan `work/`, create a second set of
`.research/` files, or grill the user every iteration (look up facts; route
scientific unknowns).
