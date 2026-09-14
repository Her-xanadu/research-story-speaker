# Extension Brief — experiment-method-design

## Request

**User need:** Inspect [jurgendn/agent-skills](https://github.com/jurgendn/agent-skills)
for operators this framework can approach, especially **experiment method
design**. Localize only what survives overlap, at the lowest ladder rung.

**Target users/projects:** Every research-story-speaker workspace that
designs an EXP (compact exploratory or full isolating design).

**Why existing behavior is insufficient:** Method-First, controls, Must /
Nice / Cut, and “what observation would change next action” already exist,
but:

- compact design often fills that observation line as “if the metric goes
  up, continue,” with no pre-run method consequence;
- claim kinds live in `scientific-reasoning.md` §B, which compact design
  is forbidden to open, so a mechanism claim can still ship with only a
  weak baseline;
- a surprisingly large gain is not named as a split / unit / metric
  question at design time.

## Classification

**Primary type:** prompt/reference enrichment (Layer 2) plus a bounded
mode note in the existing `experiment-design` Skill (compact stop-line).

**Deployment scope:** framework core (instruction-only; every workspace).

**Extension-ladder rung:** 3 (reference) + 4 (compact operators in the
existing Skill). Not rung 7.

**Decision:**
`EXTEND_EXISTING`

**Why lower rungs are insufficient:**

- Rung 0: the gaps above are real after overlap check.
- Rung 1: not project configuration.
- Rung 2: composing `idea-evaluation` or a hypothetical baseline-selector
  Skill on every proposal would violate the ordinary-exploratory
  protection case.
- Rung 3 alone: compact design **must not** open `experiment-thinking.md`,
  so operators that matter on the hot path have to live above the
  `experiment-design` stop line as in-session notes.
- Rung 7: a 16th Skill (`hypothesis-and-ablation-planner`,
  `benchmark-and-baseline-selector`, `statistical-testing-guide`,
  `model-eval-error-analysis`) is not earned. Existing owners cover the
  rest.

## Existing Owner and Overlap

**Closest existing owner:**
`.agents/references/research-intelligence/experiment-thinking.md`
(design reasoning). Compact disclosure:
`.agents/skills/experiment-design/SKILL.md`. Full-design prompt:
`.agents/prompts/experiment-proposal.md`. Claim kinds:
`scientific-reasoning.md` §B. Method-consequence verbs already used by
compact `result-analysis` and Method-First question 4.

**Overlap found (reuse, do not duplicate):**

- Question / rival / prediction first (`experiment-thinking.md` §A)
- Unit of analysis and leakage (`§B`)
- Controls taxonomy and prediction matrix (`§D`–`§E`)
- Must / Nice / Cut (`§F`) and ablation ≠ sweep (`§G`)
- Method-First five questions (`experiment-proposal.md`)
- Idea-gate ADVANCE / REVISE / PARK / ABANDON (`idea-and-mechanism.md` §H)
- Integrity-first after artifacts (`evidence-and-claim.md`, compact
  `result-analysis`)
- Classify failure before any hyperparameter (`failure-diagnosis.md`)

**What will be reused:** existing EXP section fields, method-consequence
verbs, Must-run / Nice-to-have, RSS claim kinds.

**What will not be duplicated or imported:**

- a 16th Skill, a second orchestrator, a Protocol enum, a state file
- upstream `scripts/stats.py`, `scripts/fit_scaling_law.py` (frozen
  `scripts = 0`)
- their install CLI / control plane
- a four-slot baseline taxonomy as a chant
- p-value / seed-count recipes
- `research-idea-stress-test` (Idea-gate already owns cheapest falsifier)
- slice-error-analysis as a new result Skill (later, if earned)

## Capability Contract

**Trigger:** designing a new EXP or refining a `planned` section.
Compact: ordinary exploratory / sanity. Full: existing full-design
triggers unchanged.

**Inputs:** Story gap, current EXP section / index, RESOURCES (full
design only).

**Outputs:** the same EXPERIMENTS section mapping. Compact still five
in-session items. No new canonical field.

**Read scope:** compact — target EXP section / index; STORY / STATE only
if the Question is not on disk. Full — existing full-design reads plus
`experiment-thinking.md` A→J.

**Write scope:** Main / `experiment-design` writes EXPERIMENTS + STATE as
today. `experiment-proposal.md` still writes only `.research/work/`.

**External dependencies:** none.

**Permissions:** none new.

**Fallback/degraded behavior:** if the three pre-run readings cannot be
named, do not spend compute (same as missing rival / prediction, unless
explicitly exploratory).

**Failure semantics:** missing publication baselines is **not** a
full-design trigger and **not** `idea-evaluation`.

**Provenance:** abstract ideas from jurgendn/agent-skills
research-experimentation cluster (license unknown). Local wording only.
Registry: `docs/design/research-intelligence-sources.md`.

**Observability:** operators are in the compact table notes and in
`experiment-thinking.md` §I–§J; validation is static overlap +
protection greps.

**Uninstall/rollback:** revert the three instruction files, the registry
row, and this `docs/validation/extensions/experiment-method-design/`
directory. No project state migration.

## Architecture Fit

- [x] Story-driven loop preserved
- [x] Eight canonical files preserved
- [x] Main canonical writer preserved
- [x] Single `research-loop` preserved
- [x] Prompt/file/config-first preserved
- [x] Canonical-first / adapters-thin preserved
- [x] Progressive disclosure preserved (compact still stops; A→J is full)
- [x] No second truth or state source

## Minimal File Plan

**Files added:**
`docs/validation/extensions/experiment-method-design/EXTENSION-BRIEF.md`,
`VALIDATION.md`

**Files modified:**
`.agents/references/research-intelligence/experiment-thinking.md`,
`.agents/skills/experiment-design/SKILL.md`,
`.agents/prompts/experiment-proposal.md`,
`.agents/prompts/method-review.md`,
`docs/design/research-intelligence-sources.md`

**Files intentionally untouched:**
AGENTS.md routing, subagent roles, result-analysis, failure-diagnosis,
idea-evaluation, adapters, `.research/` science, skill count, scripts.

**Rules linked rather than copied:** claim-kind table stays in
`scientific-reasoning.md` §B; Idea-gate glossary stays in
`idea-and-mechanism.md` §H; Status / Outcome stay in
`experiment-record.md`.

**New terms:** none as Protocol. “Pre-run method consequence” and
“claim-matched comparison” are Layer-2 operator names for existing
judgments.

**New dependencies:** none.

## Complexity Budget

**Permanent files added:** 0 instruction files; 2 validation docs.

**Core files touched:** 3 instruction + 1 registry.

**Default-context impact:** compact `experiment-design` stop-line grows
by a short in-session note (still five items; still no Layer 2).

**Ordinary-path impact:** compact must write three method-consequence
readings and a claim-matched contrast; must **not** open proposal /
thinking / idea-evaluation / a baseline essay.

**Network/tool impact:** none.

**External permissions:** none.

**New failure modes:** agents might treat keep / simplify / abandon as
new Outcome tokens — the copy forbids that.

**Maintenance owner:** `experiment-thinking.md` (operators);
`experiment-design` (compact disclosure).

**Complexity avoided or removed:** rejected four upstream Skills and two
Python scripts.

## Validation

**Expected-use case:** compact design of an ordinary performance probe
writes keep / change / stop readings in Expected outcomes and an honest
incumbent in Comparisons, without opening `experiment-proposal.md` or
`idea-evaluation`. Full isolating design walks A→J and cites §I–§J.

**Must-not-trigger protection case:** ordinary exploratory / sanity does
not fire idea-evaluation, evidence-verification, reviewer, a four-slot
baseline taxonomy, or auto-upgrade for missing publication baselines.

**Dependency-missing case:** n/a (no tools).

**Partial-failure case:** cannot name the stop reading → do not spend
compute (unless explicitly exploratory).

**Duplicate/retry case:** already-planned sanity verifies the five items;
no new EXP-ID.

**Held-out case:** statistical-testing-guide recipes and scaling-law
scripts stay out of tree.

**Cross-Harness case:** `.claude/skills` and `.cursor/skills` remain
symlinks to `.agents/skills`; prompts live once under `.agents/prompts/`.

**Security/privacy checks:** no credentials; unknown-license source is
abstract idea only.

**Rollback/removal test:** `git revert` of this change set.

## Source and License

**Upstream source:** https://github.com/jurgendn/agent-skills

**Specific file/Skill:**
`skills/research-experimentation/experiment-design/SKILL.md` (primary);
inspected not imported: `hypothesis-and-ablation-planner`,
`benchmark-and-baseline-selector`, `statistical-testing-guide`,
`model-eval-error-analysis`, `research-discovery/research-idea-stress-test`.

**License:** unknown (GitHub API `license: null` 2026-09-14; no root
LICENSE; no `license:` frontmatter).

**Concept adapted:** pre-run method consequence; match comparison
protocol to claim kind; large-gain is a split/unit/metric question first.

**Copied text:** no

**Local wording/structure:** RSS claim kinds, method-consequence verbs,
Must-run / Nice-to-have, compact five-item persist. No upstream
checklists or procedure lists.

## Release Class

`validated extension` (static overlap + protection). Not a live scientific
Gate. Not a 16th Skill.

## Handoff

**framework-maintenance mode:**
`standard`

**Next action:** static validation in `VALIDATION.md`; no live Gate
claimed. Independent Reviewer not required for this instruction change.
