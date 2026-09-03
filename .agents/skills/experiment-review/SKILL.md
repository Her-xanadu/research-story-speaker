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
Independence, naming, and Verdict: [reviewer.md](../../subagents/reviewer.md).
Use reviewer.md §Verdict — do not recopy the Verdict list.

## When to use

- High-cost experiment **before** final commitment (method review).
- New core method, anomalous results, or closing an important research line.
- Story core mechanism (Problem / Key Observation / Core Idea) may change.
- Preparing to mark Story complete (requires independent approval per `PROJECT.md`).
- `result-analysis` or `research-loop` flagged contested or high-stakes evidence.
- Fresh-context, different-family, or `reviewer` subagent critique is available.

Do **not** use for: running code (`experiment-execution`), primary interpretation by
the executor (`result-analysis` first), or framework hygiene (`framework-maintenance`).

## Goal

Produce **independent** method and/or result reviews tied to one `EXP-xxx`, with
traceable evidence and actionable critique — not a one-word stamp. File names:

```text
.research/reviews/EXP-xxx/method-review-r<N>.md
.research/reviews/EXP-xxx/result-review-r<N>.md
```

Any agent seeing `EXP-xxx` should rebuild the chain in
[git-linking.md](../../references/git-linking.md) §完整追溯链 without chat history.

## Default flow

1. **Select EXP-ID** — One experiment per review cycle. Load section in
   `.research/EXPERIMENTS.md`, linked Git commit, Results, and relevant
   `STORY.md` / `DISCOVERY.md` context.
2. **Choose review type(s)**:
   - **method-review** — design, controls, comparability, reproducibility, Story
     alignment (before or after runs). Model relation vs **method designer**.
   - **result-review** — whether results support claims, reliability, alternatives,
     Story impact (after artifacts exist). Model relation vs **executor / primary analyst**.
     Before result-review, **if high-stakes**: recommend
     [evidence-verification](../evidence-verification/SKILL.md) as an **optional
     prepend** (not every review). The Reviewer still reads **direct artifacts**;
     the work file is not a substitute. See
     [experiment-review.md](../../prompts/experiment-review.md) High-stakes
     evidence check.
3. **Read direct evidence** — Code at recorded commit, configs, logs, plots, metrics.
   Navigate RESOURCES → repo → Entry per [git-linking.md](../../references/git-linking.md).
   Do not rely only on executor or analyst narrative.
4. **Invoke Reviewer** — Independence and header (only definition):
   [reviewer.md](../../subagents/reviewer.md). Use prompts:
   - [method-review.md](../../prompts/method-review.md)
   - [result-review.md](../../prompts/result-review.md)
   Cite EXP-ID, Story gap, Review round `N`, file paths — do not paste full state files.
5. **Write full reviews** — Save under the `r<N>` paths above. Header, `## Verdict`
   (`Verdict: <per reviewer.md>`), then five sections: see reviewer.md.
   Do not polish a `raw` body afterward.
6. **Write summary** — Add entry to `.research/REVIEWS.md`. Template:
   [REVIEWS.template.md](../../templates/REVIEWS.template.md). Decision matrix
   and synthesis template: [experiment-review.md](../../prompts/experiment-review.md).
   A REVIEWS.md entry is Provenance `synthesis`: attach `Source reviews:`; it
   does not add independent Reviewer count.
7. **Link back** — Set Review field in `EXPERIMENTS.md` for that `EXP-xxx` to point
   at `r<N>` files and REVIEWS summary.

## Reads

| Priority | Files |
|----------|-------|
| Required | `.research/EXPERIMENTS.md` (`EXP-xxx`), raw results, code at Git commit |
| Often | `.research/STORY.md`, `.research/DISCOVERY.md`, `.research/REVIEWS.md` |
| Prompts | [method-review.md](../../prompts/method-review.md), [result-review.md](../../prompts/result-review.md), [experiment-review.md](../../prompts/experiment-review.md) |
| Optional prepend | [evidence-verification](../evidence-verification/SKILL.md) — high-stakes result-review only; not every review |
| Subagent | [reviewer.md](../../subagents/reviewer.md) |
| Reference | [experiment-record.md](../../references/experiment-record.md), [git-linking.md](../../references/git-linking.md), [state-files.md](../../references/state-files.md) |

## Updates

| File | What to update |
|------|----------------|
| `.research/reviews/EXP-xxx/*-review-r<N>.md` | Full review content |
| `.research/REVIEWS.md` | Summary index per EXP-ID |
| `.research/EXPERIMENTS.md` | Review cross-links only |

Do not rewrite Interpretation or Story here — recommend changes; owner skills apply them.
Do not paste entire state files into prompts — point to paths.
This skill does not update DISCOVERY or STORY directly.

## Deviation allowed

- Method-only or result-only when the other is unnecessary — note omission in `REVIEWS.md`.
- Lighter depth for low-cost exploratory EXP — state reduced scope in review header.
- External human Reviewer (`Model relation: human`) — still mirror summary into `REVIEWS.md`.
- Not every experiment needs review; skip when cost and stakes are trivial.
- High-stakes result-review may prepend `evidence-verification`; skip that
  prepend on ordinary reviews. Reviewer still reads direct artifacts.
- Pre-run method review without result-review until runs exist — link planned result-review.
- When both review types were required, both `r<N>` files must exist or omission documented.
