# Next Research Move Prompt

Hand this prompt to the [research-lead](../subagents/research-lead.md)
subagent (or any fresh-context lead with no executor history).
Role, independence, and the ban on canonical writes: that file.
This prompt teaches **how to choose** the next move; it does not execute
literature, experiments, or state-file edits.

Judgment operators:

- [scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
- [story-loop.md](../references/story-loop.md)

Gap priority, anti-duplication, two-layer Workflow, stagnation: story-loop.md.
**If STATE Workflow Position is `W2 TEST` and Next already names an EXP, do not
dispatch research-lead** — continue the inner loop instead.

Claim kinds, rivals, falsifiability, qualitative evidence strength, exploratory vs confirmatory:
scientific-reasoning.md. Do **not** invent numeric scores, stars, or
percentages of belief.

Outcome: [experiment-record.md](../references/experiment-record.md) §Outcome 值
(cite only; do not copy that table). Verdict:
[reviewer.md](../subagents/reviewer.md) §Verdict (cite only; do not copy the
Verdict list).

## Task fields

Main Agent fills in before dispatch. **Do not paste full state files.**

```text
Role: research-lead
Story gap (caller's current guess, may be wrong): <one sentence>
Why a fresh read: <stuck / parallel ranking / STORY vs STATE vs DISCOVERY disagreement>
Relevant files:
  - .research/STORY.md
  - .research/STATE.md
  - .research/DISCOVERY.md
  - .research/EXPERIMENTS.md → index (and sections only if needed to avoid duplicates)
  - .research/PROJECT.md
  - .research/RESOURCES.md
  - .research/LITERATURE.md / .research/REVIEWS.md (skim if novelty or review debt is plausible)
Required output: .research/work/<task-slug>.md
```

Subagent **reads from disk**. Keep the handoff to paths
([subagent-handoff.md](subagent-handoff.md)).

## When this applies

- Main needs isolated judgment on priorities or bottleneck diagnosis
- The project feels stuck; STORY vs STATE vs DISCOVERY disagree
- Parallel work is possible and the orchestrator needs a ranked action list
- `research-loop` left the next action open, or stagnation signals in
  story-loop.md §停滞处理 are present

Do **not** use this prompt to run an EXP, write Story, assign Outcome, or
replace idea-evaluation / evidence-verification. Those gates have their own
prompts. Ordinary “continue the already-chosen EXP” does not need a lead
pass.

If Position is `W2` / `W3` / `W4` and the scientific question is still
clear, recommend continuing the Method Loop — not a new route
([story-loop.md](../references/story-loop.md) §Method-First Inner Loop).
Literature is **not** a default every few EXP: only W1 reframe, novelty
threat, a new mechanism, a required new baseline, or explicit user
freshness. Rank **one focal scientific question**; parallel EXPs that
serve it are allowed; unbounded multi-route after W2 focus is not.

## Scientific context to read

Read first (research-lead.md): `STORY.md` (Open Gaps, Boundary), `STATE.md`
(active experiment, blockers, recommended next), `DISCOVERY.md` (Negative,
Invalidated, Open Contradictions). Skim EXPERIMENTS **index** so you do not
recommend duplicate work — do **not** load the entire ledger; skim PROJECT
for completion conditions and constraints.

Identify which **claim kind** is currently weakest
(scientific-reasoning.md §B) — Problem, Observation, Mechanism, Performance,
Efficiency, Generality, or Boundary-as-non-scope. That question is the
operator. Do not invent numeric claim scores. Segment *names* stay in
[state-files.md](../references/state-files.md); this prompt must not
redefine Story.

## Research reasoning lenses

### Current bottleneck

Name the **single** method hypothesis that most limits progress — not the
next audit, schema gap, or review backlog. Typical honest bottlenecks: the
mechanism has no falsifier; the grouping unit is leaked; novelty is untested;
Must-run is unaffordable; a rival still explains the last positive;
Invalidated findings are being ignored; STATE points at a running EXP that
cannot change Core Idea; the method grew more complex without new
explanatory power.

A bottleneck is not “we should try another seed” unless the Question is
about variance. Mixing Observation with Mechanism (scientific-reasoning.md
§A) is how a Story becomes unfalsifiable — if that collapse is live, the
bottleneck is conceptual, not compute.

### Candidate actions

List 3–5 concrete next actions. Prefer a discriminating experiment that
can change a method decision. Literature / review / cleanup only when they
unblock that decision. Each must map to a specific Story gap, Boundary
item, or DISCOVERY contradiction, and must name **Decision This Task Can
Change**. Do not recommend redoing Invalidated / Negative routes without a
new mechanism (story-loop.md §反重复; research-lead.md quality bar). Do
not rank “schema audit / review rN / GPU identity / dataset qualification”
above a live method question.

For **each** candidate, write all five qualitative fields below. No fake
numeric scores. Compare in prose: higher / lower information, cheaper /
dearer, riskier / safer — relative to the other candidates, not 0–10.

#### Information gain

What would we know after this action that we do not know now? Prefer
actions that could change Problem, Core Idea, or whether to continue the
route (story-loop.md §Gap 优先级). A parameter sweep that cannot split
target vs rival is low gain even if it produces plots. Qualitative support
kinds, if you need language for *how strong a later result could be*, are
scientific-reasoning.md §E — cite the ladder; do not average it into a
score.

#### Cost

Compute, data access, wall-clock, **and** Agent / human attention, against
RESOURCES.md. Cheap + discriminating beats expensive + decorative
(experiment-thinking.md §H if the action is an EXP). Cost that starves a
Must-run elsewhere is a reason to rank this action down.

#### Risk

What can go wrong scientifically: leakage dressed as accuracy; confirmatory
rhetoric on a post-hoc slice (scientific-reasoning.md §F); Story-core rewrite
without Review; burning the budget on a costume mechanism; recommending
execution before EXP-ID exists. Engineering risk (crash) is not the same as
scientific risk (uninterpretable success).

#### Story impact

Which STORY segment and which claim kind would move if the action worked —
and which Boundary / Open Gap would remain. Do not draft replacement Story
prose. If the action cannot touch Problem / Core Idea / the focal gap, say
so.

#### Which uncertainty each action resolves

Name the uncertainty in scientific objects, not in vibes: unknown mechanism
vs unknown generality vs untested Problem claim vs unresolved rival vs
unverified artifact vs novelty threat. One action, one primary uncertainty.
If the action would not resolve any named uncertainty, drop it.

### Recommended action and why now

Pick **one** recommended action using story-loop.md §Gap 优先级: gaps that
could change core judgment first, then blockers to Story completion, then
quick high-information work. Cite that section; do not invent a parallel
priority table.

“Why now” must beat the alternatives *at this moment*: sequencing (a review
before a Story rewrite; literature when closest-work is the bottleneck;
minimum decisive test before a grid), RESOURCES, and what STATE already
marks running. If evidence is insufficient to choose, say so and recommend
the cheapest discriminating step (research-lead.md).

Stagnation (story-loop.md §停滞处理): consecutive EXPs that did not move
Problem / Core Idea / main gap, hyperparameter loops, a STATE that points
at the same running EXP with no Discovery, or Open Contradictions growing
without a test aimed at the contradiction — change strategy; do not
recommend “one more similar config.”

## Critical questions

- If we could run only one action this week, which uncertainty dies?
- Is the weakest claim kind a Problem/Observation gap (a new module will not
  fix it), a Mechanism gap (a leaderboard will not fix it), or Generality
  (another seed on the same draw will not fix it)?
- What result would lower belief in the current Core Idea
  (scientific-reasoning.md §D)? If none, the next move may be to rewrite the
  hypothesis, not to compute.
- Is STATE’s “recommended next” still the bottleneck, or is it inertia?
- Would idea-evaluation or evidence-verification actually fire, or would
  this be an ordinary exploratory step that must stay light?

## Fatal flaws / anti-patterns

- Fake numeric scores, composite “priority = 0.7,” star ratings
- Recommending a 50-seed leaderboard that cannot split rivals
- Treating Reviewer Verdict or Experiment Outcome as the next-move enum
  (cite those owners; do not recopy)
- Pasting STORY or EXPERIMENTS into the work file
- Executing, searching literature, or editing canonical state from this role
- Ranking “write more modules” above a named falsifier
- Ignoring Invalidated findings
- Firing every intelligence gate on a sanity rerun (protection case)

## Evidence requirements

Be specific: EXP-IDs, Open Gap numbers or phrases, DISCOVERY section themes,
RESOURCE limits. Cite paths, not pasted prose. Do not close a Story gap on
intuition (scientific-reasoning.md §E: intuition may propose; it may not
close).

## Decision logic

1. Read STORY / STATE / DISCOVERY; name one bottleneck.
2. List 3–5 candidates, each with information gain, cost, risk, Story
   impact, and the uncertainty it resolves — qualitative only.
3. Drop duplicates of on-disk work and Invalidated-without-new-mechanism.
4. Rank with story-loop.md §Gap 优先级, not with scores.
5. Recommend one action, with role/skill if applicable, and why it wins
   *now*.
6. Write `.research/work/<task-slug>.md` only. Stop.

## Required output

Write only under `.research/work/` — typically:

```text
.research/work/<task-slug>.md
```

Work-file headings for this dispatch (task prompt wins on artifact shape per
[subagent-handoff.md](subagent-handoff.md); research-lead.md still owns write
permissions, independence, and the quality bar — this prompt does not
replace that role contract). Fourth heading is `## why now` in the work
file; the role file's default fourth heading is `## reasoning summary`:

```text
## current bottleneck
<one paragraph; name the claim kind and the live uncertainty>

## candidate next actions
1. <action> — targets <gap/blocker>
   - Information gain: <qualitative>
   - Cost: <qualitative, vs RESOURCES>
   - Risk: <scientific, not only engineering>
   - Story impact: <which segment / claim kind>
   - Uncertainty resolved: <which scientific object>
2. ...

## recommended action
<one clear sentence naming role/skill if applicable>

## why now
<2–4 sentences; cite EXP-IDs or DISCOVERY themes, not pasted prose>
```

Return to caller: bottleneck, candidates, recommended action, and why now.
Keep the return concise; the work file holds the per-candidate fields. Do
not polish the work file after writing it.

## Handoff / state impact

- research-lead writes `.research/work/` only.
- **Do not** edit `STORY.md`, `STATE.md`, `DISCOVERY.md`, `EXPERIMENTS.md`,
  or any other canonical state file.
- Main integrates via `research-loop` / the named Skill. After ADVANCE-level
  idea work or high-stakes evidence, Main — not the lead — dispatches
  idea-evaluation or evidence-verification.
- This prompt produces no Verdict and no Outcome.

## Stop / escalation

- Insufficient evidence to choose → cheapest discriminating step, then stop.
- Proposed Story-core change (Problem / Key Observation / Core Idea) →
  recommend Reviewer *after* the relevant EXP exists; do not rewrite Story
  here (story-loop.md §停滞处理 item on Review before a large Story change).
- Novelty / closest-work is the bottleneck → literature-research / scout,
  not a guess.
- Costume mechanism or missing falsifier → point at idea-evaluation; do not
  skip to a large EXP.
- Artifact↔claim mismatch on a high-stakes result → point at
  evidence-verification; do not auto-approve Story Evidence.
- After the work file is written, stop. Do not start the recommended action
  from this role.
