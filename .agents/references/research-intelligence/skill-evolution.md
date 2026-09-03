# Skill Evolution

Layer 2 — Research Intelligence. How this *framework* may change a Skill
or a prompt without turning every friction into a new Skill, a new state
file, or a silent rewrite of frozen core.

**Not Protocol.** Outcome lives only in `experiment-record.md`. Verdict
lives only in `reviewer.md`. Story six segments live in `state-files.md`.
This file must not copy those enumerations.

**Not a Skill.** V0.1.1 has 10 scientific/workflow Skills; V0.2 will add
exactly 2 later (`idea-evaluation`, `evidence-verification`). This Layer-2
file is none of those and is not a research-loop step. Project science
stays in `.research/`. Evolution records stay in `docs/validation/`.
Never mix Skill maintenance into DISCOVERY, STORY, or LITERATURE.

This is instruction-only evolution of *agent workflow*. It is not a
scientific method, not an experiment Outcome, and not a Reviewer Verdict.

Intended callers are `framework-maintenance` modes (Wave D will name
`session-diagnosis`, `skill-evolution`, `regression-eval`). Until those
modes exist, this file is still the owner of the evolution *judgment*.
Do not create a parallel Skill that “runs science better.”

Upstream doctor / upper / experience-to-skill ideas were not copied;
this is local protocol for bounded, reviewable instruction changes.

---

## A. Trigger

Propose a Skill or prompt change only if **one** of these holds:

```text
≥ 2 independent repeated problems
```

or:

```text
a deterministic reproducer + a protection case
```

**Independent** means different sessions, harnesses, or users, same
*failure class* — not the same chat repeating the same complaint, and
not two logs of one broken run. A user preference about a scientific
claim is not a trigger. A user preference about *agent workflow* can
become a trigger only if it repeats.

**Deterministic reproducer:** given a frozen fixture (files + prompt +
expected miss), the current instruction produces the miss. Vague “it
felt heavy” is not a reproducer.

**Protection case:** a situation that must remain *light* after the
change (see §C). A reproducer without a protection case invites fixing
the miss by firing every gate.

Not a trigger:

```text
one ugly session
“an upstream Skill looked thorough”
taste (“I would have written a longer prompt”)
a scientific disagreement that belongs in DISCOVERY
```

`research-memory` may *notice* repeated friction and leave a note under
`.research/work/` (project) or `docs/validation/` (framework repo). It
must not edit Skills.

---

## B. Atomic change

One change set = **one Skill or one prompt file**, **one behavior**.

Do **not** simultaneously change:

```text
workflow
prompt
state schema
reviewer contract
tooling
```

If the bug needs two layers, ship the first atomic candidate, review it,
then consider the second. “While we are here” is how frozen core dies.

Protocol owners (`experiment-record.md`, `reviewer.md`, `state-files.md`,
`story-loop.md`, `git-linking.md`) are frozen unless the bug is truly in
Protocol. Intelligence files change only when the **judgment operator**
was wrong — not to add another checklist.

Prefer a **link** from SKILL.md to an intelligence reference over pasting
a new essay into the Skill. Skills stay thin; this layer stays detailed.

Candidate shape:

```text
baseline file + one proposed hunk
stated behavior change (one sentence)
failure case it should fix
protection case it must not disturb
```

Do not bundle a Skill rewrite with a new state file, a Verdict synonym,
or a helper script. Instruction-only remains the default.

---

## C. Cases

Keep three kinds under `docs/validation/` — **never** under `.research/`:

```text
docs/validation/research-intelligence/cases/
docs/validation/research-intelligence/prompt-regression/
```

```text
failure cases     — the repeated miss the change aims to fix
protection cases  — behavior that must not get heavier or more gated
held-out cases    — not used while tuning the candidate
```

A case is a **fixture**: input files or a prompt, the behavior to observe,
and what must / must not be written. It is not a scientific EXP and not
a Discovery.

**Held-out** cases stay sealed until the candidate is frozen. Using them
to retune is leaking the test into the prompt.

The canonical **protection** pattern is: ordinary exploratory EXP must
stay light; do not fire every intelligence gate on a sanity rerun.
Fixtures for that pattern belong under
`docs/validation/research-intelligence/cases/` (none numbered yet).

Examples of the three kinds (patterns, not a quota):

```text
failure     — agent treats a weak-baseline win as mechanism evidence
protection  — cheap exploratory EXP does not load idea-evaluation
held-out    — a third fixture in another domain (systems vs security)
```

Project `.research/` may hold the *science* that a fixture talks about
only if that project is itself the framework example. Framework
evolution logs still go to `docs/validation/`.

---

## D. Baseline vs candidate

Compare old vs new under the **same**:

```text
model
context
tools
input fixture
```

Write both behaviors down: what was read, what was written, what was
*not* written, which gates fired. Impressionistic “feels smarter” is
not a comparison.

Fairness rules:

```text
same model family and size when possible
same available tools (no extra browser / extra subagent for the candidate)
same fixture bytes
same stop condition (do not give the candidate an extra turn budget)
```

If the candidate “wins” only because it was allowed to load three more
references, that is not an instruction improvement — it is a cost
increase. Record cost (what was loaded) as part of the comparison.

The comparison write-up lives in `docs/validation/`, next to the cases.
It is not an Experiment record and must not use Outcome values.

---

## E. Regression rule

Ship a candidate to *review* only if:

```text
improve the target failure
AND
not regress protection cases
```

Both conjuncts are required. A candidate that fixes the failure by
making every path heavier fails even if the failure case now passes.

Example: if the candidate fixes “weak baseline treated as mechanism”
by forcing `idea-evaluation` on every EXP, it fails that protection
pattern (ordinary exploratory EXP stays light). **Reject.**

Held-out is scored **after** the candidate is frozen. A fail on held-out
is a reject, not a hint to edit the prompt again in the same round
(that would be tuning on the test). Start a **new** atomic change set
if you still believe a different one-behavior patch would help.

If the candidate does not stably improve: **reject the change**, keep
the baseline Skill or prompt, and record the honest limitation in
`docs/validation/`. A rejected candidate is evidence about the
framework, not a Negative Discovery in a research project.

---

## F. No automatic deployment

A passing comparison means:

```text
candidate deserves review
```

It does **not** mean overwrite the canonical Skill. A human or an
independent framework Reviewer accepts or rejects the patch. Passing is
a **review request**, not a merge.

Reviewers of evolution patches check:

```text
atomic?          one file, one behavior
triggered?       §A actually held
fair compare?    §D same model / context / tools / input
regression?      failure improved, protection intact
no Protocol fork? Outcome / Verdict / Story shape untouched
no science mix?  nothing written into project .research/ as maintenance
```

Prefer merging a small hunk plus a link to Layer 2 over merging a new
Skill. V0.1.1 keeps 10 scientific/workflow Skills; V0.2's two later
Skills are `idea-evaluation` and `evidence-verification`. This Layer-2
file is not a Skill and does not authorize adding any other.

Wave H may dogfood this method on one prompt (for example
`idea-evaluation`). That dogfood still ends at “deserves review,” not
auto-deploy.

---

## Anti-patterns

- New Skill because an upstream Skill looked thorough.
- Second Outcome table or Verdict vocabulary “for convenience.”
- Putting evolution logs into DISCOVERY, STATE, or LITERATURE.
- Changing Reviewer Verdict words to encode idea-gate actions.
- “While we are here” refactors of frozen core.
- Fixing a failure by enabling every gate (protection regression).
- Tuning on held-out cases in the same round.
- Auto-merging because the comparison “passed.”
- Treating this file as a step inside `research-loop`.
- Storing fixtures under `.research/` so a science project carries
  framework maintenance history.

---

## Using this file

Load when diagnosing repeated *agent* friction or proposing one
instruction patch. Do not load on scientific Open Gaps.

Work product:

```text
trigger evidence
atomic candidate (diff)
failure / protection / held-out pointers under docs/validation/
baseline vs candidate write-up
recommendation: deserves review | reject
```

Main / framework Reviewer decides whether the canonical Skill or prompt
changes. This file never authorizes writing `.research/` science and
never redefines Outcome or Verdict.
