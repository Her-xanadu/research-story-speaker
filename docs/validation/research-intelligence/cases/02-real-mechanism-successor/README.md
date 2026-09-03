# Case 02 — Real Mechanism Successor

**Wave:** V0.2 Wave F (read-only regression fixture)
**Kind:** Idea-gate
**Gate D priority:** ordinary (ADVANCE path)
**Load:** this folder only.

Do **not** copy these snippets into the framework repo `.research/`
(keep UNINITIALIZED).

Anonymized MOCK. Same fictional **MOCK-WBS** lab as Case 01, different
candidate. No host home paths.

---

## Trigger (what to hand the agent)

High-stakes **mechanism replacement**:

> Current WES still misses low-volume rare bursts. Candidate **PRRW**
> (Port-Rarity Rank Window) changes the **information source**: each
> window is scored by destination-port rarity rank against the previous
> K windows at the same site, not by packet-size entropy. Please run
> idea-evaluation. If identity is real, ADVANCE and name the minimum
> decisive test (do not skip the mechanism-off control).

This **is** an `idea-evaluation` trigger. Not a routine ablation.

---

## Input (embedded MOCK)

### `.research/PROJECT.md` (excerpt)

```markdown
# Project: MOCK-WBS Window Burst Scoring

## Research Goal
Detect rare burst events from 60-second traffic windows at a site.

## Evaluation Principles
- Mechanism claims need a mechanism-off or information-matched control.
- Formal conclusions need an honest baseline.
- CPU-only MOCK budget; no payload, no future-packet oracles.
```

### `.research/STORY.md` (excerpt)

```markdown
# Story: Window Entropy May Flag Rare Bursts

> MOCK fixture — not a live Story.

## Problem
Rare burst windows are missed when packet volume is low.

## Key Observation
Missed rare bursts on MOCK site-A often coincide with unusual
destination-port mixes, not only with packet-size entropy.

## Core Idea
**WES:** mix of packet-size entropy and log-volume, λ = 0.5,
threshold the mixed score.

## Evidence
- EXP-010: WES pipeline runs (sanity only).
- Entropy vs volume not isolated. Port mix never used as a feature.

## Boundary
- MOCK synthetic site-A. No payload.

## Open Gaps
1. Rare-burst misses when volume is low (failure condition).
2. Whether a **different information source** (port rarity) would
   move that miss, vs retuning WES.
```

### `.research/STATE.md` (excerpt)

```markdown
## Current Focus
Decide whether PRRW is a real successor mechanism for Open Gap 1.

## Active Experiment
none

## Recommended Next Action
Idea-gate PRRW before any confirmation grid.
```

### `.research/DISCOVERY.md` (excerpt)

```markdown
## Current Scientific Understanding
WES is not isolated. Port mix is an observation, not yet a mechanism.

## Negative Discoveries
_None covering port-rarity under the rare-burst condition._
```

### `.research/EXPERIMENTS.md` (excerpt)

```markdown
## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-010 | WES sanity on synthetic site-A | completed | supports | Open Gaps: rare-burst still open | 2026-08-01 |
```

### `.research/LITERATURE.md` (excerpt)

```markdown
## MOCK-WES-2019 — Window entropy scorer
Closest existing method: WES. Same decision shape (window score +
threshold). **Axis that would have to differ:** information source
(size entropy vs destination-port rarity rank). LITERATURE does not
show a port-rarity-rank window scorer under this rare-burst condition.
```

### `.research/RESOURCES.md` (excerpt)

```markdown
## Authorized compute
One CPU host, ≤ 4h for a single-split comparison. 50-seed grids are
out of budget until a minimum decisive test exists.
```

### Candidate (real identity shift)

```text
Name: Port-Rarity Rank Window (PRRW)
Information source: destination-port rarity rank vs previous K windows
  at the same site (not packet-size entropy, not payload).
Decision rule: threshold the rarity-rank score (volume may stay as a
  reused covariate, listed under complexity-budget `reuse`).
Mechanism prediction (before any run): on the rare-burst / low-volume
  condition, Full PRRW F1 − Mechanism-off F1 ≥ 0.08, while WES stays
  flat. If destination-port ranks are shuffled inside the window
  (mechanism-off: same scaffold, claimed component destroyed), the
  rare-burst lift disappears.
Mechanism-off control: keep windowing, threshold, and volume covariate;
  replace rarity ranks with a permutation of ranks in that window.
Sham (optional later): rank-shaped noise with the same histogram,
  no genuine rarity order.
Failure condition to hit: low-volume rare-burst windows on MOCK site-A
  (STORY Open Gap 1) — not a different regime.
```

No result artifacts in this case. Gate D must not invent a completed
EXP or metrics. The **prediction** above is the pre-run bet, not evidence.

---

## Expected behavior

1. **Fire** `idea-evaluation` + `idea-evaluation.md`.
2. Work file only: `.research/work/idea-evaluation-prrw.md`.
3. Mechanism Distinction is **real**: information source (and the
   decision uses that source). Deletion test §G: deleting rarity rank
   (or shuffling it) removes the claimed information flow — not a
   rename, not a frozen weight.
4. No §D fatal hit on the fixture as written (information is obtainable
   at the site; no test-split leakage described; RESOURCES can fund the
   single comparison; on-Story).
5. **Recommended Action: `ADVANCE`** per `idea-and-mechanism.md` **§H**.
   Cite §H; do not recopy the glossary; do not treat ADVANCE as
   Protocol / Outcome / Verdict.
6. **Minimum Decisive Experiment** is specified in the work file:
   smallest comparison that can change judgment, including a
   **mechanism-off** control (see `experiment-thinking.md` §D), on the
   §B rare-burst / low-volume condition, at the window/site unit.
   Typical shape: Full PRRW vs mechanism-off vs current WES, one split,
   not a 50-seed grid. **Do not assign EXP-IDs** in this Skill.
7. After ADVANCE, **stop** Idea-gate writes. Main **may** later route
   to `experiment-design` / `experiment-proposal.md`. That follow-on is
   allowed **only after** ADVANCE and is not this fixture’s pass.

---

## Skills / prompts that should fire

| Should fire | Why |
|-------------|-----|
| `idea-evaluation` + `idea-evaluation.md` | Mechanism replacement |
| `idea-and-mechanism.md` §A–H | Identity, fatal audit, §H action + minimum test |
| `experiment-thinking.md` §D (as cited) | mechanism-off / information-matched kinds |
| `scientific-reasoning.md` §C–D | Rival + discriminating prediction |

## Skills / prompts that should **not** fire (this Idea-gate pass)

| Must not fire yet | Why |
|-------------------|-----|
| `experiment-design` / creating `EXP-xxx` | Idea-evaluation does not register the EXP |
| `experiment-execution` | No design persisted |
| `evidence-verification` | No result |
| `result-analysis` | No result |
| Deep literature by default | Axis already named on disk; escalate only if the agent claims unread novelty they cannot cite |
| `experiment-review` | No EXP; ADVANCE is not a Verdict |

Skipping Idea-gate and jumping to a confirmation grid is a miss.

---

## Observable asserts (Gate D)

- [ ] `Recommended Action: ADVANCE`
- [ ] Minimum decisive test names **mechanism-off** (or equivalent
      isolating control) and the rare-burst failure condition
- [ ] No EXP-ID minted by the Idea-gate writer
- [ ] Core Idea not rewritten by the Skill (Main integrates later)
- [ ] Work file is not a 50-seed protocol

## Forbidden regressions

- PARK/REVISE **only** because the name is new, despite a real
  information-flow change and a named mechanism-off test
- ADVANCE with no discriminating test (“run a leaderboard”)
- Copying the §H four-line glossary into the work file
