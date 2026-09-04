# V0.2.1 Wave G

Merged evidence on `v0.2.1-micro-hardening` from `v0.2.1-wave-g-codex`, `v0.2.1-wave-g-claude`, and `v0.2.1-wave-g-g3g5`. Base Gate A: `e612f8e`.

| ID | Verdict |
|----|---------|
| G1 | both harnesses **MISS** compact soft target — do not claim success |
| G2 | **PASS** |
| G4 | **PASS** |
| G3 | **ADVANCE PASS** |
| G5 | **reject** (no deploy) |

Codex compact retune lives on `v0.2.1-compact-retune` and is **not** merged here.

## Codex live (`v0.2.1-wave-g-codex`)

| Run | File | Overall |
|-----|------|---------|
| G1 | [g1-case10-codex.md](g1-case10-codex.md) | **MISS** compact soft target |
| G2 | [g2-case01-codex.md](g2-case01-codex.md) | **PASS** |
| G4 | [g4-case03-codex.md](g4-case03-codex.md) | **PASS** |

## Claude Code live (`v0.2.1-wave-g-claude`)

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

## G3 + G5 (`v0.2.1-wave-g-g3g5`)

G3 + G5 only. Sibling Codex/Claude branches own G1/G2/G4.

| ID | File | Harness | Verdict |
|----|------|---------|---------|
| G3 | [g3-case02-codex.md](g3-case02-codex.md) | Codex CLI | **PASS** (`ADVANCE`) |
| G5 | [g5-skill-evolution-independence.md](g5-skill-evolution-independence.md) | Claude Code CLI as scorer | **reject** (no auto-deploy) |

Exact agent/scorer prompts: [prompts/](prompts/). Live G3 work file copy: [artifacts/g3-idea-evaluation-prrw.md](artifacts/g3-idea-evaluation-prrw.md) (docs transcript; not framework `.research/`).
