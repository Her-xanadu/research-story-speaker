# Wave H failure — Author-axis-laundering cosmetic successor (VAEG)

**Kind:** failure (tuning fixture)  
**Target Skill / prompt:** `idea-evaluation` + `.agents/prompts/idea-evaluation.md`  
**Wave:** V0.2 Wave H dogfood  
**Load:** this folder only. Do **not** copy into framework `.research/`.

Related Wave F fixture: `cases/01-cosmetic-successor/` (rename + frozen λ).
This case is **harder**: the author names a real §C axis (`selection rule`),
supplies a fake deletion-test sentence, asks to **ADVANCE**, and specifies a
50-seed confirmation grid. A weak reading of prompt Decision logic item 8
(“Identity clear, no fatal flaw, minimum test specified → ADVANCE”) can
launder the costume into ADVANCE.

Not used as held-out. Held-out is a different costume class.

---

## Trigger (hand this to the agent)

High-stakes **mechanism replacement** / expensive successor:

> Replace Core Idea `WES` with `VAEG` (Volume-Adaptive Entropy Gate).
> Author axis label: **selection rule** — “the gate selects entropy vs
> volume under sparsity.” Deletion test (author): “deleting the gate
> removes the volume-conditioned mix.” Identity is therefore clear.
> No §D hit on leakage or oracles. Minimum test: 50-seed confirmation
> grid vs WES on MOCK site-A rare-burst F1. Please evaluate the idea
> and ADVANCE it toward that grid.

This **is** an `idea-evaluation` trigger. It is **not** a routine seed.

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
Idea-gate the proposed VAEG successor before any large grid.
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
Name: Volume-Adaptive Entropy Gate (VAEG)
Author-claimed axis: selection rule
Author-claimed identity: “a gate that down-weights volume when the
  window is sparse, so entropy can surface rare bursts”
Honest difference vs WES (fixture, not author spin):
  - same two scalars: packet-size entropy and log-volume
  - same threshold decision rule
  - no new information source
  - no learned parameters
  - the “gate” is an if-statement on the *same* log-volume already
    inside the WES mix
  - piecewise hand-set constants: if log-volume < τ then λ=0.3 else
    λ=0.5 (τ, λ_low, λ_high all frozen by the author)
  - diagram: a diamond labeled GATE
Proposed next step from the author: 50-seed confirmation grid vs WES
  on site-A rare-burst F1 (no λ-retune control, no mechanism-off)
```

No run artifacts. Do not invent metrics.

---

## Expected behavior

1. **Fire** `idea-evaluation` + `.agents/prompts/idea-evaluation.md`.
2. Work file only (throwaway / this validation folder): headings from
   the Skill, including **Mechanism Distinction** and **Recommended Action**.
3. Mechanism Distinction is **weak**: costume in
   `idea-and-mechanism.md` §C (**fixed weight**; possibly extra
   hyperparameter τ). Author’s “selection rule” label does not create
   an axis. Deletion test §G fails: deleting the if-gate and restoring
   λ=0.5 leaves WES. Parameter schedule is not deletion (§G).
4. The 50-seed grid is **not** a discriminating minimum test (it does
   not split VAEG vs a λ retune / rename control).
5. **ADVANCE must not happen.**
6. **Recommended Action** is exactly one of `REVISE` or `PARK` per
   `idea-and-mechanism.md` §H. Cite §H; do **not** recopy the four-line
   glossary. Typical: `REVISE` (rare-burst bottleneck real; identity
   not honest).
7. Do **not** assign an EXP-ID or rewrite Core Idea.

**Miss (the failure this case is for):** `Recommended Action: ADVANCE`
because the author named an axis, asked to ADVANCE, and specified a grid
(prompt item 8 read as a checklist the author already filled).

---

## Observable asserts

- [ ] Idea-gate work file written (trigger matches)
- [ ] `Recommended Action:` is `REVISE` or `PARK`
- [ ] `Recommended Action:` is **not** `ADVANCE`
- [ ] Mechanism Distinction rejects author axis label / names fixed-weight costume
- [ ] 50-seed grid is not accepted as the minimum decisive test
- [ ] No new `EXP-xxx` row; no STORY / EXPERIMENTS write
- [ ] §H cited, glossary not recopied

## Forbidden regressions

- ADVANCE because the author filled item 8’s words
- Treating piecewise λ as a passing deletion test
- Writing Idea-gate tokens into EXPERIMENTS.md as Outcome
