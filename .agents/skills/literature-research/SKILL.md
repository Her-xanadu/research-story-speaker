---
name: literature-research
description: >-
  Search external literature around the current Story gap via web, Zotero, PDF,
  or institutional access. Synthesize what is known, conflicts, supports, suggests,
  and novelty risk; update LITERATURE.md with only valuable entries. Use when
  Open Gaps need prior work, novelty checks, method conflicts, or baseline
  selection. Triggers include 查文献, literature search, prior work on. Do not
  update STORY directly (story-maintenance) or run experiments.
---

# Literature Research

Thin Skill for turning external knowledge into durable `.research/LITERATURE.md`
entries. Entry format: [state-files.md](../../references/state-files.md) §
LITERATURE.md. Gap priority and Literature routing:
[story-loop.md](../../references/story-loop.md).

Heavy parallel reading may delegate to `literature-scout` subagent; integrator
writes canonical LITERATURE entries.

## When to use

- An Open Gap is blocked by unknown prior work or novelty risk.
- Story cites a method conflict (e.g. transductive vs inductive calibration).
- Baseline or comparison selection needs literature grounding.
- `research-loop` routed to Literature for the current gap.
- `experiment-design` needs baseline papers before drafting Comparisons.

Do **not** use for: running code (`experiment-execution`), first-pass result
interpretation (`result-analysis`), Story edits (`story-maintenance`), or
compressing state files (`research-memory`).

## Goal

For the **focal Story gap**, produce a structured synthesis answering:

| Lens | Question |
| --- | --- |
| **Known** | What does the field already establish? |
| **Conflicts** | Where do papers disagree or contradict our Story? |
| **Supports** | What evidence backs (or partially backs) our direction? |
| **Suggests** | What methods, baselines, or experiments do papers imply? |
| **Novelty** | What appears already done; what gap remains for us? |

Persist only **valuable** findings in `LITERATURE.md` — one section per
important paper, not search logs or abstract dumps.

## Default flow

1. **Anchor gap** — Read `.research/STORY.md` (Open Gaps, Boundary) and
   `.research/STATE.md` focus. One focal gap per session unless parallel scouts.
2. **Scan existing** — Read `.research/LITERATURE.md` to avoid duplicate entries;
   note Relation fields already covering the gap.
3. **Search** — Use web / arXiv / Semantic Scholar / Zotero / PDF per
   `.research/RESOURCES.md` External Capabilities. Prefer primary papers,
   surveys, and benchmark papers for baselines. Zotero and PDF are acquisition
   aids; canonical record stays in LITERATURE.md.
4. **Synthesize** — Draft the five-lens summary (Known / Conflicts / Supports /
   Suggests / Novelty) for the session; use it to decide what merits a permanent
   entry.
5. **Write LITERATURE.md** — For each important source, append or update one
   section: Reference, Research Problem, Core Method, Important Finding,
   Relation to Our Story, Relation to Experiments, Possible Inspiration, Source
   (template: `.agents/templates/LITERATURE.template.md`). Do **not** paste into
   STORY or DISCOVERY unless explicitly requested.
6. **Update STATE** — Brief next step if literature changes experiment priority
   or closes a novelty blocker.
7. **Hand off** — If empirical test is now obvious → `experiment-design`; if
   Story wording only → `story-maintenance`; else `research-loop`.

## Reads

| Priority | Files |
| --- | --- |
| Required | `.research/STORY.md`, `.research/LITERATURE.md` |
| Often | `.research/STATE.md`, `.research/PROJECT.md`, `.research/DISCOVERY.md`, `.research/RESOURCES.md` |
| Reference | [state-files.md](../../references/state-files.md), [story-loop.md](../../references/story-loop.md) |
| Subagent | `.agents/subagents/literature-scout.md`, `.agents/prompts/subagent-handoff.md` |

## Updates

| File | What to update |
| --- | --- |
| `.research/LITERATURE.md` | New or revised paper sections (valuable info only) |
| `.research/STATE.md` | Next action if routing changed (brief) |

Do **not** update `EXPERIMENTS.md`, `DISCOVERY.md`, or `STORY.md` in this Skill.

Anti-duplication: no literature dumps in STORY — [state-files.md](../../references/state-files.md).

## Deviation allowed

- Literature-only session with no STATE change when gap and routing unchanged.
- Delegate bulk search to `literature-scout`; integrator writes LITERATURE entries
  from scout output in `.research/work/<task-slug>.md`.
- Skip low-relevance papers — note search scope in STATE if gap remains open.
- Compare two papers in one LITERATURE entry when they jointly address one gap.
- Stop after synthesis memo when user asked for a report only (no LITERATURE write).
- Verify methods sections before treating abstract claims as findings.

Boundaries:

- Do not treat abstracts as verified findings without checking methods.
- Do not redo full literature review when LITERATURE already covers the gap —
  update Relation fields or add one targeted paper instead.
