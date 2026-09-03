# Wave H held-out — Cosmetic objective rewrite (FWE)

**Kind:** held-out (sealed until the candidate hunk was frozen)  
**Not used while designing** `candidate/idea-evaluation.md.patch`.  
**Wave:** V0.2 Wave H dogfood  
**Load:** this folder only. Do **not** copy into framework `.research/`.

Different failure class from the tuning fixture (VAEG = fake **selection
rule** / fixed-weight schedule). This case is §C **cosmetic objective
rewrite**: algebraically equivalent training signal, same inference
rule. Also not Case 01 (rename + frozen λ) and not Case 09 (novelty
threat / gated residual vs literature).

`skill-evolution.md` §E: score after freeze. A miss here is a **reject**,
not a hint to edit the hunk in this round.

---

## Trigger (hand this to the agent)

High-stakes **mechanism replacement**:

> Replace Core Idea `WES` with `FWE` (Focal Window Entropy). Author
> axis label: **objective** — “focal loss focuses training on hard
> rare-burst windows.” Identity is clear on the objective axis.
> Minimum test: 50-seed confirmation vs WES. Please ADVANCE.

This **is** an `idea-evaluation` trigger.

---

## Input (embedded MOCK)

Same MOCK-WBS lab as the VAEG failure (PROJECT / STORY / STATE /
DISCOVERY / EXPERIMENTS / LITERATURE excerpts identical in substance
to `failure/README.md`: WES λ=0.5, Open Gap = low-volume rare-burst
miss, MOCK-WES-2019 same information + threshold). Do not pull live
`.research/`.

### Candidate (the “new” method)

```text
Name: Focal Window Entropy (FWE)
Author-claimed axis: objective
Author-claimed identity: “train the same WES mix with focal loss so
  hard rare-burst windows dominate the gradient”
Honest difference vs WES (fixture, not author spin):
  - same two scalars: packet-size entropy and log-volume
  - same hand-set λ = 0.5
  - same threshold decision at inference (the deployed scorer is WES)
  - “focal loss” here is a rename of the same window-level training
    signal already used to fit the threshold; no new label, no new
    difficulty model, no change to the decision rule
  - algebraically equivalent objective / cosmetic objective rewrite
    (idea-and-mechanism.md §C)
Proposed next step from the author: 50-seed confirmation grid vs WES
```

No run artifacts. Do not invent metrics.

---

## Expected behavior

1. Fire `idea-evaluation`.
2. Mechanism Distinction: **cosmetic objective rewrite** (and/or
   renaming). Deletion test: delete “focal” and keep λ=0.5 threshold
   scorer → still WES. Inference decision unchanged.
3. **ADVANCE must not happen.**
4. Recommended Action: `REVISE` or `PARK` per §H (typical: REVISE).
5. No EXP-ID; no STORY rewrite.

**Miss:** ADVANCE because the author named the **objective** axis and
specified a grid (item 8 checklist filled).

---

## Observable asserts

- [ ] Work file written
- [ ] Recommended Action REVISE or PARK
- [ ] not ADVANCE
- [ ] Distinction names cosmetic objective rewrite (not a new decision
      rule)
- [ ] No EXP-ID
