---
name: experiment-execution
description: >-
  Locate codebases from RESOURCES, verify Git state, implement and run
  experiments, record commits and result locations, and update EXPERIMENTS.md.
  Use when executing EXP-xxx, running code, freezing git commit, or
  implementing an experiment design. Do not use for result interpretation
  (result-analysis) or independent review (experiment-review).
---

# Experiment Execution

Thin Skill for **implementation and runs** — not scientific interpretation.
Field definitions: [experiment-record.md](../../references/experiment-record.md).
Git binding: [git-linking.md](../../references/git-linking.md).

## When to use

- An experiment design exists (`experiment-design` or Story gap) and needs code work.
- `EXPERIMENTS.md` has a new or active `EXP-xxx` ready to run (`planned` → `running`).
- You must locate an external codebase, freeze a Git commit, execute runs, record artifacts.
- `research-loop` routed to Experiment execution, or `experiment-agent` subagent dispatched.
- User asks to run, implement, or execute a specific `EXP-xxx`.

Do **not** use for: designing what to test (`experiment-design`), interpreting outcomes
(`result-analysis`), adversarial critique (`experiment-review`), or Story edits.

## Goal

Implement and run the experiment, preserve a recoverable code–result chain, and update
the experiment record with mechanical facts. **Running successfully is not scientific
success** — exit code zero or metric movement does not validate the Story; leave
interpretation to `result-analysis`.

## Default flow

1. **Resolve codebase** — Read `.research/RESOURCES.md` by Codebase ID from the
   `EXP-xxx` section. Use Preferred relative location; if stale, follow path-recovery
   in [git-linking.md](../../references/git-linking.md) (remote → relative → search →
   ask user → update RESOURCES). Never assume CWD is the code repo.
2. **Verify Git** — Confirm repo identity (`git remote -v`), branch, working tree.
   Workspace Git and code Git are independent repositories.
3. **Align with record** — Read `EXP-xxx` in `.research/EXPERIMENTS.md`: Question,
   Method, Comparisons, Data/Setup, planned Runs. Read `.research/STORY.md` for gap
   context only — do not edit Story here.
4. **Implement** — Prefer shared `src/`; experiment entry `experiments/EXP-xxx/`;
   results `results/EXP-xxx/`. Do not duplicate entire repos per EXP-ID
   ([git-linking.md](../../references/git-linking.md) §15).
5. **Freeze commit** — Before formal runs, commit or record recoverable SHA. Bind in
   `EXPERIMENTS.md`: Codebase ID, Git repository, Git commit, Entry, Results root.
   Multi-commit retries: document Initial / Fix / Valid runs per git-linking.
6. **Execute runs** — Distinguish Experiment (scientific unit) from Run (one execution).
   Log seeds, retries, host, and commit per run in the Runs field — no global Run ID.
7. **Save artifacts** — Record result paths (relative, absolute, or `host:path`). Ensure
   a stranger can navigate: EXPERIMENTS → RESOURCES → repo → Entry → Results.
8. **Update EXPERIMENTS** — Set Status (`running` / `completed` / `failed` as appropriate);
   fill Code, Git, Runs, Results, mechanical Main Findings only. Leave Interpretation,
   Discovery Impact, Story Impact for `result-analysis`.
9. **Update STATE** — Active experiment, blockers, recommended next (`result-analysis`
   when results exist).
10. **Hand off** — When runs finish, continue with `result-analysis` or dispatch
    `result-analyst` subagent. Suggest `experiment-review` only when stakes warrant.

## Reads

| Priority | Files |
|----------|-------|
| Required | `.research/EXPERIMENTS.md` (target `EXP-xxx`), `.research/RESOURCES.md` |
| Often | `.research/STORY.md` (gap context), `.research/STATE.md`, external code repo |
| Reference | [experiment-record.md](../../references/experiment-record.md), [git-linking.md](../../references/git-linking.md), [state-files.md](../../references/state-files.md) |
| Subagent | [experiment-agent.md](../../subagents/experiment-agent.md) |

## Updates

| File | What to update |
|------|----------------|
| `.research/EXPERIMENTS.md` | Index Status/Updated, Code, Git, Runs, Results, mechanical Main Findings |
| `.research/RESOURCES.md` | Last known local location after path recovery |
| `.research/STATE.md` | Active EXP, blockers, next step (brief) |

Do **not** update `DISCOVERY.md` or `STORY.md` here except trivial factual corrections.

## Deviation allowed

- Exploratory scratch without scientific claim — note absence of formal commit in Runs.
- Non-§15 layout when repo forbids it — document actual Entry/Results paths.
- Remote compute — record host and path in Results; runs may stay `running` until synced.
- Delegate implementation to `experiment-agent`; executor still owns workspace file updates.
- Abort invalid setup — record `failed` in Status/Runs; never delete the experiment section.
- Retry after bugfix under same EXP-ID — add commit notes in Git field, not a new EXP.

## Boundaries

- Do not treat metric improvement as validated science or update Story claims.
- Do not copy experiment narratives or numbers into `STORY.md`.
- Negative, null, and failed runs stay in `EXPERIMENTS.md` — never silently delete.
- `completed` Status means runs recorded, not hypothesis confirmed
  ([experiment-record.md](../../references/experiment-record.md)).
