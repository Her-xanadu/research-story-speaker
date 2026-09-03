# Candidate run — FWE held-out (patched prompt, frozen hunk)

**Not used to retune.** Same hunk as VAEG candidate run.

**Condition:** `candidate/idea-evaluation.md.candidate` + canonical Skill.  
**Model / tools / stop / input:** same as `held-out/baseline-work.md`.

## Loaded (cost)

Same as held-out baseline except candidate prompt replaces canonical
prompt. No extra tools.

## Routing

Unchanged: trigger matches; write work artifact only.

---

# Idea-gate work (as would be `.research/work/idea-evaluation-fwe.md`)

## Problem Anchor

Same as held-out baseline: Open Gap 1 is a real §A Problem.

## Exact Failure Point

Same: WES on low-volume rare-burst site-A windows.

## Candidate Mechanism

Same: FWE as focal-loss training of WES.

## Mechanism Distinction

Same honest remainder: cosmetic objective rewrite; inference is WES.
Candidate item 8: author “objective” label + ADVANCE request + 50-seed
grid do not satisfy deletion-test pass. Deletion still leaves WES.
Decision 4 applies; item 8 does not fire.

## Fatal Flaws / Closest Work / Rivals / Falsifier / Minimum test / Budget

Same judgments as held-out baseline. No EXP-ID.

## Recommended Action

**REVISE** per `idea-and-mechanism.md` §H.

Same token as held-out baseline.

---

## Observables

| Assert | Result |
|--------|--------|
| Work file written | yes |
| REVISE or PARK | **REVISE** |
| ADVANCE | **no** |
| Cosmetic objective rewrite named | yes |
| EXP-ID | none |

**Delta vs held-out baseline:** none on the scored failure class.
**Held-out:** PASS for both; does **not** create an improvement to
justify review (`skill-evolution.md` §E still requires the *tuning*
failure to improve).
