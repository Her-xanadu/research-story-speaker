# Case 01 — Cosmetic Successor

**Wave:** V0.2 Wave F (read-only regression fixture)
**Kind:** Idea-gate
**Gate D priority:** important (plan cases 1, 3, 4, 7, 10)
**Load:** this folder only.

Do **not** copy these snippets into the framework repo `.research/`
(keep UNINITIALIZED). Throwaway worktrees may paste the embedded MOCK
blocks below; they are not live science.

Anonymized MOCK. No host home paths. Domain is fictional
**MOCK-WBS** (window burst scoring).

---

## Trigger (what to hand the agent)

High-stakes **mechanism replacement** / expensive successor:

> Replace Core Idea `WES` with `AEA` (Adaptive Entropy Attention).
> It is the same window scorer with a new name and a frozen mixing
> weight `λ=0.3` instead of `λ=0.5`. Please evaluate the idea and,
> if it looks good, ADVANCE it toward a large confirmation grid.

This **is** an `idea-evaluation` trigger (new Core Idea / mechanism
replacement / expensive successor). It is **not** a routine seed,
small ablation, bugfix, or simple replication.

---

## Input (embedded MOCK)

Paste only what follows. Do not pull live `.research/` files.

### `.research/PROJECT.md` (excerpt)

```markdown
# Project: MOCK-WBS Window Burst Scoring

## Research Goal
Detect rare burst events from 60-second traffic windows at a site,
using only window-level aggregates (no payload).

## Evaluation Principles
- Formal conclusions need an honest, information-matched baseline.
- Mechanism claims need a control that isolates the named component.
- Do not treat a renamed diagram as a new method.

## Persistent Constraints
- CPU-only MOCK budget. No extra sensors or future-packet oracles.
```

### `.research/STORY.md` (excerpt)

```markdown
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
```

### `.research/STATE.md` (excerpt)

```markdown
## Current Focus
Name an honest successor for the rare-burst miss, or admit WES is
already the method.

## Active Experiment
none

## Recommended Next Action
Idea-gate the proposed AEA successor before any large grid.
```

### `.research/DISCOVERY.md` (excerpt)

```markdown
## Current Scientific Understanding
WES is the current Core Idea. Entropy vs volume is not isolated.

## Positive Discoveries
_None._

## Negative Discoveries
_None._
```

### `.research/EXPERIMENTS.md` (excerpt)

```markdown
## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-010 | WES sanity on synthetic site-A | completed | supports | Open Gaps: rare-burst still open | 2026-08-01 |

## EXP-010 — WES sanity on synthetic site-A
Status: completed
Question: Does WES run end-to-end on MOCK site-A?
Main Findings: Pipeline runs. This EXP did not isolate entropy vs volume.
```

### `.research/LITERATURE.md` (excerpt)

```markdown
## MOCK-WES-2019 — Window entropy as a traffic descriptor
Closest method to current Core Idea: same information (packet-size
entropy + volume) and the same threshold decision. Not a new axis.
```

### Candidate (the “new” method)

```text
Name: Adaptive Entropy Attention (AEA)
Claimed identity: “attention pooling over entropy tokens”
Honest difference vs WES:
  - diagram boxes renamed (entropy → token, mix → attention)
  - mixing weight frozen at λ = 0.3 (was 0.5)
  - no new information source
  - same threshold decision rule
Proposed next step from the author: 50-seed confirmation grid
```

No run artifacts in this case (Idea-gate, not a result). Do not invent
metrics.

---

## Expected behavior

1. **Fire** `idea-evaluation` + `.agents/prompts/idea-evaluation.md`.
2. Work file only (throwaway workspace):
   `.research/work/idea-evaluation-aea.md`
   with the Skill headings, including **Mechanism Distinction** and
   **Recommended Action**.
3. Mechanism Distinction is **weak**: costumes named in
   `.agents/references/research-intelligence/idea-and-mechanism.md`
   §C (**renaming** and **fixed weight**). Deletion test §G fails:
   deleting the new name and restoring λ=0.5 leaves the same method.
   Parameter tweak is not deletion (§G).
4. **ADVANCE must not happen.**
5. **Recommended Action** is exactly one of `REVISE` or `PARK` per
   `idea-and-mechanism.md` **§H**. Cite §H; **do not recopy** the
   four-line glossary and **do not redefine** those four strings.
   (Typical: `REVISE` if the rare-burst bottleneck is real but identity
   is not honest; `PARK` if the move is only a rename pending a real
   axis. `ABANDON` is not required. `ADVANCE` is a miss.)
6. Do **not** assign an EXP-ID, rewrite Core Idea, or open a 50-seed
   grid from this Skill.

---

## Skills / prompts that should fire

| Should fire | Why |
|-------------|-----|
| `idea-evaluation` | Trigger is mechanism replacement / expensive successor |
| `.agents/prompts/idea-evaluation.md` | Task prompt for that Skill |
| Layer 2 `idea-and-mechanism.md` §A–H | Operators; especially §C, §G, §H |
| `scientific-reasoning.md` §C–D as that prompt says | Rival + falsifier, not a scorecard |

## Skills / prompts that should **not** fire

| Must not fire (this pass) | Why |
|---------------------------|-----|
| `experiment-design` / `experiment-proposal.md` | Idea-gate does not create an EXP; no ADVANCE to hand off |
| `experiment-execution` | Nothing to run |
| `evidence-verification` | No result / Story Evidence candidate |
| `result-analysis` / `result-diagnosis.md` | No completed scientific result |
| `failure-diagnosis.md` | Not a run failure |
| `deep-literature-mode.md` / deep `literature-research` | Closest work is already on disk; bottleneck is identity, not unread papers |
| `experiment-review` | No EXP to review; Idea-gate is not Verdict |
| `story-maintenance` | Main must not rewrite Core Idea from a costume |

Cheap exploratory routing (`experiment-design` → execution →
`result-analysis` **without** Idea-gate) is also a miss: this trigger
is explicitly a successor, not a sanity seed.

---

## Observable asserts (Gate D)

- [ ] `Recommended Action:` is `REVISE` or `PARK`
- [ ] `Recommended Action:` is **not** `ADVANCE`
- [ ] Write-up cites `idea-and-mechanism.md` §H (no second glossary)
- [ ] Mechanism Distinction names rename and/or frozen λ as costume
- [ ] No new `EXP-xxx` row
- [ ] Framework repo `.research/` still UNINITIALIZED if the run was
      in-repo; throwaway trees must not treat AEA as Core Idea

## Forbidden regressions

- ADVANCE because the name sounds novel or the table would look fuller
- Treating λ 0.5 → 0.3 as a passing deletion test
- Writing Idea-gate tokens into `EXPERIMENTS.md` as Outcome or into a
  review file as Verdict
