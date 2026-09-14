# Validation — knowledge-gap literature

Instruction-only. No 16th Skill. No `Research Round` field. No live
scientific Gate.

## Frozen counts (must hold)

| Item | Expected | Result |
|------|----------|--------|
| Canonical `.research/*.md` | 8 | PASS (8) |
| `research-loop` Skill | 1 | PASS |
| Subagents | 5 | PASS (5) |
| Skills | 15 | PASS (15; no 16th) |
| RI files | 6 | PASS (6) |
| Runtime scripts under `.agents` / `adapters` | 0 | PASS |
| Harness skill symlinks `.claude` / `.cursor` | 15 / 15 | PASS |
| Skill YAML frontmatter | all parse | PASS |

## Instruction contracts

### Trigger

Isolating miss still fails **and** the next change would invent a new
module / mechanism / loss without unused Suggests / closest-work for
**this failure condition** → narrow `W1 FRAME` + light
`literature-research` → **one** successor → `W2 TEST`.

### Must not fire

- first valid negative (isolation first)
- named `Full - A` / simpler explanation / new isolation (stay W2)
- engineering / invalid / not-assessed
- unused Suggests already on disk (use one, 0 find)
- `Status=running` / `monitor-experiment` wait
- every inner-loop EXP
- compact opening `LITERATURE.md` or Layer 2
- knowledge-gap W1 re-running「最大 gap」or changing PROJECT Goal

### Compact stop-line

`result-analysis` and `experiment-design` compact both list
`LITERATURE.md` in do-not-open. `experiment-design` still has exactly
five in-session items.

### Closest-work

Same mechanism axis → REVISE / PARK, not a cosmetic-difference EXP.

## Static 2026-09-14

See `/opt/cursor/artifacts/knowledge-gap-literature/validation.log`.

`ALL PASS`. Skills 15. `.research/` untouched. Deep literature remains
the existing optional expensive path.
