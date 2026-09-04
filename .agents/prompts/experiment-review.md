# Experiment Review Prompt

Main Agent: when to review and how to dispatch — see the
[experiment-review](../skills/experiment-review/SKILL.md) Skill. This file only
provides the decision matrix and the `REVIEWS.md` synthesis template.
Independence, file naming, and Verdict: [reviewer.md](../subagents/reviewer.md).
Use reviewer.md §Verdict — do not recopy the Verdict list.

## Task fields

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence>
Review round: N
Relevant files:
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - .research/DISCOVERY.md → <relevant sections>
  - .research/RESOURCES.md
  - <code / config / result paths>
  - .research/work/<executor>.md
  - .research/work/<analyst>.md (if any)
  - .research/work/<EXP-ID>/evidence-verification.md (if prepended; not required)
Required output:
  - .research/reviews/<EXP-ID>/method-review-r<N>.md (if needed)
  - .research/reviews/<EXP-ID>/result-review-r<N>.md (if needed)
  - Main Agent later: summary in .research/REVIEWS.md
```

Subagents **read** these paths from disk; do not paste contents into dispatch messages.

## Review decision matrix

| Situation | Dispatch |
|-----------|----------|
| Design untested, no results yet | method-review only |
| Results exist, method never checked | method-review then result-review |
| Trusted method, questionable interpretation | result-review only |
| Both design and results contested | both, separate fresh contexts if possible |
| High-stakes results (optional prepend) | evidence-verification work file, then result-review |

Companion prompts: [method-review.md](method-review.md),
[result-review.md](result-review.md). High-stakes Evidence Gate:
[evidence-verification](../skills/evidence-verification/SKILL.md) Skill
(operators in [evidence-and-claim.md](../references/research-intelligence/evidence-and-claim.md)).

## High-stakes evidence check (optional prepend)

For high-stakes **result** reviews — Story Evidence candidate, main baseline
comparison, surprising strong result, mechanism claim, Story-core change, or
READY_FOR_WRITING — Main Agent **may** run `evidence-verification` first and
list `.research/work/EXP-xxx/evidence-verification.md` under Relevant files.

This is **not** mandatory. Ordinary exploratory or sanity results skip it.
Do not block every review on that Skill.

The Reviewer still reads **direct artifacts** (code at commit, configs, logs,
metrics, plots). The verification work file is a hypothesis to check, not a
substitute for those artifacts, and not a Reviewer Verdict.

## Synthesis template (Main Agent → REVIEWS.md)

```text
### <EXP-ID> — <short title>
**Latest method review:** r<N> — <per reviewer.md>
**Latest result review:** r<N> — <per reviewer.md>
Files:
- method-review-r1.md
- result-review-r1.md
```

`REVIEWS.md` is the review **index + current summary**. It is **not** a
Reviewer artifact and does not count as an additional review.
`Provenance: raw | synthesis` describes review artifacts under
`.research/reviews/` only ([reviewer.md](../subagents/reviewer.md)). Keep
Latest method review, Latest result review, and pointers to source review
files. Subagents do **not** write `REVIEWS.md` — Main Agent writes the
index/summary.

## Next-action sync (Main)

If the Reviewer Verdict is `REVISE`, `REJECT`, or `ATTENTION_REQUIRED` (per
reviewer.md §Verdict; do not recopy the list) **and** that changes the next
action, Main must sync both existing fields:

```text
EXPERIMENTS.md → Next
STATE.md → Recommended Next Action
```

Do not leave STATE saying revise while EXPERIMENTS still says execute. No
new fields.

## Quality bar

- One EXP-ID per review folder; do not mix experiments.
- If reviews disagree with analyst, document why in REVIEWS summary.
- Review does not replace updating EXPERIMENTS / DISCOVERY — it informs those updates.
- Do not polish `raw` review bodies when writing the index/summary.
