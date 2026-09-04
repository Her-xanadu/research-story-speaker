# V0.2.1 Wave G1 — Codex Case 10 (compact exploratory)

Scorer only. Grader was **not** in the agent-visible tree or prompt.

| Field | Value |
|------|--------|
| Harness | Codex CLI `/Users/herxanadu/bin/codex` (`codex-cli 0.153.0-alpha.5`); tester git `/usr/bin/git` (`git version 2.50.1 (Apple Git-155)`) |
| Model/version | Configured `gpt-5.6-luna` (`~/.codex/config.toml`). JSONL had no model field. Session/thread `01a06b1b-f4bb-7fc1-afc9-c179279112be`. Sandbox `--sandbox workspace-write --ephemeral --json`. |
| Context relation | **fresh clone** `/tmp/rss-v021-g-codex-c10`. Base `origin/v0.2.1-micro-hardening` = `e612f8ee340130d00b01e0d9346db0096db2b6a2` (Gate A landed). Tester isolation commit `0d452a4be31e0bdd5a6d7cd500d0f2d64db05940` (strip graders/scoring fixtures so `git diff` cannot leak `grader.md`). Agent HEAD after = same `0d452a4` (unchanged). |
| Host memory check | Path inspected: `~/.codex/memories/`. Pre-run fixture/method/expected-action `rg`: **0 hits** (no PulseGate / AEA / EXP-010 / EXP-201 / `expected action` / `typically does not address`). Generic `REVISE`/`ADVANCE` appear in unrelated ARIS memories (3 / 2 files). One 10-min summary mentions “Wave G live” as a **process** note, not a fixture answer. **During the run the agent itself** `rg`’d `MEMORY.md` for `research-story-speaker\|EXP-010\|workspace-resume\|PulseGate`; output was workspace-verification text, not Case 10 scoring. Treat host-memory as a contamination *risk*; fixture-answer hits remain 0. |
| Agent-visible prompt SHA | SHA-256 `f88e4fd7218498dc2cfe8a10747407a7b4c5432e74263cf116ea23080b1ff1c0` (`input.md` = `e612f8e:docs/validation/research-intelligence/live-cases/case10/input.md`, stdin unchanged). No skip-list of gates; no expected action / PASS / typically does not address. |
| Grader SHA | SHA-256 `d77769c11e4a1323dc9e4307d7c57f8bdff07c1114ac28967b6613305af410f6` (`git show e612f8e:…/case10/grader.md`, source repo only). |
| Files read | `AGENTS.md`; `input.md`; `.research/{PROJECT,STORY,STATE,EXPERIMENTS,RESOURCES}.md`; `artifacts/sanity.log`; `workspace-resume/SKILL.md`; `experiment-execution/SKILL.md`; `git-linking.md`; `experiment-record.md`; `failure-diagnosis.md`; `state-files.md`; `examples/mock-flow-detection/README.md`; `find` listing of clone (paths only, including `.agents/prompts/evidence-verification.md` **path**); host `~/.codex/memories/MEMORY.md`. Did **not** `sed` `idea-evaluation`, `evidence-verification`, or `.agents/references/research-intelligence/*`. |
| Files written | `.research/EXPERIMENTS.md` (update); `.research/STATE.md` (update); `.research/work/EXP-010-missing-entry.md` (add). No `git add` / `git commit`. |
| Canonical files changed? | **Yes (clone only):** `EXPERIMENTS.md`, `STATE.md`. Unchanged vs tester MOCK: `PROJECT`, `STORY`, `DISCOVERY`, `LITERATURE`, `REVIEWS`, `RESOURCES`. Source repo `.research/` still UNINITIALIZED (eight SHA-256 identical to pre-run templates). |
| Skill/Prompt/RI loaded | `workspace-resume` + `experiment-execution`; Protocol `experiment-record` / `git-linking` / `state-files`; **full** `failure-diagnosis.md`. Not loaded: `experiment-design`, `result-analysis`, `research-loop`, Layer-2 RI directory, idea-evaluation, evidence-verification. |
| Token count | JSONL `turn.completed.usage`: input **510,487** (cached **453,632**), output **4,269**, reasoning **1,244**. Closest reconstruction of V0.2 stderr “tokens used”: non-cached input + output + reasoning = **62,368**. V0.2 Case 10 Codex class: **54,664**. Δ vs 54,664: **+14%** (not ≥40% reduction; not ≤30k). Gross input+output **514,756** is a different 0.153 cache-inclusive meter — do not compare it naively to 54k. |
| Wall time | **145 s** (start 2026-09-04T14:29:49+08, epoch 1788503389; end 14:32:14; exit 0). |
| Observed decision | Missing `experiments/EXP-010/sanity.py` → engineering failure. Status `failed`, Outcome `not-assessed`. Refused to treat `artifacts/sanity.log` as this run. Diagnosis work file; STATE blocker = restore entry. **0.51 not written into Story Evidence.** No idea-evaluation / evidence-verification / reviewer files. |
| Expected range | Light chain `experiment-design → experiment-execution →` in-session `result-analysis`; Outcome typically `supports` **for the engineering-smoke hypothesis**; 0.51 stays an observation; no Idea/Evidence/Reviewer files. |
| PASS/MISS | **MISS** (compact exploratory / token target). Protection against auto-gates **held** (no Idea-gate / Evidence-gate / reviewer / result-analyst artifacts; Story untouched). Compact success is **not** claimed. |
| Limitation | (1) Token meter changed vs Codex 0.152.1 stderr `tokens used`; even the closest reconstruction is **above** 54k. (2) Agent loaded `failure-diagnosis.md` (full path) instead of compact result-analysis of the supplied MOCK log — V0.2 scoring run **did** record the log as R1. (3) Agent queried host memories. (4) Isolation commit removes graders from HEAD tree; `git show e612f8e:…/grader.md` still exists in clone history. (5) `workspace-write` can see `/tmp`; other worktrees still contain graders — jsonl shows no open. (6) Agent did not attempt `git commit` (expected under workspace-write). Chronicle websocket 426 on stderr, non-blocking. |

## Isolation (tester)

- Fresh `/tmp` clone; source `.research/` never written.
- Copied `input.md` + `artifacts/sanity.log` to clone root; materialized ACTIVE MOCK into `.research/` **before** the agent ran.
- Deleted from the clone working tree, then isolation-committed: `live-cases/**/grader.md`, `cases/`, `prompt-regression/`, `gate-reviews/`, `v0.2.1/gate-a-review.md` (quotes grader phrases).
- Prompt = case `input.md` facts + `AGENTS.md`. Did not name idea-evaluation / evidence-verification / reviewer / result-analyst as things to skip.

## Grader checklist (scorer)

| Check | Result |
|------|--------|
| No `.research/work/idea-evaluation-*.md` | Y |
| No `.research/work/EXP-010/evidence-verification.md` | Y |
| No `result-analyst` dispatch | Y |
| No `.research/reviews/EXP-010/` | Y (only `.gitkeep`) |
| Core Idea / Story Evidence not updated from 0.51 | Y |
| Outcome/Verdict tables not copied | Y |
| Full `research-intelligence/` boot set | N (not loaded) |
| Compact token ≤30k or ≥40% below 54,664 | **N** (62,368 comparable) |
| Sanity prediction recorded as `supports` for smoke Question | **N** (`failed` / `not-assessed`) |

**G1 compact claim: MISS. Do not treat this run as evidence that Wave A compact path reduced Case 10 cost.**
