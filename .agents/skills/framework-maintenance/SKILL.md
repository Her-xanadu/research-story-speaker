---
name: framework-maintenance
description: >-
  Maintain the Story Research Workspace framework itself — not research content.
  Modes: standard (audit), session-diagnosis, skill-evolution, regression-eval.
  Audit duplicate rules, skill overlap, adapter drift, AGENTS size, and file
  responsibilities. Use when modifying skills, adding harness support, running
  maintenance audit, diagnosing repeated agent friction, proposing one Skill
  change, regression-eval of a candidate, or checking canonical-first /
  thin-adapter compliance. Do not use for running experiments or updating
  Story science. Not a 13th Skill.
---

# Framework Maintenance

Thin Skill for **workspace harness hygiene** — not scientific judgment.
Single-source rules: [state-files.md](../../references/state-files.md) §20.
Loop semantics stay in [story-loop.md](../../references/story-loop.md).
Skill-change judgment:
[skill-evolution.md](../../references/research-intelligence/skill-evolution.md)
(Layer 2 — cite; do not recopy).

## When to use

- Editing `.agents/skills/`, adding `adapters/`, or a release / version audit.
- Suspected duplication, adapter drift, or files growing beyond useful size.
- User asks to audit framework, check skill duplication, or fix adapter drift.
- Repeated *agent/workflow* friction (`session-diagnosis`); one atomic Skill
  or prompt candidate (`skill-evolution`); baseline vs candidate
  (`regression-eval`).

Do **not** use for research, experiments, Story updates, or day-to-day experiment
records — use scientific Skills instead. Do **not** create a 13th Skill for
doctor / upper / experience-to-skill — those are **modes** of this Skill.

## Goal

Keep **one canonical framework** easy to migrate across harnesses: logic in
`.agents/` and `.research/` protocols, not duplicated in adapters or monoliths.

**All framework records** (audits, diagnoses, candidates, comparisons, cases)
go to `docs/validation/` — **never** project `.research/`.

## Modes

Pick **one**. This Skill already exists; add a mode, not a new Skill.

| Mode | What it is |
|------|------------|
| `standard` | Existing maintenance audit (checklist below) |
| `session-diagnosis` | skill-doctor ideas — original wording below |
| `skill-evolution` | cite [skill-evolution.md](../../references/research-intelligence/skill-evolution.md) |
| `regression-eval` | skill-upper ideas — original wording below |

## Core principles

`canonical first` / `adapter thin` / `no duplicated workflow` / `no unnecessary
abstraction`. Check drift against [state-files.md](../../references/state-files.md)
§单一事实来源（§20） (owners + host invocation) and §尺寸建议. Adapter six-question
boundary lives in `adapters/` — this Skill does not restate it.

**Never** copy the same rule into five Skills — link to references instead.

## Default flow

### `standard` — editing framework / audit

1. Identify which canonical file or Skill owns the behavior.
2. Change **one** canonical location; update cross-links elsewhere.
3. If a harness needs awareness, touch only the relevant thin `adapters/xxx.md`.
   Confirm adapters still answer only host-invocation questions and do **not**
   copy research / experiment / Story logic (see §单一事实来源（§20）).
4. Run maintenance checklist (below) before tagging a release.
5. Output a **modification suggestion list** for file owners — do not silently
   rewrite others' canonical research files during audit.

### `session-diagnosis`

skill-doctor ideas. Diagnose *agent/workflow* friction in a session.
Do **not** edit Skills in this mode. Cite
[skill-evolution.md](../../references/research-intelligence/skill-evolution.md)
§A.

Propose a Skill change only if **one** of these holds:

```text
>= 2 independent repeated problems
```

or:

```text
deterministic reproducer + protection case
```

### `skill-evolution`

Cite [skill-evolution.md](../../references/research-intelligence/skill-evolution.md).
Load that Layer 2 file; do not recopy operators. One Skill or one prompt,
one behavior. Output `candidate deserves review` — not a silent merge.

### `regression-eval`

skill-upper ideas. Compare old vs new under the **same**:

```text
model/context/tools/input
```

Cases:

```text
failure cases
protection cases
held-out cases
```

Candidate must:

```text
improve target failure
AND
not regress protection cases
```

Passing means:

```text
candidate deserves review
```

It does **not** overwrite the canonical Skill. Cite
[skill-evolution.md](../../references/research-intelligence/skill-evolution.md)
§C–F.

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
| 8 | Unnecessary scripts | instruction-only goal — framework `.py/.sh` against that intent |
| 9 | Cross-file terminology consistency | 七/八 state-file count; Status vs Outcome（Outcome per experiment-record.md）; Verdict 引用 reviewer.md；文件名；template 与 reference 一致。Do not copy Outcome/Verdict enumerations into this Skill. |
| 10 | Skill count is **12**, not 10 | Original 10 plus `idea-evaluation` and `evidence-verification`. Do not add a 13th Skill. |
| 11 | Intelligence refs are Layer 2, not Protocol | `.agents/references/research-intelligence/` vs Protocol owners (`state-files.md`, `experiment-record.md`, `reviewer.md`, `story-loop.md`, `git-linking.md`). Layer 2 does not redefine Outcome / Verdict / Story. |
| 12 | Gates selective | `idea-evaluation` / `evidence-verification` / deep literature are not default on every EXP. Ordinary exploratory stays light — [skill-evolution.md](../../references/research-intelligence/skill-evolution.md) §C–E. |

Also verify: `.agents/skills/` is the **only** canonical Skill root (**12**
Skills, including `idea-evaluation` and `evidence-verification`); subagent
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
| Layer 2 (evolution modes) | [skill-evolution.md](../../references/research-intelligence/skill-evolution.md) |
| Sample state | `.research/*.md` (bloat checks only — do not edit science) |
| Framework records | `docs/validation/` |

## Updates

| Allowed | Not allowed during audit |
|---------|--------------------------|
| Skills you own, adapters after cold-start test | Direct edits to others' files without handoff |
| `README.md` architecture notes (if owner) | Research content in `.research/` |
| Cross-links between canonical references | Deleting negative results or experiment history |
| Writes under `docs/validation/` | Framework maintenance logs in project `.research/` |

## Deviation allowed

- Helper scripts only when instruction-only is insufficient — document in #8.
- Temporarily exceed line targets if splitting harms clarity — note in audit.
- Skip adapter files for harnesses not yet tested.
- Progressive disclosure: default read set PROJECT + STORY + STATE per state-files.
- `session-diagnosis` / `skill-evolution` / `regression-eval` skip the full
  12-row checklist when the task is one friction class or one candidate.

## Boundaries

- This skill does **not** do science.
- Canonical content lives in workspace repo; code repos stay separate
  ([git-linking.md](../../references/git-linking.md)).
- Size / owner / adapter rules: check against
  [state-files.md](../../references/state-files.md) §尺寸建议 and §单一事实来源（§20）.
- Intelligence refs are Layer 2, not Protocol.
- Do not delete valuable negative results in `.research/` during cleanup.
- Do not add a 13th Skill. Doctor / upper / experience-to-skill are modes.
