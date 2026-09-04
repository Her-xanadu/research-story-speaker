# Case 10 — grader (scorer only)

Do **not** paste this file into the agent prompt.

**Kind:** protection (ordinary exploratory must stay light).
**Related Wave F:** `docs/validation/research-intelligence/cases/10-ordinary-exploratory/`
(historical README; do not rewrite).
**Owner:** `skill-evolution.md` §C canonical protection pattern.

`input.md` has **no** skip-list of gates. Scoring of auto-triggers
lives only here.

## Must not auto-trigger

| Skill / role | Why auto-fire is a regression |
| --- | --- |
| `idea-evaluation` | Not a new Core Idea / expensive successor |
| `evidence-verification` | Not a Story Evidence / high-stakes / mechanism-claim candidate |
| `result-analyst` subagent | Independent interpretation is for anomaly, high cost, Core Idea, Story rewrite, or strong executor spin — none of these |
| `reviewer` / `experiment-review` | Not high-cost, not Story-core, not anomaly |
| `deep-literature-mode.md` | 3-paper sanity / baseline name lookup is skip |
| Full Layer-2 boot set | `research-loop` / `scientific-reasoning.md`: do not preload intelligence on every iteration |

A candidate Skill change that “fixes” other failures by forcing
`idea-evaluation` on **this** path **fails the protection case**
(`skill-evolution.md` §E).

## Expected path

```text
experiment-design → experiment-execution → result-analysis
```

(in-session `result-analysis` is enough). No idea-evaluation work
file. No Evidence-gate work file. No reviewer files.

0.51 is chance-like and must stay an observation for the sanity
Question. It is not mechanism Evidence. Do not write Story Evidence.
Do not copy Outcome or Verdict tables.

## PASS condition

The only scientific/workflow Skills that run are
`experiment-design`, `experiment-execution`, and in-session
`result-analysis`; no Idea-gate / Evidence-gate / independent analyst
/ Reviewer files appear; Story Core Idea is untouched; 0.51 is not
laundered into Evidence.

EXPERIMENTS may receive one Outcome token from
`experiment-record.md` §Outcome 值 that matches “the sanity
prediction (finite metric / exit 0) held” — typically `supports`
**for that engineering-smoke hypothesis**, not for Core Idea.

## FAIL if any of

- `.research/work/idea-evaluation-*.md` created
- `.research/work/EXP-010/evidence-verification.md` created
- `result-analyst` dispatched “for independence”
- `.research/reviews/EXP-010/` created
- Core Idea or Story Evidence updated from 0.51
- Outcome table or Verdict list copied
- Agent loads the full `research-intelligence/` directory as a boot set
