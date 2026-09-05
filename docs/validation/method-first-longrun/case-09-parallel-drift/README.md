# Case 9 — Parallel Drift

**Wave:** Method-First Wave E
**Kind:** Main contracts to method-critical tasks

---

## Input

Parallel tasks became:

```text
dataset audit
new method B
new method C
framework optimization
code review
```

They share **no** common focal question. Decision This Task Can Change
cannot be answered for several of them.

---

## Expected behavior

```text
Main 收缩
只保留当前 method-critical tasks
```

Do not dispatch subagents that cannot name the Decision, unless the
user explicitly asked (`subagent-handoff.md`). Support tasks may remain
only if they block the current science EXP (Case 1 resume contract).

research-lead must not rank the audit/review backlog above the live
method question.

---

## Observable asserts

- [ ] Unrelated routes B/C and framework optimization are dropped
- [ ] Remaining work serves the current method hypothesis
- [ ] No dispatch without Decision This Task Can Change
- [ ] Still one Workflow Position
