---
name: idea-evaluation
description: >-
  Gate a new Core Idea, route competition, mechanism replacement, major pivot,
  expensive experiment, or high-stakes successor. Write an Idea-gate work
  artifact under `.research/work/`; do not edit STORY, DISCOVERY, or
  EXPERIMENTS. Do not use for routine seeds, small ablations, bugfixes, or
  simple replications. Triggers include evaluate idea, idea-evaluation,
  Core Idea gate, mechanism replacement, major pivot.
---

# Idea Evaluation

Thin Skill for an **Idea-gate** before a Core Idea or expensive successor
lands. Judgment lives in Layer 2 — load, do not copy:

- [scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md)
- [idea-and-mechanism.md](../../references/research-intelligence/idea-and-mechanism.md)
- [experiment-thinking.md](../../references/research-intelligence/experiment-thinking.md)

Idea-gate `ADVANCE` | `REVISE` | `PARK` | `ABANDON` are defined **only** in
idea-and-mechanism.md §H. Cite that section; do **not** recopy the four-line
glossary. They are **not** Protocol, **not** Outcome, **not** Verdict.

## When to use

Only:

- new Core Idea
- route competition
- mechanism replacement
- major pivot
- expensive experiment
- high-stakes successor

Do **not** use for: routine seed, small ablation, bugfix, or simple
replication. Cheap exploratory / sanity EXP stays on `experiment-design` →
`experiment-execution` → `result-analysis` and must not auto-load this Skill.

## Goal

One work artifact that recommends whether the candidate should become Core
Idea, be revised, parked, or abandoned. Main Agent integrates. This Skill
does not write canonical science files and does not create an EXP.

Output:

```text
.research/work/idea-evaluation-<slug>.md
```

Required headings:

```text
Problem Anchor
Exact Failure Point
Candidate Mechanism
Mechanism Distinction
Fatal Flaws
Closest Work
Rival Explanations
Discriminating Prediction
Minimum Decisive Experiment
Complexity Budget
Recommended Action
```

**Recommended Action** is exactly one of `ADVANCE` | `REVISE` | `PARK` |
`ABANDON` per [idea-and-mechanism.md](../../references/research-intelligence/idea-and-mechanism.md) §H.

## Default flow

1. **Confirm trigger** — If the move is a routine seed, small ablation, bugfix,
   or simple replication, stop. Write nothing.
2. **Read** — `.research/PROJECT.md`, `STORY.md`, `STATE.md`, `DISCOVERY.md`,
   `EXPERIMENTS.md` (index plus sections that touch this idea). Read
   `LITERATURE.md` when novelty / closest-work is in play.
3. **Anchor and identity** — Problem Anchor and Exact Failure Point per
   idea-and-mechanism.md §A–B. Candidate Mechanism and Mechanism Distinction
   per §C (deletion test §G).
4. **Fatal and closest** — Fatal Flaws per §D **before** a large experiment.
   Closest Work per §E. If novelty is the bottleneck, use LITERATURE and the
   deep-literature pointer in §E; do not guess papers.
5. **Rivals and test** — Rival Explanations and Discriminating Prediction per
   scientific-reasoning.md §C–D. Minimum Decisive Experiment per
   idea-and-mechanism.md §H and experiment-thinking.md. Complexity Budget per
   §F.
6. **Recommend** — One §H action. PARK or ABANDON is progress. Do not assign
   EXP-IDs, rewrite Core Idea, or skip Reviewer contract.
7. **Write and stop** — Only the work file. Hand off to Main. A subagent
   running this Skill writes `.research/work/` only.

## Reads

| Priority | Files |
| --- | --- |
| Required | `.research/PROJECT.md`, `.research/STORY.md`, `.research/STATE.md`, `.research/DISCOVERY.md`, `.research/EXPERIMENTS.md` (index + relevant sections) |
| If novelty | `.research/LITERATURE.md` |
| Reference | [scientific-reasoning.md](../../references/research-intelligence/scientific-reasoning.md), [idea-and-mechanism.md](../../references/research-intelligence/idea-and-mechanism.md), [experiment-thinking.md](../../references/research-intelligence/experiment-thinking.md) |

## Updates

| File | What to update |
| --- | --- |
| `.research/work/idea-evaluation-<slug>.md` | Full Idea-gate artifact (required headings) |

Must **not** write `.research/STORY.md`, `.research/DISCOVERY.md`, or
`.research/EXPERIMENTS.md`. Must **not** add canonical state files. Main Agent
integrates. This Skill may run in-session or as a subagent; both write only
`.research/work/`.

## Deviation allowed

- Stop with no file when the trigger does not match (protection: cheap
  exploratory EXP stays light).
- Update an existing `idea-evaluation-<slug>.md` when the same candidate is
  being revised, rather than proliferating files.
- Defer Closest Work depth when LITERATURE already names the axis; escalate
  rather than invent papers.
- Recommend REVISE and stop if identity or the minimum test is not yet honest.
- After ADVANCE, Main may route to `experiment-design`; this Skill does not
  design or register the EXP.
