# Failure Diagnosis Prompt

Used by [experiment-execution](../skills/experiment-execution/SKILL.md) and
[experiment-agent](../subagents/experiment-agent.md) when a run, setup, or
measurement does not come out as hoped.

Design reasoning:
[experiment-thinking.md](../references/research-intelligence/experiment-thinking.md).
Status vs Outcome:
[experiment-record.md](../references/experiment-record.md) — **cite** §Status 值
and §Outcome 值; **do not recopy those tables.**

Dispatch: [subagent-handoff.md](subagent-handoff.md).

## Role / Objective

**Classify the failure first**, then pick **one** next move. Seeing a red
metric or a crash is not a license to start a parameter sweep. Engineering
failure is **not** a scientific negative.

This prompt does not assign a scientific Outcome. Technical failure stays
`Outcome=not-assessed` until a valid test exists.

## When this task applies

- A run crashed, hung, produced NaNs, missing artifacts, or refused to start.
- The job “completed” but the implementation, split, host, or logger is
  suspected broken.
- The metric moved the wrong way **and** it is unclear whether that is a bug,
  a bad regime, or a real hypothesis hit.
- The executor is about to “try more hyperparameters” without a new Question.

Do **not** use this prompt as a substitute for
[result-diagnosis.md](result-diagnosis.md) once the test is scientifically
usable. Do not use it to rewrite Core Idea.

## Task fields

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence>
What failed: <crash | missing artifact | bad metric | other — one line>
Relevant files:
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - .research/RESOURCES.md
  - <log / receipt / metric paths>
  - .research/work/<executor-report>.md (if any)
  - <code @ commit>
Required output: .research/work/<task-slug>.md
```

## Scientific context to read

1. EXP Question, Comparisons, pre-run prediction, Data/Setup, Git, Runs,
   Results — the **scientific contract** of this EXP.
2. RESOURCES codebase / host / data pointers (stale path ≠ dead hypothesis).
3. Logs and receipts at the recorded paths.
4. [experiment-thinking.md](../references/research-intelligence/experiment-thinking.md)
   §A (question first), §F (must / nice / cut), §G (ablation ≠ sweep), §H
   (cheap discriminating test).

Do not load idea-evaluation or deep literature to debug a crash.

## Research reasoning lenses

### Step 1 — Classify (exactly one primary class)

These strings are **diagnosis classes**. They are not Status, not Outcome, not
Verdict. Pick the class that **best** explains this failure. If two apply,
name the primary and a secondary; do not average them.

```text
engineering failure
environment failure
experimental invalidity
measurement failure
hypothesis contradiction
mechanism failure
optimization failure
data-regime mismatch
inconclusive
```

Tells (use the one that fits; do not chant all nine):

- **engineering failure** — implementation bug, import/path error, OOM from
  a leak, wrong entrypoint. The Question is still well-posed.
- **environment failure** — host, GPU, disk, network, permissions, stale
  RESOURCES path. Same code would work elsewhere.
- **experimental invalidity** — as implemented, this EXP cannot answer its
  Question (leaked split, wrong unit, missing control that was required to
  isolate anything). Often a design/implementation break, not “the idea died.”
- **measurement failure** — logger, metric code, NaN, truncated receipt.
  The number cannot be trusted, so it cannot refute the hypothesis.
- **hypothesis contradiction** — run is scientifically usable **and** the
  pre-run prediction failed in the direction that counts against the
  hypothesis. This is **not** a debug ticket.
- **mechanism failure** — a valid isolation (e.g. Full ≈ mechanism-off, or
  Full ≈ sham) shows the named component is not doing the claimed work.
- **optimization failure** — training/search did not converge, budget
  collapsed, optimizer diverged. May be engineering **or** a confounder;
  do not declare the mechanism dead until a fair, budget-matched rerun is
  possible — and do not “fix” it with an unbounded sweep.
- **data-regime mismatch** — the run was on a different dataset, split,
  capture, or condition than the Story failure point
  ([experiment-thinking.md](../references/research-intelligence/experiment-thinking.md)
  §A–B).
- **inconclusive** — underpowered, uncontrolled, or too noisy to tell.
  Inconclusive is not `contradicts`.

### Step 2 — Choose exactly one action

These strings are **move recommendations**. They are not Protocol Status
values. Main maps them onto EXPERIMENTS.

```text
retry
bounded debug
repair same EXP
redesign same EXP
new EXP
pivot
park
close
```

- **retry** — same contract, likely flake or transient environment.
- **bounded debug** — engineering / measurement / environment, contract
  intact. Follow the protocol below. Default **1–3** effective iterations,
  then stop debugging and have Main record a blocker in STATE. Do not debug
  until the Question mutates. **This prompt owns that 1–3 default**;
  `experiment-execution` and `experiment-agent` cite it and must not fork
  the number.
- **repair same EXP** — fix code/data binding, same Question, same
  Comparisons. New commit notes in Git; not a new EXP-ID. Ordinary
  engineering (API incompatibility, missing path, serialization, CLI typo)
  uses the compact path: identify → minimal repair → targeted test →
  rerun same Run/EXP. Then stop. No new EXP, no independent scientific
  review, no Story update, no W1.
- **redesign same EXP** — the Question is still right; the method, unit, or
  controls cannot answer it. Stop execution. Return to `experiment-design`.
  Changing the scientific contract mid-debug is not “repair.”
- **new EXP** — different Question (new rival, new condition, new isolation).
  Do not silently reuse this EXP-ID for a new question.
- **pivot** — this failure (once classified as scientific) puts Core Idea or
  route in doubt. Do not pivot from a crash.
- **park** — not now (resources, sequencing, missing data). Parking is
  progress.
- **close** — recommend abandoning this EXP as low-value or the wrong test.
  Main sets Status; this prompt does not redefine Status.

## Critical questions

- Did the **scientific contract** (Question, rival, prediction, unit) survive?
- Could a simpler bug, host, or logger produce this exact symptom?
- If you “just sweep,” what belief would a better number actually change?
- Is this already a result that belongs to result-diagnosis?

## Fatal flaws / anti-patterns

### HARD RULE

**Seeing a failure must not immediately start a parameter sweep.**

A learning-rate grid, a width grid, or “try more seeds until it looks good”
is not diagnosis. Sweeps are last, and only when the Question is about a
threshold or budget
([experiment-thinking.md](../references/research-intelligence/experiment-thinking.md)
§G). Cut is a success (§F).

Other bans:

- Treating **engineering failure** as Negative Discovery or as Outcome
  `contradicts`.
- Infinite debug that quietly changes the Question, split, or metric.
- New EXP-ID for a bugfix under the same Question (parser / UUID / schema
  / path / compatibility stay support work).
- Pivot / close from a missing file.
- Copying Outcome or Verdict tables into the work file.

## Evidence requirements

Point at: log lines or exit, host, commit, config, which Run, and which part
of the EXP contract still holds. “It failed” is not evidence.

## Decision logic

```text
classify first
  engineering | environment | measurement (broken)
      → retry | bounded debug | repair same EXP
      → Status failed if the run is unusable; Outcome stays not-assessed
      → no DISCOVERY Negative
  experimental invalidity | data-regime mismatch
      → repair if the contract can be restored; else redesign same EXP
      → do not interpret the number as hypothesis death
  optimization failure
      → bounded debug only if the bug is obvious; else one budget-matched
        rerun or redesign — never an open sweep
  hypothesis contradiction | mechanism failure
      → stop debugging-as-science
      → result-diagnosis.md (Main / result-analyst)
      → action is new EXP / pivot / park / close — not a sweep
  inconclusive
      → say so; next move is a discriminating redesign or park, not more
        of the same cells
```

### Bounded debug protocol (only if that action was chosen)

Keep the original Question, rival, prediction, and unit.

```text
preserve scientific contract
  → reproduce
  → minimize
  → diagnose
  → repair
  → targeted validation (the must-run comparison, not a grid)
  → resume original EXP
```

If the repair would change the Question, split, metric family, or claimed
mechanism: **stop**. That is redesign, not debug.

## Required output

Write `.research/work/<task-slug>.md`:

```text
## failure class
<one primary class from the list; optional secondary>

## why this class
<symptoms vs contract; artifact pointers>

## scientific contract
<intact | broken — Question / rival / prediction / unit>

## recommended action
<exactly one from the action list>

## why not a sweep
<one paragraph; required even if obvious>

## bounded debug plan
<fill only if action is bounded debug; else n/a>
iterations used / remaining; stop if contract drifts

## EXPERIMENTS hint for Main
Status / Outcome hint: technical unusable run → failed + not-assessed
Do not invent Outcome tokens. Cite §Outcome 值 if a value is named.
```

Return class + action + “why not a sweep” to the caller.

## Handoff / state impact

- **`experiment-agent`:** work file only. Raw findings stay non-interpretive.
- **Main applying `experiment-execution`:** may set `Status=failed` and keep
  `Outcome=not-assessed` on a technical failure; may record Runs / Git /
  Results. Must **not** write DISCOVERY or STORY, and must **not** assign a
  scientific Outcome here.
- Scientific classes (hypothesis contradiction, mechanism failure, usable
  null-shaped results) → Main loads
  [result-diagnosis.md](result-diagnosis.md) / `result-analysis`.

## Stop / escalation

- After 1–3 bounded-debug iterations (this prompt's default, above) without
  restoring the contract: stop debugging. Main records a blocker in STATE;
  this prompt does not invent a Status token. Do not keep going until the
  EXP is a different experiment.
- If the class is scientific: stop this prompt; do not “debug the idea.”
- If RESOURCES cannot fund even the must-run comparison: **park**, do not
  substitute an invalid cheaper test
  ([experiment-thinking.md](../references/research-intelligence/experiment-thinking.md)
  §H).
