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

- Editing `.agents/skills/`, adding `adapters/`, or a release / `v0.1` audit.
- Suspected duplication, adapter drift, or files growing beyond useful size.
- User asks to audit framework, check skill duplication, or fix adapter drift.

Do **not** use for research, experiments, Story updates, or day-to-day experiment
records — use scientific Skills instead.

## Goal

Keep **one canonical framework** easy to migrate across harnesses: logic in
`.agents/` and `.research/` protocols, not duplicated in adapters or monoliths.

## Core principles

`canonical first` / `adapter thin` / `no duplicated workflow` / `no unnecessary
abstraction`. Check drift against [state-files.md](../../references/state-files.md)
§单一事实来源（§20） (owners + host invocation) and §尺寸建议. Adapter six-question
boundary lives in `adapters/` — this Skill does not restate it.

**Never** copy the same rule into five Skills — link to references instead.

## Default flow

### Editing framework

1. Identify which canonical file or Skill owns the behavior.
2. Change **one** canonical location; update cross-links elsewhere.
3. If a harness needs awareness, touch only the relevant thin `adapters/xxx.md`.
   Confirm adapters still answer only host-invocation questions and do **not**
   copy research / experiment / Story logic (see §单一事实来源（§20）).
4. Run maintenance checklist (below) before tagging a release.
5. Output a **modification suggestion list** for file owners — do not silently
   rewrite others' canonical research files during audit.

### Maintenance audit checklist

Record **pass / fail / note** and a concrete fix. Each row asks whether the
workspace **drifted** from the linked reference — do not copy the rule body here.

| # | Check | Contrast against |
|---|-------|------------------|
| 1 | Duplicate rules | Skills/prompts restating workflows already in `.agents/references/` |
| 2 | Skill overlap | Multiple Skills defining the same behavior — merge or link |
| 3 | AGENTS size | [state-files.md](../../references/state-files.md) §尺寸建议 vs `AGENTS.md` |
| 4 | STATE size | same §尺寸建议 vs `STATE.md` carrying history or experiment logs |
| 5 | STORY as log | [state-files.md](../../references/state-files.md) §STORY.md vs run details/numbers |
| 6 | DISCOVERY vs EXPERIMENTS | [state-files.md](../../references/state-files.md) §反重复规则 |
| 7 | Adapter drift | `adapters/` / `CLAUDE.md` vs [state-files.md](../../references/state-files.md) §单一事实来源（§20） |
| 8 | Unnecessary scripts | V0.1 instruction-only goal — framework `.py/.sh` against that intent |

Also verify: `.agents/skills/` is the **only** canonical Skill root; subagent
handoff uses `.research/work/` per
[subagent-handoff.md](../../prompts/subagent-handoff.md).

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

Audit produces suggestions; Lead assigns fixes — do not rewrite `.research/`
science content.

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

- Helper scripts only when instruction-only is insufficient — document in #8.
- Temporarily exceed line targets if splitting harms clarity — note in audit.
- Skip adapter files for harnesses not yet tested.
- Progressive disclosure: default read set PROJECT + STORY + STATE per state-files.

## Boundaries

- This skill does **not** do science.
- Canonical content lives in workspace repo; code repos stay separate
  ([git-linking.md](../../references/git-linking.md)).
- Size / owner / adapter rules: check against
  [state-files.md](../../references/state-files.md) §尺寸建议 and §单一事实来源（§20）.
- Do not delete valuable negative results in `.research/` during cleanup.
