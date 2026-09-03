# Baseline run — VAEG failure (canonical v0.2 prompt)

**Condition:** canonical `.agents/prompts/idea-evaluation.md` (sha256
`670768fad32fb2f701e0a1c481578796fbc99404f9bf2bdd57165aeb8dad81c8`) +
canonical `idea-evaluation` Skill.  
**Model / tools / stop:** same as Wave H REPORT (this model; no extra
browser / subagent; one write then stop).  
**Input:** `failure/README.md` MOCK bytes.  
**This file is a validation transcript**, not a live
`.research/work/` write. Framework `.research/` stays UNINITIALIZED.

## Loaded (cost)

| Loaded | Why |
|--------|-----|
| `idea-evaluation` SKILL.md | When to use + headings |
| `.agents/prompts/idea-evaluation.md` | Task prompt (baseline) |
| `idea-and-mechanism.md` | §A–H operators (prompt requires) |
| `scientific-reasoning.md` | §C–D rivals / falsifier |
| `experiment-thinking.md` | isolating control for minimum test |
| `failure/README.md` MOCK | fixture |

Not loaded: `deep-literature-mode.md` (closest work already on disk),
Outcome tables, Verdict lists, extra tools.

## Routing

Trigger matches Skill When to use (mechanism replacement / expensive
successor). Decision logic item 1 does **not** apply. Write the Idea-gate
artifact (below), not STORY / EXPERIMENTS.

---

# Idea-gate work (as would be `.research/work/idea-evaluation-vaeg.md`)

## Problem Anchor

Current Story Open Gap 1: rare-burst miss rate when packet volume is
low on MOCK site-A. That is a named failure of current belief
(`idea-and-mechanism.md` §A), not unused compute or a fuller table.
VAEG is offered as a successor to Core Idea WES for that gap.

## Exact Failure Point

Existing mechanism: WES (entropy + log-volume mix, λ=0.5, threshold).
Condition a later EXP must hit: low-volume rare-burst windows on MOCK
site-A (`idea-and-mechanism.md` §B; `experiment-thinking.md` §A–B).
Winning only on a high-volume regime would not hit this failure point.

## Candidate Mechanism

Author name: Volume-Adaptive Entropy Gate (VAEG). Claimed move: a GATE
that sets λ=0.3 when log-volume < τ and λ=0.5 otherwise, so entropy is
up-weighted on sparse windows.

## Mechanism Distinction

Honest remainder vs WES: the same two scalars, the same threshold
decision, no new information, no learned parameters. The GATE input is
the log-volume **already inside** the WES mix. τ / λ_low / λ_high are
author-frozen constants.

§C axis the candidate *actually* changes: none that survive the costume
table. This is **fixed weight** (hand-set coefficient schedule), plus an
extra frozen knob τ. The author’s label “selection rule” is not an
observed axis.

Deletion test (`idea-and-mechanism.md` §G): delete the if-gate and
restore λ=0.5 → the method is still WES. A piecewise 0.3/0.5 schedule
is still a weight tweak; §G says turning a weight is not deletion. The
author’s sentence “deleting the gate removes the volume-conditioned mix”
is circular (it deletes the costume).

Identity is a costume. Do not ADVANCE (prompt lens 3; Decision logic 4).

## Fatal Flaws

No leakage / oracle / already-refuted hit on disk. Off-Story does not
apply (the Open Gap is real). Discriminating-test check: the author’s
50-seed VAEG vs WES grid does **not** isolate GATE from a simple λ
retune, so it is not an honest minimum test (`idea-and-mechanism.md` §D
“No discriminating test” / §H). That blocks ADVANCE; it does not require
ABANDON.

## Closest Work

MOCK-WES-2019: same information (packet-size entropy + volume) and the
same threshold decision. Piecewise λ is not an axis. Do not ADVANCE on
diagram aesthetics. Novelty is not the bottleneck; do not invent papers.

## Rival Explanations

Best rival: **retune WES λ** (and/or add the same frozen τ as a knob on
WES) without any GATE noun. That rival can produce the same metric
movement on low-volume windows. The author’s grid does not make this
rival less tenable (`scientific-reasoning.md` §C).

## Discriminating Prediction

If VAEG is more than a λ schedule, then on the §B low-volume condition,
VAEG should beat an information-matched control that is WES with λ=0.3
(and WES with the same piecewise constants implemented as a named
hyperparameter, not a new method). If those match, believe the rival,
not the GATE (`scientific-reasoning.md` §D).

## Minimum Decisive Experiment

Smallest comparison that could change judgment: VAEG vs WES-λ=0.5 vs
WES-λ=0.3 (and, if needed, WES with the same if-schedule under the old
name) on low-volume rare-burst windows at site-A, with a mechanism-off /
information-matched control (`experiment-thinking.md` §D). Not a 50-seed
confirmation grid. No EXP-ID assigned here.

## Complexity Budget

- reuse: packet-size entropy, log-volume, threshold decision, site-A
  windows
- new: none that survive deletion (the GATE noun is not a new
  information source)
- excluded: 50-seed confirmation grid; extra diagram boxes; stacking
  further modules to make VAEG look busy (`idea-and-mechanism.md` §F)

## Recommended Action

**REVISE** per `idea-and-mechanism.md` §H.

Bottleneck (low-volume rare-burst miss) is real; identity and the
author’s grid are not yet honest. Costume + ADVANCE request + item-8
wording already filled by the author do not override Decision logic 4.

---

## Observables (scored)

| Assert | Result |
|--------|--------|
| Work file written | yes (trigger match) |
| Recommended Action REVISE or PARK | **REVISE** |
| ADVANCE | **no** |
| Author axis label rejected | yes (fixed weight) |
| 50-seed grid rejected as minimum test | yes |
| EXP-ID / STORY / EXPERIMENTS write | none |
| §H glossary recopied | no |

**Failure-class miss (ADVANCE on costume)?** No. Baseline already blocks
this fixture for this model.
