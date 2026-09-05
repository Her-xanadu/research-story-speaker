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

`compact` / `full` are Skill-internal modes. Never write them into STATE,
EXPERIMENTS, Status, or Outcome.

## Compact / ordinary sanity (default)

Do **not** open `failure-diagnosis.md`, `experiment-record.md`, or
`git-linking.md`.

If the designed entry is missing and an operator-supplied log or
pre-existing result file answers the smoke Question: record that mechanical
fact (do **not** fake a re-run). That is **not** bounded debug. Hand off to
compact `result-analysis`.

Compact persist: fill Runs / Results / mechanical Main Findings from the
operator-supplied log / pre-existing result file. Record provenance in Runs /
Results: path, and that it was not this-run compute. Provenance unknown → keep
Outcome `not-assessed` and put a STATE blocker. Do **not** assign a scientific
Outcome here — keep `not-assessed` until `result-analysis`. Do **not** update
`DISCOVERY.md` or `STORY.md`. Required reads: `.research/EXPERIMENTS.md`
(target `EXP-xxx`), `.research/RESOURCES.md`.

**Full / real-run path — continue past the stop line only if** you must
implement, freeze a git commit, execute a real run, recover a stale codebase
path, or enter bounded debug after a real crash / hang / unusable metrics.

**Stop. Do not read the rest of this file unless full-mode triggers fire.**

---

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
interpretation and Outcome assessment to `result-analysis`. Binding fields and
stale-path recovery follow `git-linking.md` when you actually freeze a commit.
This Skill freezes the commit before runs and writes recovered paths into
`EXPERIMENTS.md` afterward.

## Default flow

1. **Resolve codebase** — Read `.research/RESOURCES.md` by Codebase ID from the
   `EXP-xxx` section. If the path is stale, recover it only via
   `git-linking.md` §路径恢复五步法 (open that file then). Never assume
   CWD is the code repo.
2. **Verify Git** — Confirm repo identity (`git remote -v`), branch, working tree.
   Workspace Git and code Git are independent repositories.
3. **Align with record** — Read `EXP-xxx` in `.research/EXPERIMENTS.md`: Question,
   Method, Comparisons, Data/Setup, planned Runs, planned Results root. Read
   `.research/STORY.md` for gap context only — do not edit Story here.
4. **Implement** — Prefer shared `src/`; experiment entry `experiments/EXP-xxx/`;
   results `results/EXP-xxx/`. Do not duplicate entire repos per EXP-ID
   (`git-linking.md` §推荐代码布局（§15） — open when implementing).
5. **Freeze commit** — Before formal runs, commit or record recoverable SHA. Bind
   the run in `EXPERIMENTS.md` using the fields in `git-linking.md`
   §每个正式 Experiment 的最小绑定 (open that file when freezing).
   Confirm any design-time baseline commit / planned result root against the actual
   run. Multi-commit retries: document Initial / Fix / Valid runs per git-linking.
   Ordinary sanity with an operator-supplied log and no code run: skip this step.
6. **Execute runs** — Distinguish Experiment (scientific unit) from Run (one execution).
   Log seeds, retries, host, and commit per run in the Runs field — no global Run ID.
   On engineering failure of a real run, follow **Bounded debug** below; do not
   start a sweep. Ordinary sanity missing-entry plus an operator-supplied log:
   skip Bounded debug; hand off to compact `result-analysis`.
7. **Save artifacts** — Record result paths (relative, absolute, or `host:path`). Ensure
   a stranger can navigate: EXPERIMENTS → RESOURCES → repo → Entry → Results.
8. **Update EXPERIMENTS** — Set Status (`running` / `completed` / `failed`);
   keep **Outcome=`not-assessed`** unless `result-analysis` has already judged.
   Technical failure: **Status=`failed`**, **Outcome=`not-assessed`** (section + Index).
   Fill Code, Git, Runs, Results, mechanical Main Findings only. Leave Interpretation,
   Discovery Impact, Story Impact for `result-analysis`.
9. **Update STATE** — Active experiment, blockers, recommended next (`result-analysis`
   when results exist).
10. **Hand off** — When runs finish (or an operator-supplied log / pre-existing
    result file is the artifact), continue with compact `result-analysis`. Do
    **not** default-dispatch `result-analyst`. Suggest `experiment-review` only
    when stakes warrant.

## Compact support failure (ordinary engineering)

Python/API incompatibility, missing path, serialization error, CLI typo,
parser/schema fix, runner bug, logging:

```text
identify → minimal repair → targeted test → rerun same Run/EXP
```

Default **end**. Do **not** create a new EXP-ID, do **not** open an
independent scientific review, do **not** update Story, do **not** reframe
to W1. Record in current EXP Run notes or
`.research/work/<current-exp>-support-*.md` plus Git
([experiment-record.md](../../references/experiment-record.md)
§Support-task rule).

**Support Resume Contract** (write in STATE Next + the work artifact; no
new STATE field):

```text
Blocked science EXP: EXP-xxx
Scientific question: ...
Support task: ...
Return condition: tests pass / artifact produced / resource restored
After return: resume EXP-xxx immediately
```

## Bounded debug (engineering failure)

Engineering failure is not a scientific negative. Open `failure-diagnosis.md`
**only** when a real run crashed, hung, or produced unusable metrics **and**
you are about to loop bounded debug.

When the action is bounded debug, keep the original Question / rival /
prediction / unit and:

```text
preserve scientific contract
  → reproduce
  → minimize
  → diagnose
  → repair
  → targeted validation
  → resume original EXP
```

Default **1–3** effective debugging iterations (number owned by
`failure-diagnosis.md`; cite, do not fork), then **stop** and record the
reason. Main may put a blocker in
`STATE.md`. Do not infinite-debug until the scientific question has changed. If the repair would change the scientific
contract (Question, split, metric family, or claimed mechanism), stop and
return to `experiment-design` — that is redesign, not debug.

## Reads

| Priority | Files |
|----------|-------|
| Required (ordinary sanity) | `.research/EXPERIMENTS.md` (target `EXP-xxx`), `.research/RESOURCES.md` |
| Do not open (ordinary sanity) | See **Compact / ordinary sanity (default)** |
| Often | `.research/STORY.md` (gap context), `.research/STATE.md`, external code repo |
| Reference (when running / binding git) | `experiment-record.md`, `git-linking.md`, `state-files.md` |
| On bounded-debug failure | `failure-diagnosis.md` |
| Subagent | `experiment-agent.md` |

## Updates

| File | What to update |
|------|----------------|
| `.research/EXPERIMENTS.md` | Index Status/Outcome/Updated, Code, Git, Runs, Results, mechanical Main Findings |
| `.research/RESOURCES.md` | Last known local location after path recovery |
| `.research/STATE.md` | Active EXP, blockers, next step (brief) |

Do **not** update `DISCOVERY.md` or `STORY.md`. Do not assign a scientific
Outcome here — keep `not-assessed` until `result-analysis`.

## Deviation allowed

- Exploratory scratch without scientific claim — note absence of formal commit in Runs.
- Non-§15 layout when repo forbids it — document actual Entry/Results paths.
- Remote compute — record host and path in Results; runs may stay `running` until synced.
- Delegate implementation to `experiment-agent`; Main Agent (this skill) still owns
  EXPERIMENTS/STATE updates after the subagent returns.
- Abort invalid setup — Status=`failed`, Outcome=`not-assessed`; never delete the section.
- Retry after bugfix under same EXP-ID — add commit notes in Git field, not a new EXP.
- Ordinary support failure: compact path above; resume the blocked science EXP.
- After 1–3 bounded-debug iterations without restoring the contract (cite
  `failure-diagnosis.md`): stop;
  record the reason. Do not keep going until the EXP is a different experiment.
- Do not treat metric improvement as validated science or copy numbers into `STORY.md`.
- Negative, null, and failed runs stay in `EXPERIMENTS.md` — never silently delete.
- `completed` Status means runs recorded, not hypothesis confirmed
  (`experiment-record.md`).
