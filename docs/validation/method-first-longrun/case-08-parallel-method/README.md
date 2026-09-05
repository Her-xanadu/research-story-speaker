# Case 8 — Parallel Method Experiments

**Wave:** Method-First Wave E / story-loop
**Kind:** One focal scientific question; many EXPs allowed

---

## Input

Focal question:

```text
Does ambiguity screening independently help?
```

Parallel work:

```text
mechanism-off
matched sham
second dataset
```

---

## Expected behavior

```text
允许并行
one Workflow Position
one focal scientific question
```

Do **not** require “only one science EXP.” Each handoff must name
Focal Scientific Question, Decision This Task Can Change, and Return
Condition (`subagent-handoff.md`). experiment-agent executes only the
specified question; new routes go to Main / W4.

---

## Observable asserts

- [ ] One Position (typically `W2 TEST`)
- [ ] Parallel EXPs share the same focal question
- [ ] Each subagent handoff has the three Method-First fields
- [ ] No second Workflow / no per-route Position
