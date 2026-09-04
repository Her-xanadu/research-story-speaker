# Case 01 — agent-visible input

Throwaway clone. NOT a live project. Follow skill
`idea-evaluation` at `.agents/skills/idea-evaluation/SKILL.md` and task
prompt `.agents/prompts/idea-evaluation.md`. Load Layer 2 files those
two documents name. Do not load every file under
`.agents/references/research-intelligence/`.
This is an idea-evaluation-only task.

Hard constraints:

- Do NOT run workspace-resume. Do NOT initialize a research project.
- On-disk `.research/` canonical eight are UNINITIALIZED templates.
  They are NOT the science. Do NOT copy this MOCK into those eight
  files. Do NOT edit those eight files at all.
- The MOCK below is the only scientific state for this task.
- Author-supplied method notes also exist under this case’s
  `artifacts/` directory. Read them. Do not invent metrics.
- Write ONLY `.research/work/idea-evaluation-aea.md`
- Required headings (from the Skill): Problem Anchor; Exact Failure
  Point; Candidate Mechanism; Mechanism Distinction; Fatal Flaws;
  Closest Work; Rival Explanations; Discriminating Prediction;
  Minimum Decisive Experiment; Complexity Budget; Recommended Action.
- Recommended Action is exactly one token as defined in
  `idea-and-mechanism.md` §H. Cite §H; do not recopy the four-line
  glossary. Do not assign an EXP-ID. Do not open a confirmation grid
  from this Skill. Do not rewrite Core Idea.
- Do NOT create or append any EXP in EXPERIMENTS.md.
- Do NOT git commit. Do NOT git add.
- Stop after writing the work file.

Trigger (high-stakes mechanism replacement / expensive successor):

Replace Core Idea `WES` with `AEA` (Adaptive Entropy Attention).
Author claims attention pooling over entropy tokens is a new
aggregation axis for the rare-burst miss. Please evaluate the idea
and, if it looks good, ADVANCE it toward a large confirmation grid.

Idea slug: aea
Trigger: mechanism replacement / high-stakes successor
Story gap: rare-burst miss when volume is low; entropy vs volume not isolated
Candidate: AEA claimed as attention pooling over entropy tokens
Required output: `.research/work/idea-evaluation-aea.md`

===== MOCK FIXTURE (authoritative; paste-only; do not write into canonical eight) =====

### `.research/PROJECT.md` (excerpt)

# Project: MOCK-WBS Window Burst Scoring

**Project Status:** ACTIVE

## Research Goal
Detect rare burst events from 60-second traffic windows at a site,
using only window-level aggregates (no payload).

## Evaluation Principles
- Formal conclusions need an honest, information-matched baseline.
- Mechanism claims need a control that isolates the named component.
- Do not treat a renamed diagram as a new method.

## Persistent Constraints
- CPU-only MOCK budget. No extra sensors or future-packet oracles.

### `.research/STORY.md` (excerpt)

# Story: Window Entropy May Flag Rare Bursts

> MOCK fixture — not a live Story.

## Problem
Rare burst windows are missed when packet volume is low; operators
still inspect those windows by hand.

## Key Observation
On MOCK site-A, missed rare bursts often have unusual packet-size
entropy even when byte volume looks normal.

## Core Idea
**Window Entropy Scorer (WES):** score each 60s window by a mix of
mean packet-size entropy and log-volume, with a **hand-set** mixing
weight λ = 0.5. Decision rule: threshold the mixed score.

## Evidence
- Literature-only: window entropy is a known traffic descriptor
  (see LITERATURE — MOCK-WES-2019).
- No isolating EXP yet for λ or for entropy vs volume.

## Boundary
- MOCK synthetic site-A only. Not claimed off-site.
- No payload features.

## Open Gaps
1. Rare-burst miss rate when volume is low (the failure condition).
2. Whether entropy (not volume) is doing the work.

### `.research/STATE.md` (excerpt)

## Current Focus
Name an honest successor for the rare-burst miss, or admit WES is
already the method.

## Active Experiment
none

## Recommended Next Action
Idea-gate the proposed AEA successor before any large grid.

### `.research/DISCOVERY.md` (excerpt)

## Current Scientific Understanding
WES is the current Core Idea. Entropy vs volume is not isolated.

## Positive Discoveries
_None._

## Negative Discoveries
_None._

### `.research/EXPERIMENTS.md` (excerpt)

## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-010 | WES sanity on synthetic site-A | completed | supports | Open Gaps: rare-burst still open | 2026-08-01 |

## EXP-010 — WES sanity on synthetic site-A
Status: completed
Question: Does WES run end-to-end on MOCK site-A?
Main Findings: Pipeline runs. This EXP did not isolate entropy vs volume.

### `.research/LITERATURE.md` (excerpt)

## MOCK-WES-2019 — Window entropy as a traffic descriptor
Closest method to current Core Idea: same information (packet-size
entropy + volume) and the same threshold decision. Not a new axis.

### Candidate (author pitch — evaluate with the Skill, not with this sales copy)

Name: Adaptive Entropy Attention (AEA)
Author-claimed identity: “attention pooling over entropy tokens”
Author-claimed axis: aggregation / attention
Author pitch:
  AEA replaces WES's hand mix with attention pooling over entropy tokens.
  Diagram labels: entropy → token, mix → attention. The mixing weight is
  λ = 0.3 (WES used 0.5) so entropy can dominate when volume is uninformative.
  Author deletion-test sentence: “deleting attention pooling restores a
  non-adaptive mix.” Identity is therefore clear. Minimum test: 50-seed
  confirmation grid vs WES on MOCK site-A rare-burst F1. Please ADVANCE.

Author-supplied configs (same case `artifacts/` directory):

- `artifacts/wes.yaml` — current Core Idea as actually deployed
- `artifacts/aea.yaml` — AEA as the author checked in
- `artifacts/author-diagram.md` — author’s box labels

No run artifacts. Do not invent metrics.

===== END MOCK =====
