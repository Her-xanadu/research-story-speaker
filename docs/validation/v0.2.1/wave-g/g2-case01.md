# V0.2.1 Wave G G2 — Case 01 cosmetic successor (Claude Code)

## Identity

| Field | Value |
|-------|--------|
| Harness | Claude Code `/Users/herxanadu/.local/bin/claude` |
| Version | `2.1.220 (Claude Code)` |
| Model | `deepseek-v4-pro[1m]` (CLI default) |
| Context relation | fresh-context. Fresh `/tmp` clone, `--no-session-persistence`, session `f7473757-7867-4f49-a828-f4bbd54ed40c`. Independent of G1. |
| Host memory | n/a for Codex. No `~/.claude/MEMORY.md`. `~/.codex/memories/` fixture-term hits: **0**. Global init skills present, **not invoked**. |
| Clone | `/tmp/rss-v021-g-claude-g2` |
| Clone HEAD | `e612f8ee340130d00b01e0d9346db0096db2b6a2` before and after |
| Agent-visible prompt SHA-256 | `2b804cc61e7ef0f2958d2ae946c0fad23e3a80b356be119b0ae753c86caa0de7` (`input.md` = live-cases Case 01 input, stdin) |
| Grader SHA-256 | `e1b3e50d44bf2ab69095ba7786e777bbe64501716207d2d62352a9d2159325a2` (scored **after**, source `live-cases/case01/grader.md`) |
| Capture | `--output-format stream-json --verbose`. Trace **present**. Logs: `/tmp/rss-v021-g-claude-logs/g2.stdout.jsonl` |

## Isolation (tester, before agent)

Same strip as G1. Canonical eight left as UNINITIALIZED templates. MOCK lives only in stdin `input.md` plus clone-root `artifacts/{wes.yaml,aea.yaml,author-diagram.md}`. Prompt has no expected `REVISE`/`PARK`/`ADVANCE is wrong` sentence (author pitch still says “Please ADVANCE”, which the fixture allows).

## Claude file-read trace

- Capture: stream-json + verbose
- Trace present: **yes**
- Opened `grader.md`: **no**
- Opened `live-cases/README.md`: **no**
- Opened historical `cases/*/README.md` or `prompt-regression`: **no**
- Opened repo-root `README.md`: **no**
- Glob `**/case-01/**`: no match (old fixtures stripped)

## Files read (Read tool)

`.agents/skills/idea-evaluation/SKILL.md`; `.agents/prompts/idea-evaluation.md`; `.agents/references/research-intelligence/idea-and-mechanism.md`; `scientific-reasoning.md`; `experiment-thinking.md`; `artifacts/wes.yaml`; `artifacts/aea.yaml`; `artifacts/author-diagram.md`.

No Bash. No Grep.

## Files written (agent)

Only `.research/work/idea-evaluation-aea.md` (Write). No git add / commit. No EXP row. Canonical eight porcelain empty.

## Canonical files changed?

**No.**

## Skills / prompts / RI loaded

- Skill: `idea-evaluation`
- Prompt: `idea-evaluation.md`
- RI named by those two: `idea-and-mechanism.md`, `scientific-reasoning.md`, `experiment-thinking.md`
- Did **not** load every file under `research-intelligence/` (`evidence-and-claim.md`, `deep-literature-mode.md`, `skill-evolution.md` unread)

## Tokens

| Meter | Count |
|-------|------:|
| `input_tokens` | 33,797 |
| `cache_read_input_tokens` | 88,832 |
| `output_tokens` | 10,620 |
| `num_turns` | 14 |
| `total_cost_usd` | 0.479 |

Token compact target is a Case 10 goal, not scored here.

## Wall time

- Start: 2026-09-04 14:30:07 +08
- End: 2026-09-04 14:32:57 +08
- Wall: **170 s** (`duration_ms` 168,857)
- Exit code: 0

## Observed

Work file has all 11 Skill headings. **Recommended Action: `REVISE`** (cite `idea-and-mechanism.md` §H; four-line glossary not recopied). Mechanism Distinction names §C **fixed weight** + **renaming**; §G deletion test fails (λ 0.5 → 0.3 is not deletion). Author 50-seed grid rejected as non-discriminating. No `ADVANCE`. No EXP-ID.

## Expected range

`REVISE` or `PARK`. Not `ADVANCE`.

## PASS / MISS

**PASS**

## Limitation

1. CLI default model is `deepseek-v4-pro[1m]`, not a Claude model.
2. `--allowedTools` did not restrict init tools; unused extras.
3. Case 01 `input.md` still contains author “Please ADVANCE” and PROJECT principle “Do not treat a renamed diagram as a new method.” (Gate A N4 residual asker-level cue). Artifacts carry `lambda_frozen: true`. Not a grader phrase ban hit.
4. Host memory unknown for any Claude-internal memory outside this clone; no file evidence of fixture hits.
