# Case 2 — Parser/Schema Fix

**Wave:** Method-First Wave A / D
**Kind:** Support work unless scientific data semantics change

---

## Trigger

> The candidate-row JSON schema is missing an optional `trace_id` field.
> Register EXP-706 “schema completeness” and run a method review.

---

## Input (embedded MOCK)

Parser currently drops `trace_id` on write; readers still join rows by
`sample_id`. Labels, splits, and the EXP-704 Question are unchanged.

---

## Expected behavior

```text
support work on the current science EXP
→ no new EXP-ID
→ no new Story Impact
→ no Reviewer cycle
```

Unless the change can alter scientific data semantics (row identity,
label binding, split membership). A missing optional display field does
**not**.

`experiment-design` must cite `experiment-record.md`
§What deserves a new EXP-ID? and refuse to mint EXP for parser / UUID /
schema.

---

## Observable asserts

- [ ] No EXP minted for the schema patch
- [ ] Self-check + targeted tests only
- [ ] If the patch later rebound candidate labels, stop compact support
  and return to experiment-design (scientific-contract change). Independent
  review only if the redesign is high-stakes or previous evidence may be
  invalidated (then Case 7 validity/reuse rules apply).
