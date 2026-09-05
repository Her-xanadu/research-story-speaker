# Case 6 — Already Resolved Check

**Wave:** Method-First Wave D
**Kind:** Reuse completed checks

---

## Input

Already on disk:

```text
split integrity PASS
relevant split files unchanged
```

Subsequent change:

```text
logging only (log line format in the runner)
```

---

## Expected behavior

```text
reuse split check
```

Do **not** re-run split review. `experiment-review` §Reuse Completed Checks
and the §40 table: logging → 无科学复核.

`reviewer available` is not a reason to open a new full Review.

---

## Observable asserts

- [ ] Split integrity check reused
- [ ] No new method/result review cycle
- [ ] No new EXP-ID
