# Baseline run — FWE held-out (canonical v0.2 prompt)

**Sealed until candidate frozen.** Scored after
`candidate/idea-evaluation.md.patch` was written; hunk not edited in
response to this fixture.

**Condition / model / tools / stop:** same as `failure/baseline-work.md`.  
**Input:** `held-out/README.md` MOCK.

## Loaded (cost)

Same Layer 2 set as the VAEG baseline run. `deep-literature-mode.md`
not loaded (closest work on disk; bottleneck is identity, not unread
papers).

## Routing

Trigger matches. Write Idea-gate artifact only.

---

# Idea-gate work (as would be `.research/work/idea-evaluation-fwe.md`)

## Problem Anchor

Open Gap 1: low-volume rare-burst miss. Scientific bottleneck is real
(§A).

## Exact Failure Point

WES under low-volume rare-burst windows on MOCK site-A (§B).

## Candidate Mechanism

FWE: author says train WES with focal loss so hard rare-burst windows
dominate the gradient.

## Mechanism Distinction

Honest remainder: same entropy + log-volume, same λ=0.5, same threshold
**at inference**. The deployed scorer is WES. “Focal loss” in this
fixture is a rename of the same window-level training signal
(`idea-and-mechanism.md` §C **cosmetic objective rewrite**).

Deletion test §G: delete the focal noun → still WES. Identity is a
costume. Author axis label “objective” does not create a new decision
rule.

## Fatal Flaws

No leakage/oracle on disk. Author 50-seed FWE vs WES grid cannot split
target vs rename-of-the-same-objective (not discriminating).

## Closest Work

MOCK-WES-2019: same information and threshold decision.

## Rival Explanations

The scorer **is** WES. Any metric movement is retuning / seed noise /
training-code rename, not a new objective mechanism
(`scientific-reasoning.md` §C).

## Discriminating Prediction

If focal were a real objective shift, a control that trains WES with
the original loss vs FWE should diverge **and** the deployed decision
rule should change. Here inference is unchanged; a null on that split
lowers belief in FWE as a successor (`scientific-reasoning.md` §D).

## Minimum Decisive Experiment

Do not run a 50-seed confirmation. If anything: original-loss WES vs
FWE with the **same** inference rule — expecting no honest identity
split. No EXP-ID. Prefer REVISE identity before any EXP.

## Complexity Budget

- reuse: WES mix, threshold
- new: none (focal label is not a new component)
- excluded: 50-seed grid; stacking further losses

## Recommended Action

**REVISE** per `idea-and-mechanism.md` §H.

---

## Observables

| Assert | Result |
|--------|--------|
| Work file written | yes |
| REVISE or PARK | **REVISE** |
| ADVANCE | **no** |
| Cosmetic objective rewrite named | yes |
| EXP-ID | none |

**Held-out miss (ADVANCE)?** No. Baseline already blocks FWE for this
model.
