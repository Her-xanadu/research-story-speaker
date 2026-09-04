# V0.2.1 Wave G1 — Claude Code Case 10 compact retune

Scorer only. Grader was **not** in the agent-visible tree or prompt.
Skill commit: `a81f0ca` (`V0.2.1 Wave A: compact skill stop-fold`) on
`v0.2.1-compact-retune`. Pre-retune Claude G1 (50,170, read README) is on
`origin/v0.2.1-wave-g-claude`. Do **not** claim compact success.

## Identity

| Field | Value |
|-------|--------|
| Harness | Claude Code `/Users/herxanadu/.local/bin/claude` |
| Version | `2.1.220 (Claude Code)` |
| Model | `deepseek-v4-pro[1m]` (CLI default; `provider: firstParty`; 1M context). Not Anthropic Claude. |
| Context relation | fresh-context. Fresh `/tmp` clone of **this** stop-fold branch, `--no-session-persistence`, new session `f910f92e-05ef-43eb-99bf-a2538f9ecb01`. |
| Host memory | n/a for Codex. No `~/.claude/MEMORY.md`. Init skill list included global extras (`computer-work-daily`, `ego-browser`, `experiment-loop`, `deep-research`, plugins) — **not invoked**. |
| Clone | `/tmp/rss-v021-g-claude-c10-retune` (macOS `/tmp` → `/private/tmp`) |
| Clone HEAD | `ea83e7f2176f48a13c604a64ee9d817ca76b4730` (isolation) before and after; parent Skill `a81f0ca693ed1d7c5dcbde09f681dca09e1b29df` |
| Agent-visible prompt SHA-256 | `f88e4fd7218498dc2cfe8a10747407a7b4c5432e74263cf116ea23080b1ff1c0` (`input.md` = live-cases Case 10 input, stdin) |
| Grader SHA-256 | `d77769c11e4a1323dc9e4307d7c57f8bdff07c1114ac28967b6613305af410f6` (scored **after**, from source `live-cases/case10/grader.md`, never in the clone) |
| Capture | `--output-format stream-json --verbose`. Trace **present**. Logs: `/tmp/rss-v021-g-claude-retune-logs/c10.stdout.jsonl` |

## Isolation (tester, before agent)

- Clone `origin/v0.2.1-compact-retune` at Skill `a81f0ca`.
- Deleted from clone: every `grader.md`; historical `cases/`; `prompt-regression/`; remaining `live-cases/`; `gate-reviews/`; `v0.2.1/gate-a-review.md`. Isolation-committed as `ea83e7f`.
- Copied Case 10 `input.md` → clone-root `input.md`; `artifacts/sanity.log` → clone-root `artifacts/`.
- Tester wrote ACTIVE MOCK PulseGate + planned EXP-010 into clone `.research/` only (source repo untouched). Needed so `AGENTS.md` does not fall into `workspace-resume`.
- Prompt = `input.md` only. **No** skip-list of idea-evaluation / evidence-verification / reviewer / result-analyst. **No** expected Outcome / PASS sentence. **No** token target.

## Claude file-read trace

- Capture: `--output-format stream-json` + `--verbose`
- Trace present: **yes**
- Opened `grader.md`: **no**
- Opened `live-cases/README.md`: **no** (deleted)
- Opened historical `cases/*/README.md` or `prompt-regression`: **no** (deleted)
- Opened repo-root `README.md`: **no** (unlike pre-retune Claude G1)
- Opened Protocol / RI (`state-files.md`, `experiment-record.md`, `evidence-and-claim.md`): **no** (unlike pre-retune Claude G1)
- Read three compact Skills as **whole files** (Read tool, no offset/limit) — stop-fold **not** followed
- Bash `git show --stat` of `ea83e7f` and `f57f34b` (path/stat only of isolation + previous Codex retune evidence). Filename `g1-case10-codex-retune.md` therefore appeared in the trace; content of that report was **not** Read.

## Files read (Read tool)

`AGENTS.md`; `.research/{RESOURCES,EXPERIMENTS,STATE,DISCOVERY,STORY}.md`; `input.md`; `artifacts/sanity.log`; `.gitignore`; `.agents/skills/experiment-execution/SKILL.md`; `result-analysis/SKILL.md`; `experiment-design/SKILL.md`.

Bash (not Read): `find` / `ls` / `git ls-files` / `git log` / `git show --stat` / clone-wide `grep` for code markers. No `cat` of root `README.md`.

## Files written (agent)

- Edit `.research/EXPERIMENTS.md` (three times): Status `planned` → `completed`; Outcome `supports` **for the engineering-smoke Question**; MOCK log recorded as the artifact; no fake re-run.
- Edit `.research/STATE.md`: next action = locate `CB-omega-mock` then Open Gap 1; blocker = codebase absent.

No `.research/work/idea-evaluation-*.md`. No `.research/work/EXP-010/evidence-verification.md`. No `.research/reviews/EXP-010/`. Work/reviews still `.gitkeep` only. No git add / commit. HEAD unchanged.

## Canonical files changed?

**Yes, by agent:** `EXPERIMENTS.md`, `STATE.md` only.

Tester MOCK already differed from UNINITIALIZED templates. Agent did **not** further edit STORY / DISCOVERY. Core Idea unchanged. Evidence section still “No EXP in Story Evidence”. `0.51` / `0.5100` not written into STORY or DISCOVERY. EXPERIMENTS does not copy the Outcome table. `compact` / `full` not written into STATE / EXPERIMENTS / Outcome.

## Skills / prompts / RI loaded

| Loaded | Not loaded (trace) |
|--------|--------------------|
| `experiment-design`, `experiment-execution`, `result-analysis` Skills (**whole files**) | Protocol: `state-files.md`, `experiment-record.md` |
| | RI: `evidence-and-claim.md`; full `research-intelligence/` boot set |
| | `workspace-resume`; `research-loop` SKILL.md as a file |
| | `idea-evaluation`; `evidence-verification`; `experiment-review`; `deep-literature-mode.md` |

No Skill / Task / Agent tool calls. `result-analyst` not dispatched.

## Tokens

V0.2 Codex Case 10 reference: **54,664** (stderr `tokens`).
Pre-retune Claude G1: **50,170** uncached `input_tokens`.

This run (`result.usage`):

| Meter | Count |
|-------|------:|
| `input_tokens` (uncached) | **44,765** |
| `cache_read_input_tokens` | 248,704 |
| `cache_creation_input_tokens` | 0 |
| `output_tokens` | 11,672 |
| uncached input+output | 56,437 |
| all three input-side + output | 305,141 |
| `num_turns` | 23 |
| `total_cost_usd` | 0.640 |
| `duration_ms` | 131,933 |

Uncached input vs 54,664: **−18%**. vs Claude G1 50,170: **−11%**. Soft target was ≥40% reduction **or** ≤30k. Neither met. Cache-read is large because the CLI default model is a 1M-context ID; **do not** treat 44,765 as a compact-path win, and do not compare cache_read to the Codex 54k meter as if they were the same unit.

## Wall time

- Start: 2026-09-04 15:04:21 +08
- End: 2026-09-04 15:06:34 +08
- Wall: **133 s** (`result.duration_ms` 131,933)
- Exit code: 0

## Observed

Ordinary exploratory compact chain: EXP already `planned` (verified, not rewritten); `sanity.py` / `CB-omega-mock` absent; supplied MOCK `artifacts/sanity.log` treated as the usable smoke artifact. Status `completed`, Outcome `supports` **for the engineering-smoke Question**. Chance-like 0.51 stays an observation; Story Evidence untouched. No Idea-gate / Evidence-gate / Reviewer / `result-analyst` work files.

## Expected range

- Compact tokens: ≤30k **or** ≥40% below ~54,664 (≤ ~32.8k uncached-equivalent).
- Protection (grader): `experiment-design` → `experiment-execution` → in-session `result-analysis` (or honest stop); no gate/reviewer files; 0.51 not laundered into Evidence.

## PASS / MISS

| Axis | Verdict |
|------|---------|
| Compact token target | **MISS** (44,765 uncached input) |
| Protection / grader (file-level) | **PASS** |
| README leak (G1 class) | **held this run** (README not Read) |
| Stop-fold followed | **N** (whole SKILL.md Read) |
| **G1 overall** | **MISS** — do not claim compact success |

## Comparison (honest; different meters)

Codex comparable = non-cached input + output + reasoning. Claude column is uncached `input_tokens` (G1’s published meter). Do not mix them into one “win”.

| Run | Published meter | vs 54,664 | ≤30k | ≥40% below 54,664 |
|-----|----------------:|----------:|:----:|:-----------------:|
| V0.2 Case 10 Codex | **54,664** | — | N | — |
| Claude G1 pre-retune (`origin/v0.2.1-wave-g-claude`) | **50,170** uncached in | **−8%** | N | N |
| Codex G1 | **62,368** comparable | **+14%** | N | N |
| Codex retune1 | **38,009** comparable | **−30%** | N | N |
| Codex retune2 (sibling report) | **40,321** comparable | **−26%** | N | N |
| This Claude retune | **44,765** uncached in | **−18%** | **N** | **N** |

## Limitation

1. Token target still MISS. Stop-fold did not prevent whole-file `Read` of the three Skills (no offset/limit). Same residual as Codex retune2’s `sed 1,240p`.
2. Did **not** Read root `README.md` this time (G1 did). File-level protection PASS is cleaner than G1, but still not unleaked against git-history `--stat` of `f57f34b` (previous Codex retune evidence filename on this branch).
3. `--allowedTools` did not shrink the init tool list (Task / WebSearch / MCP still present). Unused except Read/Bash/Edit.
4. Host model is DeepSeek via Claude Code, not a Claude model. Cross-harness claims must name that.
5. Isolation commit removes graders from HEAD; `git show a81f0ca:…/grader.md` still exists in clone history. Agent `grep`’d the clone path; no grader file remained on disk.
6. Do **not** retune fixtures to chase 30k.

**G1 compact claim after stop-fold on Claude: still not the Wave A token target.** Behavior (MOCK log as `supports`; no auto-gates; no README) improved vs Claude G1. Byte load of SKILL.md did not fold.
