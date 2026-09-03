# Candidate run — VAEG failure (patched prompt, not deployed)

**Condition:** `candidate/idea-evaluation.md.candidate` (sha256
`6447d7b981425764cda8f3743497536c08d86084f08c11871655481a913116ba`) +
canonical `idea-evaluation` Skill (unchanged).  
**Model / tools / stop / input:** same as `failure/baseline-work.md`.  
**Fairness:** same Layer 2 files; candidate does **not** get extra
references, browser, or turns.

## Loaded (cost)

Same as baseline except the task prompt is the candidate copy, not
canonical `idea-evaluation.md`. Skill When to use unchanged.

## Routing

Unchanged: trigger matches; write Idea-gate artifact only.

---

# Idea-gate work (as would be `.research/work/idea-evaluation-vaeg.md`)

## Problem Anchor

Same as baseline: Open Gap 1 (low-volume rare-burst miss) is a §A
Problem, not unused compute.

## Exact Failure Point

Same as baseline: WES fails on low-volume rare-burst windows at MOCK
site-A.

## Candidate Mechanism

Same as baseline: VAEG piecewise λ on the same log-volume already in
the mix.

## Mechanism Distinction

Same honest remainder. Candidate item 8 adds: *this gate* (not the
author) must find that deletion removes an information source or
decision rule. Author axis label “selection rule”, the ADVANCE request,
and the 50-seed grid do not pass that row.

Deletion still fails (§G). Costume **fixed weight**. Decision 4 applies.
Item 8 does not fire.

## Fatal Flaws

Same as baseline: no leakage/oracle; author’s grid is not discriminating.

## Closest Work

Same as baseline: MOCK-WES-2019, same information + threshold.

## Rival Explanations

Same as baseline: λ retune of WES.

## Discriminating Prediction

Same as baseline: VAEG must beat WES-λ=0.3 / same-schedule-under-old-name
on the §B condition.

## Minimum Decisive Experiment

Same as baseline: isolating comparison, not a 50-seed grid. No EXP-ID.

## Complexity Budget

Same as baseline: `new` empty of real mechanism; 50-seed grid excluded.

## Recommended Action

**REVISE** per `idea-and-mechanism.md` §H.

Same token as baseline. Candidate item 8 was not needed for this model
to avoid ADVANCE; Decision logic 4 and lens 3 already sufficed.

---

## Observables (scored)

| Assert | Result |
|--------|--------|
| Work file written | yes |
| Recommended Action REVISE or PARK | **REVISE** |
| ADVANCE | **no** |
| Author axis label rejected | yes |
| 50-seed grid rejected as minimum test | yes |
| EXP-ID / STORY / EXPERIMENTS write | none |
| §H glossary recopied | no |

**Delta vs baseline (failure class):** none. Same Recommended Action
token, same no-ADVANCE, same no-EXP. Write-up mentions candidate item 8
explicitly; that is not a scored improvement (`skill-evolution.md` §D:
impressionistic “feels tighter” does not count).
