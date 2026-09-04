# V0.2.1 Wave G G1 — Case 10 compact exploratory (Claude Code)

## Identity

| Field | Value |
|-------|--------|
| Harness | Claude Code `/Users/herxanadu/.local/bin/claude` |
| Version | `2.1.220 (Claude Code)` |
| Model | `deepseek-v4-pro[1m]` (CLI default; `provider: firstParty`; 1M context). Not Anthropic Claude. |
| Context relation | fresh-context. Fresh `/tmp` clone, `--no-session-persistence`, new session `5fe8b2d5-2ec2-4d24-8224-c640325471ac`. |
| Host memory | n/a for Codex. No `~/.claude/MEMORY.md`. `~/.codex/memories/MEMORY.md` fixture-term hits: **0** (Claude does not load it). Init skill list included global extras (`computer-work-daily`, `ego-browser`, `experiment-loop`, `deep-research`, plugins) — **not invoked**. |
| Clone | `/tmp/rss-v021-g-claude-g1` (macOS `/tmp` → `/private/tmp`) |
| Clone HEAD | `e612f8ee340130d00b01e0d9346db0096db2b6a2` before and after |
| Agent-visible prompt SHA-256 | `f88e4fd7218498dc2cfe8a10747407a7b4c5432e74263cf116ea23080b1ff1c0` (`input.md` = live-cases Case 10 input, stdin) |
| Grader SHA-256 | `d77769c11e4a1323dc9e4307d7c57f8bdff07c1114ac28967b6613305af410f6` (scored **after**, from source `live-cases/case10/grader.md`, never in the clone) |
| Capture | `--output-format stream-json --verbose`. Trace **present**. Logs: `/tmp/rss-v021-g-claude-logs/g1.stdout.jsonl` |

## Isolation (tester, before agent)

- Clone `origin/v0.2.1-micro-hardening` at `e612f8e`.
- Deleted from clone: every `grader.md`; `docs/validation/research-intelligence/cases/`; `prompt-regression/`; remaining `live-cases/`.
- Copied Case 10 `input.md` → clone-root `input.md`; `artifacts/sanity.log` → clone-root `artifacts/`.
- Tester wrote ACTIVE MOCK PulseGate + planned EXP-010 into clone `.research/` only (source repo untouched). Needed so `AGENTS.md` does not fall into `workspace-resume`.
- Prompt = `input.md` only. **No** skip-list of idea-evaluation / evidence-verification / reviewer / result-analyst. **No** expected Outcome / PASS sentence.

## Claude file-read trace

- Capture: `--output-format stream-json` + `--verbose`
- Trace present: **yes**
- Opened `grader.md`: **no**
- Opened `live-cases/README.md`: **no** (deleted)
- Opened historical `cases/*/README.md` or `prompt-regression`: **no** (deleted). Glob attempted `docs/validation/research-intelligence/cases/10-ordinary-exploratory/**` — empty.
- Opened repo-root `README.md`: **yes** (see Limitation 1)

## Files read (Read tool)

`AGENTS.md`; `.research/STATE.md`; `.research/EXPERIMENTS.md`; `input.md`; `README.md`; `artifacts/sanity.log`; `.agents/references/state-files.md`; `.agents/references/experiment-record.md`; `.agents/references/research-intelligence/evidence-and-claim.md`; `.agents/skills/experiment-execution/SKILL.md`; `.agents/skills/result-analysis/SKILL.md`.

Also ingested via Bash `cat` (not Read): clone `.research/{PROJECT,STORY,DISCOVERY,RESOURCES}.md`; `examples/mock-flow-detection/.research/{RESOURCES,EXPERIMENTS}.md` and that example README; `.gitignore`. `find .agents -type f` listed names only.

## Files written (agent)

- Edit `.research/EXPERIMENTS.md` (three times): Status `planned` → `failed`; Outcome stays `not-assessed`; recorded technical non-run (code absent); MOCK log explicitly not a result.
- Edit `.research/STATE.md`: blocker = `CB-omega-mock` missing; next action restore checkout or return to Open Gap 1.

No `.research/work/idea-evaluation-*.md`. No `.research/work/EXP-010/evidence-verification.md`. No `.research/reviews/EXP-010/`. Work/reviews still `.gitkeep` only. No git add / commit. HEAD unchanged.

## Canonical files changed?

**Yes, by agent:** `EXPERIMENTS.md`, `STATE.md` only.

Tester MOCK already differed from UNINITIALIZED templates on PROJECT / STORY / DISCOVERY / RESOURCES / LITERATURE / EXPERIMENTS / STATE. Agent did **not** further edit STORY / DISCOVERY. Core Idea unchanged. Evidence section still “No EXP in Story Evidence”. `0.51` / `0.5100` not written into STORY or DISCOVERY. EXPERIMENTS does not copy the Outcome table.

## Skills / prompts / RI loaded

| Loaded | Not loaded (trace) |
|--------|--------------------|
| `experiment-execution` Skill, `result-analysis` Skill | `experiment-design` Skill (EXP already `planned`; skipped) |
| Protocol: `state-files.md`, `experiment-record.md` | `research-loop` SKILL.md as a file |
| RI: `evidence-and-claim.md` only | Full `research-intelligence/` boot set; `idea-evaluation`; `evidence-verification`; `experiment-review`; `deep-literature-mode.md` |

No Skill / Task / Agent tool calls. `result-analyst` not dispatched.

## Tokens

V0.2 Codex Case 10 reference: **54,664** (stderr `tokens`).

This run (`result.usage`):

| Meter | Count |
|-------|------:|
| `input_tokens` (uncached) | **50,170** |
| `cache_read_input_tokens` | 522,368 |
| `cache_creation_input_tokens` | 0 |
| `output_tokens` | 14,448 |
| uncached input+output | 64,618 |
| all three input-side + output | 586,986 |
| `num_turns` | 29 |
| `total_cost_usd` | 0.873 |

Uncached input vs 54,664: **−8.2%**. Soft target was ≥40% reduction **or** ≤30k. Neither met. Cache-read is large because the CLI default model is a 1M-context ID; **do not** treat 50,170 as a compact-path win, and do not compare cache_read to the Codex 54k meter as if they were the same unit.

## Wall time

- Start: 2026-09-04 14:26:09 +08
- End: 2026-09-04 14:30:07 +08
- Wall: **238 s** (`result.duration_ms` 236,068)
- Exit code: 0

## Observed

Ordinary exploratory, honest stop: `sanity.py` / `CB-omega-mock` absent in the clone; MOCK `artifacts/sanity.log` (`macro_f1=0.5100`) **not** treated as a real run or Story Evidence. EXP-010 Outcome left `not-assessed` (technical non-run, not a scientific negative). No Idea-gate / Evidence-gate / Reviewer / `result-analyst` work files.

## Expected range

- Compact tokens: ≤30k **or** ≥40% below ~54,664 (≤ ~32.8k uncached-equivalent).
- Protection (grader): `experiment-design` → `experiment-execution` → in-session `result-analysis` (or honest stop); no gate/reviewer files; 0.51 not laundered into Evidence.

## PASS / MISS

| Axis | Verdict |
|------|---------|
| Compact token target | **MISS** |
| Protection / grader (file-level) | **PASS** (see Limitation 1) |
| **G1 overall** | **MISS** — do not claim compact success |

## Limitation

1. Agent **Read** repo-root `README.md`, which states Case 10 file-level PASS = no Idea/Evidence/Reviewer work files, and also names Case 01 `REVISE` / Case 03 `does not address`. Protection PASS is **file-level and scoring-visible**, not unleaked. Same class as V0.2 Case 10 Limitation.
2. Loaded `evidence-and-claim.md` (one RI file), not the full boot set. Extra vs compact default; not a grader FAIL.
3. Did not Read `experiment-design` SKILL; EXP was already planned. Honest non-run is allowed.
4. `--allowedTools` did not actually shrink the init tool list (Task / WebSearch / MCP still present). Unused in this session.
5. Token meters differ from Codex stderr `tokens`. Even on Claude `input_tokens`, compact target is a miss.
6. Host model is DeepSeek via Claude Code, not a Claude model. Cross-harness claims must name that.
