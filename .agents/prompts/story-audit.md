# Story Audit Prompt

Used when [story-maintenance](../skills/story-maintenance/SKILL.md) is about
to make a **large** change (Problem, Key Observation, or Core Idea — or a
change that would grow the method to save a failing one).

Judgment:
[idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md),
[scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md).
Story six segments remain Protocol:
[state-files.md](../references/state-files.md).

This is **not paper writing**. No Related Work section, no abstract polish,
no performance numbers in Story.

**Main writes STORY.**

## Role / Objective

Stress-test the **current belief state** before (or while) Main rewrites
core Story segments. Detect story creep, mechanism creep, post-hoc
explanation, and claim expansion. Keep **one** dominant contribution.

## When this task applies

- Large `story-maintenance`: Problem / Key Observation / Core Idea would
  move, or Core Idea is being replaced.
- The method is accumulating modules after a failure.
- A new explanation appeared only after the result disappointed.
- Open Gaps no longer look like the highest-value uncertainty.

Do **not** audit every Evidence wording tweak. Do not use this prompt to
draft a paper. Do not run it on ordinary exploratory EXP notes.

## Task fields

```text
EXP-ID: <triggering EXP, or N/A>
Story gap: <why this audit>
Change contemplated: <which STORY segments; one paragraph>
Relevant files:
  - .research/STORY.md
  - .research/DISCOVERY.md
  - .research/EXPERIMENTS.md → <triggering EXP if any>
  - .research/reviews/<EXP-ID>/ (if a large change suggests Review)
Required output: .research/work/story-audit-<slug>.md
```

## Scientific context to read

1. Current STORY six segments — the only current narrative.
2. DISCOVERY, especially Negative, Invalidated, Open Contradictions,
   Research Evolution.
3. Triggering EXP Interpretation / Story Impact only — not full run dumps.
4. [idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
   §A–C, §F–G (anchor, identity, complexity budget, deletion test).
5. [scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
   §A–C, §F (objects, claim kinds, rivals, exploratory vs confirmatory).

Do not load full EXPERIMENTS history as belief.

## Research reasoning lenses

Walk the audit questions **in order**. Answer with evidence pointers
(`STORY` segment, `EXP-xxx`, DISCOVERY line), not with hope.

Story must not get more complex **in order to save a method**.

## Critical questions

Answer each. “Unclear” is allowed; “yes” without a pointer is not.

1. **Problem clear?**
   Is the bottleneck a named failure of current belief (Open Gap, Boundary,
   or DISCOVERY contradiction) — not “we have not tried this module”
   ([idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
   §A)?
2. **Observation evidence-backed?**
   Is Key Observation still an observation, with artifacts or EXPs behind
   it — not a metric jump already narrated as theory
   ([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
   §A)?
3. **Core Idea actually addresses Problem?**
   If we ran the Idea perfectly, would *this* Problem move, or a different
   one? Off-Story gadgets fail
   [idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
   §D (off-Story).
4. **Evidence identifies mechanism?**
   Does Evidence isolate an information source or decision rule, or only
   report that Full beat a weak baseline
   ([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
   §B–C)?
5. **Boundary honest?**
   Does Boundary state non-scope, or is silence being read as generality?
   Scope inflation is a Story bug
   ([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
   §B).
6. **Open Gaps still highest value?**
   Would closing the first Open Gap change Problem, Core Idea, or route?
   Cosmetic ablations and leaderboard cells are not the top gap
   ([story-loop.md](../references/story-loop.md) gap priority).
7. **Story becoming too complex?**
   Count independent gadgets in Core Idea. If deleting one leaves “the same
   method,” identity is unidentified
   ([idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
   §F–G). **Excluded is mandatory.**
8. **New explanation added only after failure?**
   Was this mechanism named in the pre-run prediction, or minted to rescue
   a miss ([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
   §F)? Post-hoc explanations belong in a **new** EXP’s Question, not in
   today’s Core Idea.
9. **One dominant contribution still exists?**
   After the contemplated edit, is there still one claim a closest-work
   paper would have to match — or a pile of extras?

Also check, without turning them into extra Story files:

```text
story creep        — narrative grows to absorb every result
mechanism creep    — new boxes after each miss
post-hoc explanation
claim expansion    — local evidence → general law
```

## Fatal flaws / anti-patterns

- Parallel competing Stories in one file.
- Performance numbers in STORY.
- Pasting EXPERIMENTS or a literature survey into Evidence.
- Saving a costume (rename, frozen weight, extra knob, stacked unchanged
  parts) as a new Core Idea
  ([idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
  §C).
- Treating Reviewer taste or “we would be sad” as a falsifier
  ([scientific-reasoning.md](../references/research-intelligence/scientific-reasoning.md)
  §D).
- Using this audit as paper outlining.
- A subagent editing `.research/STORY.md`.

## Evidence requirements

Each “yes” on questions 2, 4, and 5 needs an `EXP-xxx` or a DISCOVERY line.
Each “no” on 7–9 needs to name the extra gadget or the post-hoc sentence.
Intuition may propose; it may not close a gap.

## Decision logic

```text
If Problem is unclear or Core Idea does not address it
  → do not apply the large edit; REVISE the contemplated change
If Evidence does not identify a mechanism
  → keep Core Idea tentative; put isolation in Open Gaps
If complexity / post-hoc / claim expansion is the real move
  → cut extras (excluded list); do not grow Story to rescue the method
If the large edit is still warranted
  → propose segment-level wording for Main
  → suggest Reviewer on the triggering EXP when stakes are high
```

This prompt does not emit Idea-gate ADVANCE/REVISE/PARK/ABANDON as Protocol.
If the contemplated Core Idea is itself a new mechanism, recommend Main load
`idea-evaluation` — do not recopy that Skill’s action glossary here.

## Required output

Write `.research/work/story-audit-<slug>.md`:

```text
## answers
1 Problem clear? ...
2 Observation evidence-backed? ...
3 Core Idea actually addresses Problem? ...
4 Evidence identifies mechanism? ...
5 Boundary honest? ...
6 Open Gaps still highest value? ...
7 Story becoming too complex? ...
8 New explanation added only after failure? ...
9 One dominant contribution still exists? ...

## creep flags
story creep / mechanism creep / post-hoc explanation / claim expansion
(each: present | absent — with pointer)

## recommended STORY edits
<segment-level proposals; no metrics; no full paper prose>

## whether to apply now
apply / hold for Reviewer / cut extras first / do not apply
```

Return answers + whether-to-apply to the caller.

## Handoff / state impact

- **Default (dispatched auditor, or Main not yet editing):** write **only**
  `.research/work/story-audit-<slug>.md`. Do not edit canonical files.
- **Main writes STORY.** Never a subagent.
- **Exception:** when **Main** is already applying `story-maintenance` in
  this session, Main may edit `.research/STORY.md` after this audit (and
  may update STATE). The audit still happens; the work file is still
  written unless the audit is so small it is inlined into Main’s STORY
  diff — even then, Main is the writer.
- Do not write DISCOVERY, EXPERIMENTS, LITERATURE, or Reviewer files from
  this prompt.

## Stop / escalation

- If evidence is contested, **hold** Story; leave tension in DISCOVERY.
- High-stakes Core Idea replacement → `idea-evaluation` work artifact
  first, then this audit, then Main’s STORY edit.
- If the honest move is to cut extras, stopping the large rewrite is the
  successful audit.
