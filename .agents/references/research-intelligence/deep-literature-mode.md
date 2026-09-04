# Deep Literature Mode

Layer 2 — Research Intelligence. How to search when novelty or landscape
actually matters.

**Not Protocol.** Layer-2 boundary: cite `scientific-reasoning.md`.
LITERATURE.md field shapes stay in `state-files.md` and
`LITERATURE.template.md`. This file must not invent a bibliography store,
survey file, or taxonomy file.

**License (binding).** Upstream `deep-research` is **CC-BY-NC-SA-4.0**. This
file takes *ideas only*: freeze questions before a wide search; look from
more than one angle; check how deeply a citation was actually read; hunt
the paper that would kill novelty. **Fully original wording.** Do not paste
upstream procedures, gate tables, hedge ladders, or a MECE survey-paper
skeleton. The deliverable is **not** a survey-paper clone.

This mode is **OPTIONAL EXPENSIVE**. It is not default literature.
Default work stays light (`literature-research` Mode `light`). Deep is
that Skill's optional switch, not a new Skill. A handful of highly
relevant sources that already name a discriminating experiment is
enough to leave this file unread.

Load only when: novelty is unclear, a new Core Idea is proposed, papers
conflict in a way that would change Story, or the *field's evaluation
convention* is the bottleneck. Skip for a 3-paper sanity check before a
cheap EXP, for ordinary baseline name lookup, and for “write Related Work.”

Objects: `scientific-reasoning.md`. Mechanism identity:
`idea-and-mechanism.md` §C–E. Discriminating tests: `experiment-thinking.md`.

---

## A. RQ freeze

Before any deep search, freeze **2–4 research questions** on disk (a
`.research/work/` memo or the literature-task prompt output). Do **not**
create a new canonical state file for RQs.

Questions must be answerable from papers, for example:

```text
What method already claims this job under our condition?
Where is the strongest conflicting evidence, and under what split?
What failure modes or negative results bound the mechanism?
What does this community treat as a fair comparison?
```

Not a research question:

```text
Is our Idea cool?
Can we cite enough papers to look thorough?
What is the entire history of this field?
```

You may refine the frozen RQs mid-search **only** if you **record why**:
a new contradiction appeared; the population or capture regime was wrong;
the method family split; a question as stated is unanswerable from the
literature. Silent RQ drift is how a search becomes confirmation bias.

If you cannot write two answerable questions tied to the current Story
gap, you are not ready for this mode. Stay in light literature, or return
to `idea-and-mechanism.md` until the bottleneck is named.

---

## B. Multi-angle search

Cover the angles that **apply**. Skip an angle that does not exist in this
field rather than inventing empty rows. Do not treat the eight names as a
mandatory ritual.

```text
direct method           — papers that claim the same scientific job
closest competitor      — best alternative mechanism for that job
critical evidence       — strongest empirical tests, not the friendliest
negative result         — failures, retracted assumptions, “did not help”
adjacent field          — same information flow, different application
methodological paper    — leakage, unit-of-analysis, evaluation warnings
benchmark convention    — what “fair” means here (splits, metrics, leakage)
application literature  — deployment constraints that kill lab Ideas
```

Adversarial default: spend at least as much effort on **threats** as on
support. A pile of similar positive abstracts is not a landscape.

Search the angles with **separate** queries. One shared keyword list that
only finds friends is a single angle wearing eight labels.

In ML / systems / network security, typical mappings:

```text
direct method        — papers on this detection / scheduling / routing job
closest competitor   — a different feature or decision rule for the same job
critical evidence    — papers that stress-test under shift or hard traffic
negative result      — ablations that removed the claimed signal
adjacent field       — vision / NLP / control using the same information idea
methodological paper — pcap grouping, site leakage, identity features
benchmark convention — CIC / campus traces / encrypted backbone splits
application literature — inline latency, encrypted payloads, missing labels
```

After Pass 1, follow **names the hits revealed** (method names,
dataset names, shared citations). That follow-up **is** Pass 2 of the
search budget (§G), not an unbounded third survey. An angle that
returns almost nothing may be a real gap *or* a bad query; try one
renamed query **inside the current pass** before declaring a hole.

---

## C. Closest-work matrix

Compare **our Idea / Story** to each close paper on the axes that apply:

```text
problem
information
mechanism
objective
training
evaluation
limitation
```

The interesting cell is **mechanism × information**. Same diagram, same
loss, different dataset is usually *not* a new mechanism
(`idea-and-mechanism.md` §C–E). Same backbone with a renamed head is
usually not a new mechanism. A new training schedule with the same
decision rule is usually not a new mechanism.

No required citation count. One correctly compared closest paper beats
ten unread bibitems. Fill only the columns that exist; do not pad.

How to read a row:

```text
match on problem + information + mechanism
  → novelty threat (§F); do not invent a cosmetic difference

match on problem, differ on information or decision rule
  → possible identity; name the axis and the discriminating test

differ only on training recipe or extra capacity
  → not a Core Idea replacement; maybe a cheap EXP, maybe PARK
```

Write the matrix in the work artifact. Do not paste it into STORY.
When a paper is kept, LITERATURE.md gets the existing template fields
(Relation to Our Story / Relation to Experiments), not a second matrix
schema.

---

## D. Contradiction map

When papers disagree, do **not** average their conclusions. Do not write
“the literature shows mixed results, so we proceed.” Mixed is a map, not
a mean.

Locate the difference on:

```text
dataset
assumption
setting
population
metric
method
```

Write the map as a condition, not a blend:

> Paper A finds X on campus traffic with flow-level splits.
> Paper B finds not-X on encrypted backbone with capture-level grouping.
> Metrics differ (AUROC vs operational FPR at fixed budget).

Then name **our** condition (the Story Problem / dataset / unit). If our
condition matches A, B does not refute us; if it matches B, A is not
support. Remaining disagreement becomes an Open Gap or a discriminating
EXP (`experiment-thinking.md`), not a blended sentence in Story Evidence.

Do not “synthesize an effect size” across mismatched populations or
metrics. In security traces, **site / capture / day** are often the
hidden split between two papers that look like they study the same task.

---

## E. Citation verification depths

Record how deep each **important** citation was checked. These depths are
**judgment labels** in the work artifact. They are not a new LITERATURE
enum and they do not redefine the template `Access` field.

```text
metadata only      — title / authors / year / venue; weakest
abstract only      — claims without methods
full text checked  — read enough to locate the result in the paper
methods checked    — splits, leakage, unit, baselines, information used
```

Mapping into existing LITERATURE.md `Access` (Protocol / template):

```text
metadata only      → Access: metadata-only
abstract only      → Access: abstract-only
full text checked  → Access: full-text-checked
methods checked    → Access: full-text-checked
                  plus a note that methods were inspected
                  (Important Finding / work memo — not a fourth Access value)
```

Do not put abstract-only papers into Story Evidence, or into “closest
work already does this,” as if methods were checked. Novelty threats
(§F) need **methods checked** on the closest paper.

If a source cannot be retrieved far enough to locate the result, it is
**unused** as a finding. Guessing from a title is not literature.

Identifier / Access / Source when incorporating a paper follow the
existing template. Do not invent a parallel bibliography.

---

## F. Novelty threat search

Actively search:

> Which paper would most convincingly show our Idea is not new?

That is the paper to methods-check. Ask it on the closest-work axes, not
on diagram aesthetics. If it already uses the same information and the
same decision rule under a condition we cannot honestly distinguish, the
Idea is a novelty threat even if our figure looks different.

Then recommend one action per `idea-and-mechanism.md` §H. A novelty
threat with no real mechanism or information axis maps to REVISE or PARK
per that owner, not ADVANCE. Recommend ADVANCE from this mode only when
the matrix shows a mechanism or information difference and a
discriminating test exists. These are Idea-gate recommendations, not
Outcome and not Verdict.

Deep mode ends with, in the work artifact:

```text
remaining gap
experiment implications (what would discriminate)
what we still have not read
```

It does **not** write STORY. It does not dump a survey into DISCOVERY.
Main integrates valuable papers into LITERATURE.md. Routing after that
is the existing loop (`literature-research` → experiment-design or
story-maintenance), not a new literature state machine.

---

## G. Search budget (soft)

This is a **soft attention budget**. It is **not** a paper-count
Protocol enum, **not** a LITERATURE field, and **not** a STATE field.
Write it only in the work artifact or the literature-task context.

Default shape:

```text
Pass 1 — broad landscape   → Main: paper-find bounded queue + ingest (if consult inadequate)
Pass 2 — targeted closest-work / contradiction → second find pass only if still blocked
```

Pass 1 covers the angles that apply (§B) **through local consult first**, then one
`paper-find` pass when needed. Pass 2 is a **second `paper-find` pass** (targeted
queries), not scout-side web search. Literature-research owns transport; this file
owns **whether** to open Pass 1, Pass 2, or one extension — not arXiv/S2 API steps.

If light literature already names a discriminating experiment, **do not
enter this mode**. Deep literature is for uncertainty that would change
Core Idea, novelty, or evaluation convention — not for delaying a cheap
must-run test, not for padding Related Work, and not for “the agent
feels under-read.”

**Stop** when the frozen RQs are **actionable**: answered well enough to
choose revise the Idea, park it, or design the next EXP. Unread papers
go in “still have not read”; they are not a reason to postpone a cheap
test that would change belief. Stop at the end of Pass 1 if the RQs are
already actionable — Pass 2 is not a ritual.

### One extension

After the default passes, **one** further targeted pass is allowed
**only if** a research decision is still blocked by one of:

```text
closest work unresolved
contradiction not localized
evaluation convention unclear
critical source inaccessible
```

If you extend, write in the work artifact **why the extra pass would
change a research decision** (which frozen RQ; which choice among
revise / park / which EXP). An extra pass that cannot change that
choice is not allowed. After the extension, stop.

Do **not** extend, and do not continue the default passes, because:

```text
more papers exist
the agent feels under-read
related work could be longer
```

`Pass 1` / `Pass 2` / `extension` are judgment notes in the work file.
They are not Status, not Outcome, not Verdict, and not a new STATE
machine.

---

## Anti-patterns

- Running this mode on every Open Gap.
- Cloning a survey-paper outline, MECE tree, or citation quota.
- Averaging contradictory papers into one Story sentence.
- Treating metadata or abstracts as methods-checked closest work.
- Searching only for papers that agree with Core Idea.
- Inventing a fourth Access enum or a `SURVEY.md` / `CLAIMS.md`.
- Pasting the closest-work matrix into STORY.
- Redefining Outcome or Verdict to encode “novelty pass/fail.”
- Continuing because more papers exist, the agent feels under-read, or
  Related Work could be longer.
- Writing the search budget into STATE, or inventing a paper-count enum.

---

## Using this file

Work output lives in `.research/work/` until Main writes LITERATURE.md.
This file owns the *judgment operators* (freeze, angles, matrix, map,
depth, novelty threat, search budget / stop / one extension). The Skill
owns search tools, the light|deep switch, template fields, and the
prohibition on writing STORY. Budget / stop / extend live in the work
artifact or task context only — never as a STATE field.

Load `idea-and-mechanism.md` if the matrix shows no real axis.
Load `experiment-thinking.md` if the map names a discriminating test.
Load `evidence-and-claim.md` before a literature claim enters Story
Evidence — a citation is still only as strong as the artifact it points
to.
