# Extension Brief — knowledge-gap literature

## Request

**User need:** Coordinate the experiment inner loop with the literature
vault so that, after an isolating miss still fails, the next method
change is grounded in closest-work / Suggests — not an invented module.
Keep looping. Find innovation from the knowledge base, not from guessing.

**Target users/projects:** Every research-story-speaker workspace that
runs a Method-First inner loop against a local paper vault.

**Why existing behavior is insufficient:** After the no-yield retune,
isolating miss → simplify / delete / change → stay W2. Literature is
blocked on the same-mechanism inner loop. The agent can always name a
component to delete or a module to add and never consult `LITERATURE.md`.
The existing hook (“文献属于 W1，或 W4 判定知识缺口后回 W1”) was unused.

## Classification

**Primary type:** prompt/reference enrichment (routing operator)

**Deployment scope:** framework core (instruction-only; every workspace)

**Extension-ladder rung:** 4 (existing Skills) + 3 (story-loop / idea-and-mechanism)

**Decision:**
`EXTEND_EXISTING`

**Why lower rungs are insufficient:**

- Rung 0: hundreds of local tweaks with no vault consult is a real miss.
- Rung 1–2: not project configuration; not composing a 16th Skill.
- Rung 3 alone: compact `result-analysis` must not open Layer 2, so the
  W1 hand-off has to live above the compact stop line.
- Rung 7: a 16th Skill (`literature-for-successor`, etc.) is not earned.
  `literature-research` already owns Suggests / Novelty / closest-work.

## Existing Owner and Overlap

**Closest existing owner:** `.agents/references/story-loop.md` (W1 文献 /
W4 知识缺口；Method Complexity Rule). Compact disclosure:
`result-analysis`, `experiment-design`. Route owner: `research-loop`.
Consult owner: `literature-research`. Successor identity:
`idea-and-mechanism.md` §E / §H.

**Overlap found (reuse, do not duplicate):**

- Literature is not a W stage (`story-loop.md`)
- Light default hands off to `experiment-design` when sources name a test
- Five lenses already include Suggests / Novelty
- Method Complexity Rule already forbids default add-component
- Closest-work same-axis → REVISE / PARK (`idea-and-mechanism.md` §E)
- Deep literature is already optional expensive

**What will not be duplicated or imported:**

- a 16th Skill, a second orchestrator, a Protocol enum, a STATE field
- literature on every EXP, first miss, engineering failure, or `running` wait
- compact opening `LITERATURE.md` / `experiment-thinking.md`
- Obsidian / `paper-find` toolchain changes

## Capability Contract

**Trigger:** Isolating miss still fails, and the next change would be an
**invented** new module / mechanism / loss without unused Suggests /
closest-work in `LITERATURE.md` for **this failure condition**.

**Do not trigger:** first valid negative (isolation first); named
`Full - A` / simpler explanation / new isolation (stay W2); engineering /
invalid / not-assessed; unused Suggests already on disk (use **one**,
**0** find); `monitor-experiment` wait; every EXP.

**Default owner:** `research-loop` at narrow `W1 FRAME` → light
`literature-research` (local `paper-consult` first) → **one** successor
or closest contrast → `experiment-design` back to `W2 TEST`.

**Inputs:** failure condition, current method, `STORY.md` gap,
`LITERATURE.md` if already on disk.

**Outputs:** Position `W1 FRAME` then back to `W2 TEST`; one successor
named from Suggests / closest-work. Closest-work same axis → REVISE /
PARK, not a cosmetic-difference EXP.

**Read scope:** `research-loop` may read `LITERATURE.md`. Compact
`result-analysis` / `experiment-design` must **not**.

**Write scope:** existing canonical files only. No new STATE field.

**External dependencies:** existing `paper-consult` / optional `paper-find`.

**Fallback/degraded:** no vault → `paper-find` candidates only; do not
treat abstract-only as methods-checked.

**Uninstall/rollback:** revert the instruction edits on this branch.

## Architecture Fit

- [x] Story-driven loop preserved (no new W stage)
- [x] Eight canonical files preserved
- [x] Main canonical writer preserved
- [x] Single `research-loop` preserved
- [x] Prompt/file/config-first preserved
- [x] Canonical-first / adapters-thin preserved
- [x] Progressive disclosure preserved (compact does not open Layer 2)
- [x] No second truth or state source (`Research Round` still forbidden)

## Minimal File Plan

**Files added:** this brief + `VALIDATION.md`

**Files modified:** `story-loop.md`, `research-loop`, compact
`result-analysis` / `experiment-design`, `literature-research`,
`idea-and-mechanism.md`, `experiment-record.md`, `workspace-resume`,
`next-research-move.md`, `AGENTS.md`

**Files intentionally untouched:** Obsidian toolchain, compact always
reading `LITERATURE.md`, new STATE fields, `deep-literature-mode.md`
body, live `.research/` science, PR #3 orchestration branch

**Rules linked rather than copied:** Method Complexity verbs; Idea-gate
ADVANCE / REVISE / PARK / ABANDON; five literature lenses

## Protection Cases

- First valid negative → new isolation, still W2, no literature
- Named deletion stays W2
- Compact does not open `LITERATURE.md` or Layer 2
- No literature during `sleep` / `running`
- Unused Suggests → use one, do not re-search
- Knowledge-gap W1 is **not** 「最大 gap」 and not a Goal change
- Deep literature remains optional expensive
- Skills stay 15

## Validation Plan

Static greps: frozen counts; operator present; compact stop-line does
not open `LITERATURE.md`; first-miss / running / every-EXP protections;
no `Research Round`. No live Gate.

## Rollback

Revert this commit family. No schema migration.

## Notes

Complements `docs/validation/extensions/no-progress-inner-loop/`:
that operator forbids `keep` + same contrast; this one forbids
guessing a new module when the vault was never asked.
