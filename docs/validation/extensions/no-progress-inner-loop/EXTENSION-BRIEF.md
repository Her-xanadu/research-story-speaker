# Extension Brief — no-progress inner loop

## Request

**User need:** A local project ran hundreds of experiment rounds without
successful results. Stop the inner loop from minting another similar EXP
when method judgment has not moved.

**Target users/projects:** Long-running ACTIVE workspaces.

**Why existing behavior is insufficient:** `W4 → W2` was the default even
when Method Check failed and stagnation actions were marked 非强制. A
numbered Next kept `research-loop` bypassed. “No successful results” was
read as “need more runs,” not “the campaign did not change the method.”

## Classification

**Primary type:** prompt/reference enrichment of the existing Story loop.

**Deployment scope:** framework core (instruction-only).

**Extension-ladder rung:** 3–4. Not a new Skill.

**Decision:** `EXTEND_EXISTING`

**Why lower rungs are insufficient:** Rung 0 already had Method Check and
stagnation language, but the **default** still continued the inner loop.
Project configuration cannot flip that default.

## Existing Owner and Overlap

**Closest existing owner:** `story-loop.md` §停滞处理, §W4, Method Check.

**Reuse:** method-consequence verbs; Method Check cadence (3–5 scientific
EXPs); Idea-gate PARK/ABANDON; support-task rule; compact five-item design.

**Will not add:** 16th Skill, `Research Round` STATE field, numeric “after
N EXPs park” Protocol, idea-evaluation on a single sanity miss.

## Capability Contract

**Trigger:** consecutive meaningful scientific EXPs left method consequence
unchanged and the next proposal is the same Question / same rival.

**Outputs:** W4 exit `W1 FRAME`; do not mint a same-route EXP.

**Protection:** one exploratory miss, engineering retry, or a true new
isolation still continues W2.

## Architecture Fit

Story loop, eight files, Main writer, single `research-loop`, no new state
source. Progressive disclosure: compact result-analysis inlines the Next
rule; does not open idea-evaluation.

## Minimal File Plan

Modified: `story-loop.md`, `research-loop/SKILL.md`, `result-analysis/SKILL.md`,
`experiment-design/SKILL.md`, `experiment-record.md`, `next-research-move.md`,
`idea-and-mechanism.md`, `workspace-resume/SKILL.md`, `AGENTS.md`.

## Validation

**Expected-use:** long same-route stretch → W1 park/abandon/reframe, no
new EXP-ID.

**Must-not-trigger:** single sanity negative; parser retry; new
mechanism-off after `keep`.

## Release Class

`validated extension` (static). No live Gate.

## Handoff

**framework-maintenance mode:** `standard`
