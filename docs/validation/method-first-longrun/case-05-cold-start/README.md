# Case 5 — 600+ EXP Cold Start

**Wave:** Method-First Wave B
**Kind:** Scientific working set; do not load the ledger

---

## Input

Project has a huge `EXPERIMENTS.md` (600+ sections). STATE:

```markdown
## Workflow Position
`W2 TEST`
## Current Focus
判断 candidate ambiguity 是否具有独立于 group balancing 的检测收益。
## Active Experiment
EXP-704
## Key Files
| 用途 | 路径 |
| Results | results/EXP-704/latest.json |
```

---

## Expected reads

```text
PROJECT
STORY
STATE
EXP-704 section
linked artifact (results/EXP-704/latest.json)
```

Must not:

```text
cat EXPERIMENTS.md
scan .research/work/
read entire REVIEWS
read all historical DISCOVERY sections
```

(`workspace-resume` §Load scientific working set;
`state-files.md` §Scientific Working Set)

---

## Observable asserts

- [ ] Resume packet answers Position, Focus, Active EXP, Next
- [ ] Only the pointed EXP section was opened, not the 600-row body
- [ ] No `research-memory` call just because EXPERIMENTS is large
- [ ] Inner loop continues; no W1 reframe
