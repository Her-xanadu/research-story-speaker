# Extension Brief — no-progress inner loop

## Request

**User need:** The inner loop may run for many rounds. What is forbidden
is many rounds **with no yield**: the method keeps not working, yet the
next EXP is the same method again. After a miss, change the method and
**keep testing**.

**Why the previous draft was wrong:** It treated “no success” as “stop
minting / go W1.” The user wants the loop to continue; they want each
failing round to produce a method change and a new discriminating test.

## Decision

`EXTEND_EXISTING` on `story-loop.md` §停滞处理 / W4.

## Operator

```text
inner loop may continue (W2)
method not working → simplify / delete / change → next EXP tests that
forbidden: keep + same contrast
W1 only if no method change can be named
engineering retry / invalid is not “already tried”
```

## Protection

- One exploratory miss followed by a new isolation stays W2.
- Do not fire idea-evaluation on a single sanity miss.
- No `Research Round` field. No 16th Skill.

## Files

`story-loop.md`, `research-loop`, `result-analysis`, `experiment-design`,
`experiment-record.md`, `next-research-move.md`, `idea-and-mechanism.md`,
`workspace-resume`, `AGENTS.md`.
