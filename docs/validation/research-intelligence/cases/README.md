# Research Intelligence Regression Cases (Wave F)

Read-only fixtures for V0.2 Gate D / later harness checks.

- Live under this directory only. **Do not** copy them into the framework
  repo `.research/` (that tree stays **UNINITIALIZED**).
- Each case folder is self-contained. Input is **embedded MOCK markdown**
  in that case’s `README.md`, not a checkout of live state.
- Numeric / log facts for later Gate D live in `artifacts/` when the case
  needs them. Do not invent replacement numbers.
- Anonymized: no host home paths, no live project dumps.

| ID | Folder | Title | Primary gate |
|----|--------|-------|----------------|
| 01 | [01-cosmetic-successor/](01-cosmetic-successor/) | Cosmetic Successor | `idea-evaluation` |
| 02 | [02-real-mechanism-successor/](02-real-mechanism-successor/) | Real Mechanism Successor | `idea-evaluation` |
| 03 | [03-weak-baseline-positive/](03-weak-baseline-positive/) | Weak Baseline Positive Result | `result-analysis` + `evidence-verification` |
| 04 | [04-technical-failure/](04-technical-failure/) | Technical Failure | `result-analysis` / `failure-diagnosis` |
| 05 | [05-scientific-negative/](05-scientific-negative/) | Scientific Negative | `result-analysis` + `story-maintenance` |
| 06 | [06-null-result/](06-null-result/) | Null Result | `result-analysis` / `result-diagnosis` |
| 07 | [07-invalid-previous-evidence/](07-invalid-previous-evidence/) | Invalid Previous Evidence | `result-analysis` + `story-maintenance` |
| 08 | [08-pseudoreplication/](08-pseudoreplication/) | Pseudoreplication | `experiment-design` + `experiment-thinking` §B |
| 09 | [09-novelty-threat/](09-novelty-threat/) | Novelty Threat | `idea-evaluation` + deep literature |
| 10 | [10-ordinary-exploratory/](10-ordinary-exploratory/) | Ordinary Exploratory (**protection**) | `experiment-design` → `experiment-execution` → `result-analysis` |

Cases 01–05 are sibling folders in this same directory (MOCK-WBS domain).
Cases 06–10 are independent MOCK TraceSet-Omega / PulseGate fixtures; do not
merge the two Story universes.

**Kinds** (`skill-evolution.md` §C) for 06–10: 06–09 are **failure** patterns
(the miss to catch). **10 is the canonical protection case** — a candidate
that “fixes” other misses by firing every intelligence gate on a sanity EXP
must be rejected (`skill-evolution.md` §E).

Cite Protocol owners (`experiment-record.md` §Outcome 值, `reviewer.md`
§Verdict, `state-files.md` Story six). Do **not** copy Outcome or Verdict
tables into fixtures or work files. Idea-gate actions live only in
`idea-and-mechanism.md` §H.
