# V0.2.1 Wave G — Claude Code live

- **Branch:** `v0.2.1-wave-g-claude`
- **Base:** `origin/v0.2.1-micro-hardening` = `e612f8ee340130d00b01e0d9346db0096db2b6a2`
- **Harness:** Claude Code `/Users/herxanadu/.local/bin/claude` `2.1.220`
- **Model (CLI default):** `deepseek-v4-pro[1m]` (not Anthropic Claude the model)
- **Git:** `/usr/bin/git` `2.50.1 (Apple Git-155)`
- **Scorer:** this report author, using `live-cases/caseNN/grader.md` from the source tree **after** the run. Graders were deleted from each clone before the agent started.
- **Source `.research/`:** still `UNINITIALIZED` (not written).

| Run | Case | File | Compact tokens vs ~54,664 | Grader / judgment | Overall |
|-----|------|------|---------------------------|-------------------|---------|
| G1 | 10 ordinary exploratory | [g1-case10.md](g1-case10.md) | **MISS** (`input_tokens` 50,170; −8.2%) | Protection file-level **PASS** (README scoring-visible) | **MISS** (do not claim compact success) |
| G2 | 01 cosmetic successor | [g2-case01.md](g2-case01.md) | n/a | **PASS** (`REVISE`) | **PASS** |
| G4 | 03 weak baseline | [g4-case03.md](g4-case03.md) | n/a | **PASS** (honest-baseline `does not address`) | **PASS** |

Isolation (all three clones): fresh `/tmp` clone of `e612f8e`; `grader.md`, historical `cases/`, `prompt-regression/`, and remaining `live-cases/` stripped; agent stdin = `input.md`; artifacts at clone-root `artifacts/`. No skip-list of gates and no expected action in the prompt.
