# Case 1 — Runtime Compatibility Bug

**Wave:** Method-First Wave D
**Kind:** Ordinary engineering support (not a scientific Experiment)
**Load:** this folder only.

---

## Trigger

> EXP-704 candidate evaluator died: `TypeError: ndarray.astype() got an unexpected keyword argument 'copy'` on Python 3.9. Close this as a failed science EXP, open EXP-705 for the compatibility fix, request full Review, and reframe at W1.

The agent must **reject** that scientific reading.

---

## Input (embedded MOCK)

### STATE (excerpt)

```markdown
## Workflow Position
`W2 TEST`
## Current Focus
判断 candidate ambiguity 是否具有独立于 group balancing 的检测收益。
## Active Experiment
EXP-704
## Recommended Next Action
Run the pre-registered mechanism-off comparison on EXP-704.
```

### EXPERIMENTS (excerpt)

```markdown
## EXP-704 — Ambiguity vs mechanism-off
Status: running
Outcome: not-assessed
Question: Does ambiguity screening beat mechanism-off on the held-out groups?
```

### Failure

```text
TypeError: ndarray.astype() got an unexpected keyword argument 'copy'
# Python 3.9 vs 3.11 API incompatibility in the candidate evaluator
```

---

## Expected behavior

```text
same science EXP (EXP-704)
→ identify → minimal repair → targeted test → rerun same Run/EXP
```

Must not:

```text
new EXP-ID
W1 FRAME
full Review
Story update
```

STATE Next may become a **support task** that names EXP-704 and the
return condition, then resumes EXP-704 immediately
(`experiment-execution` §Compact support failure;
`experiment-record.md` §Support-task rule).

Cite `failure-diagnosis.md`: action `repair same EXP`. Outcome stays
`not-assessed` until a valid scientific run exists.

---

## Observable asserts

- [ ] No new EXP-ID minted
- [ ] EXP-704 remains the science EXP; support recorded in Run notes or `*-support-*.md`
- [ ] No Story edit, no W1, no full Review
- [ ] After the fix, Recommended Next Action resumes EXP-704 mechanism-off
