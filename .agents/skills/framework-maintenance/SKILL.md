---
name: framework-maintenance
description: >-
  Maintain the Story Research Workspace framework itself — not research content.
  Audit duplicate rules, skill overlap, adapter drift, AGENTS size, and file
  responsibilities. Use when modifying skills, adding harness support, running
  maintenance audit, or checking canonical-first / thin-adapter compliance. Do
  not use for running experiments or updating Story science.
---

# Framework Maintenance

Thin Skill for **workspace harness hygiene** — not scientific judgment.
Single-source rules: [state-files.md](../../references/state-files.md) §20.
Loop semantics stay in [story-loop.md](../../references/story-loop.md).

## When to use

- Editing or adding `.agents/skills/` content.
- Adding or changing `adapters/` or host-specific thin wrappers.
- Periodic hygiene before a release or `v0.1` tag.
- Suspected duplication across Skills, references, or adapters.
- `AGENTS.md`, Skills, or state files growing beyond useful size.
- User asks to audit framework, check skill duplication, or fix adapter drift.

Do **not** use for: conducting research, experiments, Story updates, or day-to-day
experiment records — use scientific Skills (`experiment-execution`, `result-analysis`, etc.).

## Goal

Keep **one canonical framework** easy to migrate across harnesses: logic in `.agents/`
and `.research/` protocols, not duplicated in adapters or monolithic Skills.

## Core principles

```text
canonical first
adapter thin
no duplicated workflow
no unnecessary abstraction
```

| Concern | Canonical owner |
|---------|-----------------|
| Story semantics | `story-maintenance` |
| Experiment record fields | [experiment-record.md](../../references/experiment-record.md) |
| Git experiment linking | [git-linking.md](../../references/git-linking.md) |
| State file roles | [state-files.md](../../references/state-files.md) |
| Research loop | [story-loop.md](../../references/story-loop.md) |
| Host invocation | `adapters/*.md` only |

**Never** copy the same rule into five Skills — link to references instead.

## Default flow

### Editing framework

1. Identify which canonical file or Skill owns the behavior.
2. Change **one** canonical location; update cross-links elsewhere.
3. If a harness needs awareness, touch only the relevant thin `adapters/xxx.md`.
4. Run maintenance checklist (below) before tagging a release.
5. Output a **modification suggestion list** for file owners — do not silently rewrite
   others' canonical research files during audit.

### Adapter rules

Each `adapters/xxx.md` may only answer: how host finds `AGENTS.md` and Skills; how to
create/invoke Subagent and Reviewer; MCP exposure; host limitations. Adapters must
**not** define how research, experiments, or Story changes work.

### Maintenance audit checklist

Record **pass / fail / note** and concrete fix per item:

| # | Check | What to look for |
|---|-------|------------------|
| 1 | Duplicate rules | Same workflow or field defs repeated across Skills or references |
| 2 | Skill overlap | Multiple Skills defining the same behavior — merge or link |
| 3 | AGENTS size | `AGENTS.md` bloated with skill-level detail — route to Skills |
| 4 | STATE size | `STATE.md` carrying history or experiment logs |
| 5 | STORY as log | `STORY.md` accumulating run details or numbers |
| 6 | DISCOVERY vs EXPERIMENTS | Redundant narratives; experiments duplicated in Discovery |
| 7 | Adapter drift | `adapters/` or `CLAUDE.md` copying canonical science logic |
| 8 | Unnecessary scripts | Framework scripts against V0.1 instruction-only goal |

Also verify: `.agents/skills/` is the **only** canonical Skill root; subagent handoff uses
`.research/work/` per [subagent-handoff.md](../../prompts/subagent-handoff.md).

### Audit output format

```text
## Framework Maintenance Audit — <date>

### Findings
- [CRITICAL|MAJOR|MINOR] <issue> — owner: <file/skill> — suggested fix

### Passed checks
- ...

### Release recommendation
freeze | fix-first
```

Audit produces suggestions; Lead assigns fixes — do not rewrite `.research/` science content.

## Reads

| Area | Files |
|------|-------|
| Framework | `AGENTS.md`, `CLAUDE.md`, `README.md`, `.agents/skills/**`, `.agents/references/**`, `adapters/**` |
| Sample state | `.research/*.md` (bloat checks only — do not edit science) |

## Updates

| Allowed | Not allowed during audit |
|---------|--------------------------|
| Skills you own, adapters after cold-start test | Direct edits to others' files without handoff |
| `README.md` architecture notes (if owner) | Research content in `.research/` |
| Cross-links between canonical references | Deleting negative results or experiment history |

## Deviation allowed

- Helper scripts only when instruction-only is insufficient — document why in finding #8.
- Temporarily exceed line targets if splitting harms clarity — note in audit.
- Skip adapter files for harnesses not yet tested.
- Progressive disclosure: default read set PROJECT + STORY + STATE per state-files.

## Boundaries

- This skill does **not** do science.
- Canonical content lives in workspace repo; code repos stay separate
  ([git-linking.md](../../references/git-linking.md)).
- Size targets (guidance): `AGENTS.md` < ~150 lines; `STATE.md` tens of lines;
  `STORY.md` ~one page; Skills modular (~100–130 lines), not 500-line monoliths.
- Do not delete valuable negative results in `.research/` during cleanup.
