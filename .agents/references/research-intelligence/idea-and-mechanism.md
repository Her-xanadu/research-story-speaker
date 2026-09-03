# Idea and Mechanism

Layer 2 — Research Intelligence. How to tell a real mechanism from a costume.

**Not Protocol.** Do not redefine Story structure, Experiment Outcome, or
Reviewer Verdict. Do not create `HYPOTHESES.md`. Idea reports belong in
`.research/work/` until the Main Agent integrates.

Load with a **new Core Idea**, route competition, mechanism replacement,
major pivot, expensive successor, or high-stakes method change. Skip for
routine seeds, small ablations, bugfixes, and simple replications.

Cross-links: scientific objects in `scientific-reasoning.md`;
discriminating tests in `experiment-thinking.md`; novelty search in
`deep-literature-mode.md` when the closest-work question is actually the
bottleneck.

---

## A. Problem Anchor

Every new Idea must answer:

> What is the actual scientific bottleneck *now*?

Unacceptable anchors:

```text
I want to add a module.
This architecture is popular.
We have unused compute.
The name sounds more novel.
The ablation table would look fuller.
```

Acceptable anchors point at a **named failure of current belief**: a Story
Open Gap, a Boundary that blocks completion, or a DISCOVERY contradiction
that would change Core Idea if resolved.

If the bottleneck is “we have not tried this trick yet,” park the Idea
until the bottleneck is scientific. A popular diagram is not a Problem.

---

## B. Exact failure point

State, specifically:

```text
Which existing mechanism fails?
Under what data, split, or experimental condition does it fail?
```

“The baseline is not SOTA” is not a failure point unless the Story’s
Problem is SOTA chasing. “Accuracy is low” is not a failure point without
the **condition** (distribution shift, rare class, site, capture, budget,
latency envelope).

If you cannot name the failing mechanism *and* the condition, you cannot
yet claim a successor mechanism. You may still run a cheap exploratory
probe — that is not a Core Idea replacement.

Write the failure point so a later Experiment can hit the same condition
(`experiment-thinking.md` §A–B). A successor that only wins on a different
regime has not addressed this failure point.

---

## C. Mechanism identity

Ask *where* the method actually changes. Typical axes — use only those
that apply; do not score all of them:

```text
information source
selection rule
objective
representation
optimization
aggregation
architecture
decision policy
training dynamics
```

A real identity shift is a change in **what information is used or how
decisions are made**, plus a prediction that would fail if that change
were removed.

Then check whether the “new method” is only cosmetic:

| Costume | Tell |
|---------|------|
| **renaming** | Same information flow and decision rule; new noun in the diagram |
| **fixed weight** | A coefficient frozen or hand-set; no new selection or information |
| **extra hyperparameter** | A knob any method could grow; the claim is “we tuned” |
| **component stacking** | Unchanged parts concatenated; neither part is the claim |
| **post-hoc routing** | Hard cases sent elsewhere after errors are seen; the router is not the mechanism |
| **cosmetic objective rewrite** | Algebraically equivalent loss, or a rename of the same training signal |

If the only honest difference is a name, a frozen coefficient, or an extra
knob, **mechanism identity is weak**. Do not ADVANCE a Core Idea on that
basis. Stacking unchanged parts is still stacking.

Identity is not “we drew a new box.” Identity is the axis a closest-work
paper would have to match to threaten novelty (§E).

---

## D. Fatal Flaw Audit

Run this **before** designing a large Experiment. Fatal beats eloquence.
One hit is enough to stop ADVANCE.

| Check | Fail if |
|-------|---------|
| Already refuted | DISCOVERY Negative / Invalidated, or LITERATURE, already covers this mechanism under the same condition |
| Unobtainable information | Relies on labels, oracles, future packets, or privileged side channels the deployment cannot have |
| Obvious leakage | Uses test-split statistics, identity, site ID, capture ID, or grouping-unit membership as a feature without declaring it |
| No discriminating test | No result would lower belief (`scientific-reasoning.md` §D) |
| Cost vs RESOURCES | The smallest honest test exceeds authorized compute, data, or access |
| Off-Story | Does not address the current Problem / Open Gaps |

A single fatal flaw → do not ADVANCE; pick REVISE / PARK / ABANDON per §H.

Those four actions (`ADVANCE` / `REVISE` / `PARK` / `ABANDON`) are
**Idea-gate recommendations only**. They are not Protocol enums. They are
not Experiment Outcome values. They are not Reviewer Verdicts. Do not
write them into `EXPERIMENTS.md` or into a review artifact as if they
were. Main Agent decides whether STORY / EXPERIMENTS change.

Do not import external scoring taxonomies (improvement-dimension
scorecards, paper-type ladders, or venue-style accept/reject labels).
This audit is Story-anchored and fatal-first.

---

## E. Closest-work axis

At least:

```text
What is the closest existing method?
On which mechanism axis does ours differ?
```

No quota of “must cite five papers.” One correctly compared closest
method beats an unread bibliography. If novelty matters (new Core Idea,
publication-facing claim, literature conflict), escalate to
`deep-literature-mode.md` instead of guessing.

If the closest work already implements the same information flow and
decision rule, the Idea is a **novelty threat** even if the diagram looks
new. REVISE the identity or PARK until a real axis is named. A different
dataset with the same mechanism is usually not a new mechanism.

Closest-work is an axis comparison, not a survey deliverable. Do not turn
this section into a mandatory deep literature pass.

---

## F. Complexity budget

Record three lists before experiments balloon:

```text
reuse     — inherited components (keep)
new       — the actual addition
excluded  — tempting extras we will not add this round
```

**Excluded is mandatory.** Agents otherwise accumulate modules to rescue a
weak core. One major mechanism beats many decorative components.

If `new` has more than one independent gadget, ask which one is the claim.
The others belong in `excluded` or a later EXP. “We might need it later”
is not a reason to keep it in `new`.

The budget is a thinking record in the idea-evaluation work artifact. It
is not a new canonical state file.

---

## G. Deletion test

> If we delete the new component, is the method still basically the same thing?

If **yes**, the innovative mechanism has not been identified. The Idea is
cosmetic or under-specified. Do not treat an ablation that *must* change
nothing as evidence of contribution.

If deletion would remove the information source or decision rule named in
§C, identity is at least coherent — then design the minimum test in §H.

Deletion is the identity check. Parameter sweeps are not a deletion test.
Turning a weight from 0.7 to 0.3 is not deletion. Replacing the claimed
component with a sham that keeps cost and appearance is a later
experimental move (`experiment-thinking.md` §D, §G), not a substitute for
naming what would disappear.

---

## H. Minimum decisive experiment

Ideas must not jump to large grids.

Find:

```text
smallest test that could change judgment
```

Usually: one comparison that includes a **mechanism-off** or
**information-matched** control (`experiment-thinking.md` §D–E), on the
**failure condition** from §B, at the correct experimental unit.

If that test cannot be run, the Idea is not ready for expensive
confirmation. Exploratory pilots may generate signal; they do not replace
this step. A 50-seed leaderboard that cannot split rivals is not
“more decisive.”

Recommended action after the audit (work artifact only):

```text
ADVANCE  — identity clear, no fatal flaw, minimum test specified
REVISE   — bottleneck real, flaw fixable; mechanism or test not yet honest
PARK     — not now (resources, sequencing, novelty check pending)
ABANDON  — bottleneck real, this mechanism cannot be the answer
```

Again: these four strings are Idea-gate recommendations. They are **not**
Protocol. Main Agent writes canonical state; this file never authorizes
creating an EXP, rewriting Core Idea, or skipping Reviewer contract.

If the honest action is PARK or ABANDON, stopping is progress. Adding
components until the Idea looks busy is not.
