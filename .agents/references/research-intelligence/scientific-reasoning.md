# Scientific Reasoning

Layer 2 — Research Intelligence. How a careful researcher thinks.

**Not Protocol (Layer-2 boundary; other RI files cite this paragraph).**
This file owns scientific *objects* and *speech calibration*. It does not
own workspace enumerations.

- Experiment Outcome values live only in `experiment-record.md` §Outcome 值.
- Reviewer Verdict lives only in `reviewer.md` §Verdict.
- Story's six segments live only in `state-files.md`.
Do not copy those lists into Layer 2. Do not add extra canonical state files
(`HYPOTHESES.md`, `RUBRIC.md`, `CLAIMS.md`, `FINDINGS.md`, `SURVEY.md`).
Do not create a second Outcome table “for convenience.”

Load when constructing hypotheses, attributing a mechanism, splitting a
Story into claim kinds, or calibrating how strongly to speak. Do **not**
preload this file or the rest of `research-intelligence/` on every cold
start or loop iteration (`AGENTS.md` Skill Routing; `research-loop`).
Skip a one-line sanity rerun whose question is already on disk.

---

## A. Scientific objects

Keep these nine distinct. Mixing them is how a Story becomes unfalsifiable.

| Object | Meaning | Typical collapse |
|--------|---------|------------------|
| **Observation** | What was seen, measured, or reported, *before* theory | Treating a metric jump as already explained |
| **Research Question** | What we need to know next | Disguising a method preference as a question |
| **Hypothesis** | A testable claim about how the world behaves | A slogan (“module X will help”) with no prediction |
| **Mechanism** | The causal story: which information or process produces the effect | Correlation, architecture name, or a loss term treated as mechanism |
| **Prediction** | What should happen *if* the hypothesis is true, stated *before* seeing the result | Rewriting the prediction after the run |
| **Alternative Explanation** | A rival account of the *same* observation | Listing only the favorite mechanism |
| **Null Hypothesis** | The boring default: no effect, no difference, or “shared design did it” | Treating null as “the method is worthless” |
| **Evidence** | Artifacts and comparisons that could *change belief* | A plot that exists but does not answer the question |
| **Claim** | What we now assert, at a stated scope | Inflating a local result into a general law |

Hard inequalities — do not blur:

```text
Observation ≠ Interpretation
Prediction ≠ Evidence
Correlation ≠ Mechanism
Metric movement ≠ Causal explanation
```

How to use them (ML, systems, and network-security work alike):

- An F1 lift, a latency drop, or a higher detection rate is an
  **observation**. The sentence that says *why* it moved is interpretation.
- A **prediction** is a bet recorded before the run. The number you later
  read is not that bet; it is a candidate piece of **evidence**.
- Two quantities moving together is correlation. A **mechanism** names the
  process that should still produce the effect if the correlate were
  removed or scrambled.
- A metric moving in the hoped-for direction does not explain itself. It
  becomes evidence *for a mechanism* only after rivals that could also
  move the number are made less plausible (§C).

These objects are thinking tools. They do not become extra canonical
fields. Canonical Story structure remains the six segments owned by
`state-files.md`.

### Quantity vs proxy

Use rules for how Observation, Evidence, and Claim *talk about a
quantity* — not a tenth scientific object, not extra canonical fields,
not a review gate.

When first introducing, replacing, or changing the use of a measured
quantity, state in existing design prose: **source**, **object**,
**direction**, and **intended use**. If the quantity is already defined
and the inputs have not changed, cite that definition. Do not invent a
new EXP field for these four words.

Distinguish information used for training, simulation, measurement, or
inference from information used only for *post-hoc verification*. A
quantity that arrives after the decision is not the input that produced
the decision.

When only a **proxy** is obtainable, a proxy hypothesis is allowed.
State how the proxy differs from the original quantity and what bridging
still has to be tested. Do not confer the original quantity's validity
on the proxy. Do not silently replace a core input so the pipeline can
run.

```text
Keep the measured quantity, its intended meaning, and its use consistent across proposal, implementation, and interpretation. A proxy may support a new exploratory hypothesis; it does not automatically inherit the original quantity's calibration, causal meaning, or evidence.
```

Heuristic scores need not be probability-calibrated before exploration.
A semantic note is not a new review gate. Narrow the Question, or rewrite
it as a proxy hypothesis, only when missing premises make the original
question unanswerable.

If a score measures degree of anomaly but the proposal treats it as a
probability of treatment benefit: name the inconsistency; rewrite the
hypothesis or state the bridge still to verify. Do not design operations
that depend on that benefit probability. Do not start a project-wide
qualification audit of every score.

---

## B. Claim decomposition

The current `STORY.md` is not one blob. Split it when asking what is weak.
Segment *names* are Protocol (`state-files.md`). The *claim kinds* below
are this file’s lens; they must not redefine Story.

| Claim kind | Usually inspected in | Example shape |
|------------|----------------------|---------------|
| **Problem claim** | Problem | This bottleneck is real and worth the next experiment |
| **Observation claim** | Key Observation | This failure or pattern occurs under named conditions |
| **Mechanism claim** | Core Idea | The named process is why the method should work |
| **Performance claim** | Evidence (do not copy metrics into Story) | Under the stated setup, the method beats the stated comparison |
| **Efficiency claim** | Evidence / Boundary | Same or better behavior at lower cost, data, or latency |
| **Generality claim** | Boundary | Holds beyond the training draw, site, capture, or traffic mix |
| **Boundary claim** | Boundary | Explicitly does *not* claim X |

After reading Story, ask:

> Which claim kind currently has the weakest direct evidence?

That question is the operator. Do not invent numeric claim scores.

Do not prop up a mechanism claim with a performance number that a weaker
baseline, extra compute, or extra capacity could also produce. Do not
treat silence in Boundary as a generality claim. An honest Boundary line
is a claim of *non*-scope, not leftover text.

If the weakest kind is the Problem or Observation claim, a new module
will not fix it. If it is the Mechanism claim, a leaderboard will not
fix it. If it is Generality, another seed on the same draw will not
fix it.

---

## C. Competing explanations

Any **important positive result** needs at least one serious rival, chosen
for *this* task — not a mandatory ten-item chant.

Candidate rivals (use only those that could actually produce the same
pattern):

```text
target mechanism
simpler mechanism
data artifact
baseline weakness
optimization effect
capacity difference
random seed
measurement issue
leakage
selection effect
```

How to pick, not how to recite:

1. Name the **best** rival — the one a skeptical colleague would actually
   offer. In representation learning that is often capacity, tuning
   budget, or leakage; in systems, measurement method or workload mix;
   in network security, capture identity, site, or split leakage. Skip
   rivals that cannot generate *this* pattern.
2. Ask which **control or split** would make that rival less tenable.
   Designing that comparison is `experiment-thinking.md`.
3. If no such test exists, the mechanism claim is not ready for Story
   Evidence.

Negative and null results have rivals too (underpowered run, bug, wrong
metric, regime mismatch). Do not assume “it failed, therefore the idea is
false,” and do not assume “it failed, therefore keep sweeping
hyperparameters.” Collapse of every failure into one story is the same
error as collapse of every success into the favorite mechanism.

---

## D. Falsifiability

For any hypothesis that would change Core Idea or route, answer:

> What result would make me *lower* belief in this mechanism?

If no such result is conceivable, the hypothesis is **not falsifiable**.
Do not promote it into Core Idea. Park it as speculation, or rewrite until
a discriminating prediction exists.

A falsifier must be **observable in an Experiment**. “We would be sad” is
not a falsifier. “The Reviewer might dislike it” is not a falsifier.
Outcome vocabulary for the completed Experiment stays in
`experiment-record.md`; this section only demands that *some* observable
pattern would count against the mechanism.

A prediction that can only be “confirmed” — every outcome spun as support
— is not a prediction. If both “the metric went up” and “the metric went
down” can be narrated as success, stop and rewrite the hypothesis.

---

## E. Evidence strength (qualitative)

Do not invent numeric scores, stars, or percentages of belief. Rank *this*
support with the **weakest applicable** label:

```text
direct discriminating experiment
  — comparison designed to split target vs a named rival
replicated controlled evidence
  — same claim, more than one honest run or site, controls held
single controlled result
  — one fair comparison with the right unit of analysis
pilot
  — cheap signal; may generate hypotheses; does not confirm them
observational association
  — co-occurrence without isolation
literature analogy
  — someone else’s setting; transfer is itself a claim
model reasoning
  — theory or architecture argument without a discriminating test
intuition
  — allowed to propose; not allowed to close a Story gap
```

A beautiful figure of a pilot is still a pilot. Story Evidence should not
rest on intuition or literature analogy alone when a discriminating
experiment is feasible. Matching a file on disk to a claim is
`evidence-and-claim.md`; this ladder only says how *strong* a given kind
of support is.

Do not average labels. The claim is as strong as its weakest necessary
rung.

---

## F. Exploratory vs confirmatory

```text
exploratory  — may generate hypotheses and new Open Gaps
confirmatory — may only test hypotheses that were explicit *before* the run
```

**Forbidden:** treating a post-hoc slice, lucky seed, after-the-fact
metric, or a subgroup found while browsing as if it had been the
preregistered prediction.

**Allowed:** an exploratory run that *discovers* a pattern, recorded as
an observation, then a **new** Experiment whose Question and Prediction
are written before the confirming runs.

If the EXP section never stated a prediction, analysis may still report
what happened. It must not pretend the observation was confirmatory
support for a mechanism named afterwards. Speak with §G (`suggestive` or
`inconclusive`) rather than laundering the finding into Story Evidence.

Exploratory work stays light. It does not automatically trigger
idea-evaluation, evidence-verification, or an independent Reviewer.

---

## G. Uncertainty calibration (wording, not Protocol)

These words guide how strongly to *speak* in work notes, Story prose, and
discussion. They are **not** Experiment Outcome values, **not** Reviewer
Verdicts, and **not** Status values.

Do not write them into `EXPERIMENTS.md` as Outcome.
Do not write them into a review artifact as Verdict.

| Word | Use when |
|------|----------|
| **established** | Repeated discriminating evidence; serious rivals addressed |
| **supported** | Fair test came out as predicted; scope still local |
| **suggestive** | Directionally consistent; rivals or power still open |
| **inconclusive** | Cannot tell; more discriminating design needed |
| **contradicted** | Fair test went the other way |
| **invalid** | The measurement cannot be used (leakage, wrong object, broken run) |
| **unknown** | Not yet tested |

Overlap of English words with Protocol tokens is accidental and must not
be exploited. If an EXP needs an Outcome, look up `experiment-record.md`
§Outcome 值 and write *that* enum. If a review needs a Verdict, look up
`reviewer.md` §Verdict. This table never maps one-to-one onto either list,
and it is not a hidden Status machine.

---

## Habits (judgment, not a checklist gate)

Before believing a positive result: what simpler explanation remains?
Before a mechanism claim: which comparison isolates it?
Before updating Story: which claim kind is actually evidenced, at what scope?
After a failure: did engineering fail, or did the hypothesis fail?

These questions do not add a workflow step and do not authorize writing
canonical state. Ordinary exploratory work stays light.

Load `idea-and-mechanism.md` when asking whether an idea is a real
mechanism. Load `experiment-thinking.md` to design the discriminating
test. Load `evidence-and-claim.md` to match artifacts to claims.
