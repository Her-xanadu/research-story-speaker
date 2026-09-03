# Method Review Prompt

Hand this prompt to the `reviewer` subagent (or any fresh-context reviewer with no designer history).
Independence header, Verdict, and Required output headings:
[reviewer.md](../subagents/reviewer.md).
**Model relation** is relative to the **method designer**.
Write `Verdict: <per reviewer.md>` — do not recopy the Verdict list.

## Task fields

Main Agent fills in before dispatch. **Do not paste full state files** — the reviewer reads from disk.

```text
EXP-ID: <e.g. EXP-031>
Story gap: <one sentence — which Open Gap or Boundary item>
Review round: N
Relevant files:
  - .research/STORY.md
  - .research/EXPERIMENTS.md → section <EXP-ID>
  - .research/RESOURCES.md → <codebase entry>
  - <repo path> @ <commit hash>
  - <config / script paths>
  - .research/work/<design-or-executor-report>.md (if any)
Required output: .research/reviews/<EXP-ID>/method-review-r<N>.md
```

## Optional Layer 2 (this task only)

Load when the design claims a mechanism or spends serious compute. Skip for a
one-line sanity rerun whose Question is already on disk. **Do not** load the
rest of `research-intelligence/` for this review.

- [idea-and-mechanism.md](../references/research-intelligence/idea-and-mechanism.md)
  — mechanism identity, deletion test, complexity budget, minimum decisive test
- [experiment-thinking.md](../references/research-intelligence/experiment-thinking.md)
  — question-first, unit of analysis, confounders, controls, must/nice/cut

## Instructions for reviewer

1. **Read** all listed workspace files and code artifacts from disk.
2. Determine whether the **experimental method** can answer the Story gap — not whether results look good.
3. Inspect **direct evidence**: design doc, code diff at commit, configs, data pipeline, baseline definitions.
4. Cross-check executor claims against implementation; note any design–implementation drift.
5. **Write** the full review to `Required output` path, starting with the header in reviewer.md.
6. **Do not** edit `STORY.md`, `EXPERIMENTS.md`, `REVIEWS.md`, or other canonical state files.
7. **Do not** polish this `raw` body after writing it.

## Attribution gate

Ask, even before results exist:

> If the result is positive, can this design attribute the gain to the claimed mechanism?

If **no** — missing isolating control, unfair baseline, information or compute
asymmetry, leaked unit, or a costume rather than an identity — the method
Verdict is `REVISE` even before results. Use reviewer.md §Verdict — do not
recopy the list. That token is the Reviewer contract, not idea-and-mechanism.md
§H. Do not defer a non-attributing design to result review.

## Judgment lenses (internal)

Apply the lenses that can actually threaten **this** design. Do not score every
row. Cite artifacts (commit, config, split, comparison), not adjectives.

- **Scientific question alignment** — Does the method answer the stated Story gap / EXP Question, or a nearby easier question? (`experiment-thinking.md` §A)
- **Mechanism identity** — Does the change alter information flow or the decision rule, or only a costume (rename, frozen weight, extra knob, stacked unchanged parts)? (`idea-and-mechanism.md` §C, §G)
- **Falsifiability** — Is there an observable pattern that would *lower* belief in the claimed mechanism? If every outcome can be narrated as support, the design cannot change belief.
- **Control sufficiency** — Is there a control that splits the claimed mechanism from the best rival (mechanism-off, sham, information-matched, budget-matched, …)? Not every EXP needs every control. (`experiment-thinking.md` §D)
- **Baseline fairness** — Is the default honest (tuned, informed, capacity-appropriate), or a straw man?
- **Information symmetry** — Did the method see features or side channels the baseline was denied?
- **Compute fairness** — Same steps, data volume, tuning budget, or wall-clock envelope when those are plausible rivals?
- **Unit of analysis** — Name sample / experimental / analysis / grouping units. Inflated *n* from a leaked grouping unit is not more evidence. (`experiment-thinking.md` §B)
- **Leakage** — Grouping unit (site, capture, subject, session, day) across train / test / folds, or test-split statistics used as features.
- **Confounders** — Split, seed, init, budget, capacity, augmentation, optimizer, preprocessing, extra information, tuning opportunity — equalize the best rival, not the whole list. (`experiment-thinking.md` §C)
- **Replication structure** — Can another agent rerun from commit + config? Is one notebook being sold as a confirmatory design?
- **Minimum decisive design** — Is this the smallest comparison that could change judgment on the gap, or a decorative grid? (`idea-and-mechanism.md` §H; `experiment-thinking.md` §F–H)
- **Unnecessary complexity** — Components that do not serve the Question; extras that would survive a deletion test. (`idea-and-mechanism.md` §F)
- **Alternative explanation** — If the method “succeeds,” could it still be measuring the rival (shared scaffold, extra info, extra compute, leakage)?
- **Reproducibility / cost** — Can another agent rerun? Is cost proportionate to information gained?

## Required review body

Header, then `## Verdict` (`Use reviewer.md §Verdict`), then the five sections
in [reviewer.md](../subagents/reviewer.md), filled from the **method** perspective:

- strongest evidence — what makes the method credible for this gap
- main weakness — most serious methodological flaw or gap
- alternative explanation — if method succeeds, could it measure the wrong thing?
- story impact — how method quality affects trust in Evidence / Boundary / Open Gaps
- recommended next move — fix design, add control, rerun, or proceed to result review

## Return to caller

After writing the file, return Verdict plus the same five sections (concise summary acceptable).

## Escalation

Recommend a separate result review once method is adequate and raw results exist.
If method is fatally flawed, say so explicitly — do not defer to results.
