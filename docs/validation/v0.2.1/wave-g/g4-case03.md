# V0.2.1 Wave G G4 — Case 03 weak-baseline evidence gate (Claude Code)

## Identity

| Field | Value |
|-------|--------|
| Harness | Claude Code `/Users/herxanadu/.local/bin/claude` |
| Version | `2.1.220 (Claude Code)` |
| Model | `deepseek-v4-pro[1m]` (CLI default) |
| Context relation | fresh-context. Fresh `/tmp` clone, `--no-session-persistence`, session `f9c8298d-cb13-428d-9dea-19270aba8af0`. Independent of G1/G2. |
| Host memory | n/a for Codex. No `~/.claude/MEMORY.md`. `~/.codex/memories/` fixture-term hits: **0**. Global init skills present, **not invoked**. |
| Clone | `/tmp/rss-v021-g-claude-g4` |
| Clone HEAD | `e612f8ee340130d00b01e0d9346db0096db2b6a2` before and after |
| Agent-visible prompt SHA-256 | `918b17040a5027efdbcdaefbbcecb02cbb4292c65486ebb6325135e6a09e7ba8` (`input.md` = live-cases Case 03 input, stdin) |
| Grader SHA-256 | `cd3d7fdce1159c110ea660cce70350bbef583ed96356a4f9c563b143ef32141d` (scored **after**, source `live-cases/case03/grader.md`) |
| Capture | `--output-format stream-json --verbose`. Trace **present**. Logs: `/tmp/rss-v021-g-claude-logs/g4.stdout.jsonl` |

## Isolation (tester, before agent)

Same strip as G1. Canonical eight left UNINITIALIZED. MOCK only in stdin + clone-root `artifacts/{metrics.json,run.log}`. Prompt has no “typically does not address” / “criterion still fails” scoring sentence.

## Claude file-read trace

- Capture: stream-json + verbose
- Trace present: **yes**
- Opened `grader.md`: **no**
- Opened `live-cases/README.md`: **no**
- Opened historical `cases/*/README.md` or `prompt-regression`: **no**
- Opened repo-root `README.md`: **no**
- Bash `ls` on stripped path `docs/validation/research-intelligence/cases/03-weak-baseline-positive/`: empty (isolation held)

## Files read (Read tool)

`.agents/skills/evidence-verification/SKILL.md`; `.agents/prompts/evidence-verification.md`; `artifacts/metrics.json`; `artifacts/run.log`; `.agents/references/research-intelligence/evidence-and-claim.md`; `scientific-reasoning.md`; `.agents/references/git-linking.md`; `experiment-thinking.md`.

## Files written (agent)

Only `.research/work/EXP-201/evidence-verification.md`. No `RUBRIC.md` / `CLAIMS.md`. No git add / commit. Canonical eight porcelain empty. Outcome / STORY unchanged (still UNINITIALIZED templates).

## Canonical files changed?

**No.**

## Skills / prompts / RI loaded

- Skill: `evidence-verification`
- Prompt: `evidence-verification.md`
- RI: `evidence-and-claim.md`, `scientific-reasoning.md`, `experiment-thinking.md`
- Protocol: `git-linking.md`
- Did not load the full `research-intelligence/` directory. Did not run `result-analysis` as Outcome owner. Did not run `story-maintenance`.

## Tokens

| Meter | Count |
|-------|------:|
| `input_tokens` | 36,925 |
| `cache_read_input_tokens` | 224,384 |
| `output_tokens` | 14,835 |
| `num_turns` | 16 |
| `total_cost_usd` | 0.668 |

## Wall time

- Start: 2026-09-04 14:32:57 +08
- End: 2026-09-04 14:36:26 +08
- Wall: **209 s** (`duration_ms` 207,636)
- Exit code: 0

## Observed

Four per-criterion blocks with Skill headings. Honest-baseline criterion Satisfaction: **`does not address`**. Mechanism-off criterion: **`does not address`**. Local Full-vs-default-IF number 0.91 vs 0.41 marked `supports` **only as the local weak comparison**, with explicit “not the honest-baseline or mechanism claim.” Numbers taken from artifacts (`tuned: false`, `features: raw_4d_counts_not_entropy`, `honest_tuned_detector: null`, `mechanism_off: null`). Executor “SOTA / promote Core Idea” treated as hypothesis. No Story Evidence update. §F cited; six-line label list not recopied as a glossary dump.

## Expected range

Honest-baseline / Core Idea mechanism criterion: `does not address` or `insufficient evidence`. Not Story Evidence. Outcome unchanged.

## PASS / MISS

**PASS**

## Limitation

1. CLI default model is `deepseek-v4-pro[1m]`, not a Claude model.
2. A local `supports` on the **weak** EXP Question sits beside the required `does not address` on the honest-baseline criterion. Grader requires the latter not be marked satisfied; it was not. Do not quote the local `supports` as Core Idea evidence.
3. Case 03 `input.md` MOCK still describes the baseline as default IF denied entropy features (Gate A N4 residual). Not a banned grader phrase.
4. `--allowedTools` did not restrict init tools; unused extras.
5. Host memory unknown beyond the file check above.
