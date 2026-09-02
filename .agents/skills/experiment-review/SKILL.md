---
name: experiment-review
description: >-
  Create independent Reviewer work for a specific EXP-ID: method-review and
  result-review. Reads direct evidence, writes to .research/reviews/EXP-xxx/
  and REVIEWS.md. Use for high-stakes experiments, anomalous results, Story
  core changes, or before closing a research line. Do not use for executing or
  interpreting experiments as the same agent that produced them.
---

# Experiment Review

Thin Skill for **independent critique** of one `EXP-xxx` at a time.
Traceability chain: [git-linking.md](../../references/git-linking.md).
Review index: [state-files.md](../../references/state-files.md) (REVIEWS section).

## When to use

- High-cost experiment **before** final commitment (method review).
- New core method, anomalous results, or closing an important research line.
- Story core mechanism (Problem / Key Observation / Core Idea) may change.
- Preparing to mark Story complete (requires independent approval per `PROJECT.md`).
- `result-analysis` or `research-loop` flagged contested or high-stakes evidence.
- Fresh-context, different-model, or `reviewer` subagent critique is available.

Do **not** use for: running code (`experiment-execution`), primary interpretation by
the executor (`result-analysis` first), or framework hygiene (`framework-maintenance`).

## Goal

Produce **independent** method and/or result reviews tied to one `EXP-xxx`, with
traceable evidence and actionable critique — not merely approve/reject.

## Default flow

1. **Select EXP-ID** — One experiment per review cycle. Load section in
   `.research/EXPERIMENTS.md`, linked Git commit, Results, and relevant
   `STORY.md` / `DISCOVERY.md` context.
2. **Choose review type(s)**:
   - **method-review** — design, controls, comparability, reproducibility, Story
     alignment (before or after runs).
   - **result-review** — whether results support claims, reliability, alternatives,
     Story impact (after artifacts exist).
3. **Read direct evidence** — Code at recorded commit, configs, logs, plots, metrics.
   Navigate RESOURCES → repo → Entry per [git-linking.md](../../references/git-linking.md).
   Do not rely only on executor or analyst narrative.
4. **Invoke Reviewer** — Prefer: different model, different model family, fresh context,
   or `reviewer` subagent. Use prompts:
   - [method-review.md](../../prompts/method-review.md)
   - [result-review.md](../../prompts/result-review.md)
   Cite EXP-ID, Story gap, file paths — do not paste full state files.
5. **Write full reviews** — Save under:
   ```text
   .research/reviews/EXP-xxx/method-review.md
   .research/reviews/EXP-xxx/result-review.md
   ```
6. **Write summary** — Add entry to `.research/REVIEWS.md` (verdict summary, key
   weakness, Story impact pointer). Template: [REVIEWS.template.md](../../templates/REVIEWS.template.md).
7. **Link back** — Set Review field in `EXPERIMENTS.md` for that `EXP-xxx` to point
   at review files and REVIEWS summary.

## Reviewer output must include

At minimum address (see prompt templates):

- Strongest evidence
- Main weakness
- Alternative explanation
- Story impact
- Recommended next move

## Reads

| Priority | Files |
|----------|-------|
| Required | `.research/EXPERIMENTS.md` (`EXP-xxx`), raw results, code at Git commit |
| Often | `.research/STORY.md`, `.research/DISCOVERY.md`, `.research/REVIEWS.md` |
| Prompts | [method-review.md](../../prompts/method-review.md), [result-review.md](../../prompts/result-review.md), [experiment-review.md](../../prompts/experiment-review.md) |
| Subagent | [reviewer.md](../../subagents/reviewer.md) |
| Reference | [experiment-record.md](../../references/experiment-record.md), [git-linking.md](../../references/git-linking.md), [state-files.md](../../references/state-files.md) |

## Updates

| File | What to update |
|------|----------------|
| `.research/reviews/EXP-xxx/*.md` | Full review content |
| `.research/REVIEWS.md` | Summary index per EXP-ID |
| `.research/EXPERIMENTS.md` | Review cross-links only |

Do not rewrite Interpretation or Story here — recommend changes; owner skills apply them.

## Traceability

Any agent seeing `EXP-xxx` should follow:

```text
EXPERIMENTS.md → EXP-xxx
  → .research/reviews/EXP-xxx/method-review.md
  → .research/reviews/EXP-xxx/result-review.md
  → REVIEWS.md → EXP-xxx summary
  → Discovery / Story Impact (via result-analysis / story-maintenance)
```

Gate: stranger Agent rebuilds chain without chat history ([git-linking.md](../../references/git-linking.md)).

## Deviation allowed

- Method-only or result-only when the other is unnecessary — note omission in `REVIEWS.md`.
- Lighter depth for low-cost exploratory EXP — state reduced scope in review header.
- External human or MCP Reviewer — still mirror summary into `REVIEWS.md`.
- Not every experiment needs review; skip when cost and stakes are trivial.
- Pre-run method review without result-review until runs exist — link planned result-review.

## Boundaries

- Reviewer should be as independent as practical from the executing agent.
- Do not paste entire state files into prompts — point to paths.
- When both review types were required, both files must exist or omission documented.
- This skill does not update DISCOVERY or STORY directly.
