# Case 7 — Real Scientific Validity Change

**Wave:** Method-First Wave D
**Kind:** Integrity-sensitive; never reuse

Proves efficiency rules did **not** sacrifice scientific reliability.

---

## Input

Changed:

```text
train/test grouping
```

A prior split-integrity PASS exists for the old grouping. Dependent
EXPs (EXP-701–704) used that split.

---

## Expected behavior

```text
must recheck split validity
related old results may be invalid
```

Integrity-sensitive changes **never reuse**: split, candidate labels,
train/test grouping (`experiment-review` §Reuse Completed Checks /
§40 table). Mark dependent results possibly `invalid` per
`experiment-record.md` §Outcome 值 (cite; do not copy the table).
DISCOVERY: Invalidated Findings, not a casual “still PASS.”

This is validity check + dependent-evidence reconsideration because
previous evidence may be invalidated — not an automatic Reviewer for
every method-component / selector change. Independent review is
required when the redesigned experiment is high-stakes under
`experiment-review` normal scientific-stakes rules, or when previous
evidence may be invalidated.

Delta review may read the grouping diff + affected artifacts; it must
**not** skip the split recheck.

---

## Observable asserts

- [ ] Split validity rechecked
- [ ] Dependent results flagged possibly invalid
- [ ] Prior PASS is not reused
- [ ] No silent “logging-style” skip
