# Literature Synthesis Prompt

Deep, optional, expensive synthesis around a Story gap. Default literature
stays light ([literature-research](../skills/literature-research/SKILL.md)).
This prompt is for when novelty, conflict, or the field’s evaluation
convention would actually change belief.

Operators:
[deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md).
May also load
[idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
§C–E when closest-work identity is the bottleneck.

**Main writes `LITERATURE.md`.** Scouts write work files only.

Do not clone a survey paper. Do not create `SURVEY.md`.

## Role / Objective

Freeze research questions, search from more than one angle, and produce a
**map** — consensus, closest work, competing schools, threats, contradictions,
remaining gap, and what experiment would discriminate. Do **not** average
conflicting papers into one blended sentence.

## When this task applies

Load [deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md)
when Task field `Why deep` is novelty, conflict, convention, or a new Core
Idea — cite that file's load rules; do not restate its skip list.

Skip ordinary baseline lookup, Related Work padding, and every Open Gap.
If light literature already names a discriminating EXP, stay on
[literature-research](../skills/literature-research/SKILL.md).

## Task fields

```text
EXP-ID: <if tied to an EXP, else N/A>
Story gap: <one sentence>
Why deep: <novelty | conflict | convention | new Core Idea>
Relevant files:
  - .research/STORY.md
  - .research/LITERATURE.md
  - .research/DISCOVERY.md
  - .research/PROJECT.md
Required output: .research/work/<task-slug>.md
Search budget: work file / this task only — not a STATE field
  (owner: deep-literature-mode.md §G)
```

## Scientific context to read

1. STORY Problem / Core Idea / Boundary / Open Gaps — the condition **we**
   are in.
2. Existing LITERATURE.md — do not duplicate sections; note extends vs
   contradicts.
3. DISCOVERY Invalidated / Negative — do not “rediscover” a killed route
   from abstracts.
4. [deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md)
   §A–G (RQ freeze, angles, closest-work matrix, contradiction map, citation
   depths, novelty threat, search budget).

## Research reasoning lenses

1. **Freeze 2–4 research questions** in the work file **before** wide search
   ([deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md)
   §A). They must be answerable from papers. Silent RQ drift is confirmation
   bias; if you refine, record why.
2. Cover the **angles that apply** (§B). Budget threat-search at least as
   heavily as allied papers. Separate queries — a friend-only keyword list
   is one angle.
3. Closest work is an **axis** comparison (problem / information / mechanism /
   …), not a citation quota (§C). Mechanism × information is the interesting
   cell.
4. Contradictions are a **map**, not a mean (§D).
5. Every important citation gets a **depth label from that reference** (§E).
   Novelty threats need **methods checked** on the closest paper. Do not put
   abstract-only sources into Story Evidence or into “closest work already
   does this.”
6. Search budget is **soft** and lives in this work file / task only
   ([deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md)
   §G). Default: Pass 1 landscape, Pass 2 targeted closest-work /
   contradiction, then **stop when frozen RQs are actionable**. One further
   targeted pass only if §G allows it, and only if you write why that
   extension changes a research decision. Do not continue because more
   papers exist, the agent feels under-read, or Related Work could be
   longer. Not a paper-count enum; not a STATE field.

Citation depth labels (owned by
[deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md)
§E — use these strings; do not invent a fourth LITERATURE `Access` enum):

```text
metadata only
abstract only
full text checked
methods checked
```

Map into existing LITERATURE `Access` only when Main writes the canonical
entry; follow §E, do not recopy that mapping table here.

## Critical questions

- Which paper would most credibly kill novelty under our condition?
- Where do two important papers disagree, and on which condition
  (dataset / assumption / setting / population / metric / method)?
- What counts as a fair comparison in this community?
- What failure modes already bound the mechanism?
- What experiment would discriminate remaining disagreement **for our**
  condition?

## Fatal flaws / anti-patterns

- Averaging disagreements into “the field is mixed, so we proceed.”
- Promoting metadata/abstract depth to closest-work identity.
- Searching only for papers that flatter Core Idea.
- MECE survey outline, citation quota, or padding Related Work.
- Continuing because more papers exist, the agent feels under-read, or
  Related Work could be longer.
- Writing the search budget into STATE or inventing a paper-count enum.
- Pasting a closest-work matrix into STORY.
- Inventing `SURVEY.md` / `CLAIMS.md` or a new Access value.
- Redefining Outcome or Verdict as “novelty pass/fail.”
- Scout writing `.research/LITERATURE.md` or `.research/STORY.md`.

## Evidence requirements

Important sources: title, year, identifier (arXiv / DOI / URL), depth label,
and **which frozen RQ** they answer. Claims about methods (splits, leakage,
unit, information used) require `methods checked`. If retrieval never
reaches the result, do not use that source as a finding — a title is not a
read.

## Decision logic

```text
Cannot write two answerable RQs tied to the Story gap
  → stop; stay in light literature or return to idea-and-mechanism

Light literature already names a discriminating EXP
  → stop; do not run this prompt

Papers conflict
  → locate the difference; name OUR condition; do not blend
  → remaining disagreement → Open Gap or a discriminating EXP, not Story Evidence

Closest paper matches information + decision rule under a condition we
cannot honestly distinguish
  → novelty threat; recommend REVISE/PARK at Idea-gate (cite
    idea-and-mechanism.md §H; do not recopy that glossary) — not an Outcome

Frozen RQs actionable after Pass 1 or Pass 2
  → stop; unread papers go in “still have not read,” not a delay of a cheap
    discriminating test

A research decision is still blocked by closest work unresolved,
contradiction not localized, evaluation convention unclear, or critical
source inaccessible — and one extra pass would change revise / park /
next EXP
  → one targeted extension; record why under Search budget; then stop

More papers exist / feel under-read / Related Work could be longer
  → not an extension reason; stop
```

## Required output

Write `.research/work/<task-slug>.md` with **all** of the following headings
(empty is allowed only if the angle does not exist in this field — say so):

```text
## Research Questions
<2–4 frozen RQs; any mid-search change + why>

## Known consensus
<what is actually established, at stated scope>

## Closest work
<axis comparison; mechanism × information; depth labels>

## Competing schools
<distinct mechanisms or evaluation religions, not a name cloud>

## Critical evidence
<strongest empirical tests, not the friendliest abstracts>

## Failure modes
<negative results, retracted assumptions, “did not help”>

## Benchmark conventions
<what “fair” means here: splits, metrics, leakage, grouping unit>

## Novelty threats
<the paper most likely to kill novelty; must be methods-checked if claimed>

## Contradictions
<map, not average: Paper A vs Paper B on a named condition; our condition>

## Remaining gap
<what is still ours to answer>

## Experiment implications
<what would discriminate; must-run vs nice vs cut in prose — no new EXP-ID>

## Search budget
<Pass 1 landscape done? Pass 2 targeted done? RQs actionable?>
<if extending: which §G reason, and why the extra pass changes
revise / park / next EXP — not “more papers exist”>
```

Also include:

```text
## citation depths
- <source> — <metadata only | abstract only | full text checked | methods checked>

## still have not read
<list; not a reason to postpone a cheap discriminating test>
```

Return remaining gap + experiment implications + novelty threats to the
caller.

## Handoff / state impact

- **`literature-scout` / any deep-mode agent:** work file only.
- **Main writes `LITERATURE.md`.** Integrate only **valuable** papers into
  existing template fields (Reference, Identifier, Access, Relation to Our
  Story, Relation to Experiments, …). Do not paste this whole synthesis into
  STORY or DISCOVERY.
- Main may briefly update STATE if routing changed. Story wording changes
  go through `story-maintenance`, not this prompt.
- Do not assign EXP-IDs. Do not write Outcome. Do not write Reviewer files.

## Stop / escalation

- Stop when frozen RQs are actionable (usually after Pass 2; earlier if
  already decidable). One targeted extension only per
  [deep-literature-mode.md](../references/research-intelligence/deep-literature-mode.md)
  §G, and only with a written reason that the extra pass changes a
  research decision. Do not continue because more papers exist, the
  agent feels under-read, or Related Work could be longer.
- If the matrix shows no real mechanism/information axis → load
  `idea-and-mechanism.md`; do not invent a cosmetic difference.
- If the contradiction map names a discriminating test →
  `experiment-design` / [experiment-thinking.md](../references/research-intelligence/experiment-thinking.md),
  not more papers.
- If a literature claim is about to enter Story Evidence →
  `evidence-verification`; a citation is only as strong as the artifact it
  points to.
