# Case 4 — Full < Matched Sham

**Wave:** Method-First Wave C
**Kind:** Target mechanism contradicted

---

## Input

```text
Full < matched sham
```

The sham sees a matched information budget without the claimed source
mechanism. Full underperforms sham on the registered metric.

---

## Expected behavior

```text
target mechanism contradicted
→ Discovery negative
→ remove mechanism
```

Method Consequence: `delete component` or `abandon mechanism`
(`story-loop.md` §Method Complexity Rule).

Routing:

- If Core Idea is still salvageable (another mechanism remains) → **W2**
- If Core Idea itself failed → **W1**

Do not add a new module because the result was bad.

---

## Observable asserts

- [ ] DISCOVERY Negative cites this EXP
- [ ] Next step removes or abandons the unsupported component
- [ ] W2 if the Story core can still be tested; W1 only if Core Idea died
- [ ] No “add loss / add gate / add expert” default
