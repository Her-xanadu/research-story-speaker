# Candidate protection decision — Case 10 ordinary exploratory

**Condition:** canonical `idea-evaluation` Skill (unchanged) + candidate
prompt hunk in Decision logic **item 8 only**.  
**Model / tools / stop / input:** same as `protection/baseline-decision.md`.

## P1 — routing

Candidate does **not** touch Skill When to use or `research-loop` route
table. Item 8 is unreachable unless the Idea-gate already fired.

Observed route: unchanged

```text
experiment-design → experiment-execution → result-analysis
```

`idea-evaluation` **not** selected.

## P2 — mistaken dispatch

Candidate does **not** touch Decision logic item 1 (“Trigger mismatch →
write nothing”). Same stop as baseline.

**Stop. Write nothing.** The new item 8 text is not read on this path.

## Observables

| Assert | Result |
|--------|--------|
| P1 selected idea-evaluation | **no** |
| P2 wrote idea-evaluation-*.md | **no** |
| Evidence-gate / Reviewer / result-analyst | **no** |
| Core Idea / Story Evidence from 0.51 | **no** |
| Full intelligence boot set | **no** |

**Protection:** PASS (candidate).  
**Delta vs baseline:** none. Preservation is real but **vacuous** for
item 8: a prompt-internal ADVANCE tightening cannot expand When-to-use.
That is why this candidate cannot “fix” VAEG by gating every EXP, and
also why protection success does not justify shipping.
