# V0.2.1 Wave G1 — Codex Case 10 compact retune

Scorer only. Grader was **not** in the agent-visible tree or prompt.
Skill commit first: `ee515fe` on `v0.2.1-compact-retune`. This report is the
second commit on the same branch.

| Field | Value |
|------|--------|
| Harness | Codex CLI `/Users/herxanadu/bin/codex` (`codex-cli 0.153.0-alpha.5`); tester git `/usr/bin/git` (`git version 2.50.1 (Apple Git-155)`) |
| Model/version | Configured `gpt-5.6-luna` (`~/.codex/config.toml`). JSONL had no model field. Session/thread `01a06b31-b4df-7682-9528-d48a4a51b418`. Sandbox `--sandbox workspace-write --ephemeral --json`. |
| Context relation | **fresh clone** `/tmp/rss-v021-g-codex-c10-retune` of **this** retune branch. Skill commit `ee515fe0b843e0a71dd975cc4d464772a2beb3f0`. Tester isolation commit `66b991d8882092caba43edffaf659a5893dedf48` (strip graders/scoring fixtures so `git diff` cannot leak `grader.md`). Agent HEAD after = same `66b991d` (unchanged). |
| Host memory check | Path inspected: `~/.codex/memories/` (md only; graphify cache index ignored — short `AEA` false-hits hashes). Pre-run fixture/method/expected-action `rg`: **0 hits** (no PulseGate / EXP-010 / EXP-201 / `expected action` / `typically does not address` / `macro_f1=0.5100`). Generic `REVISE`/`ADVANCE` appear in unrelated memories (3 / 4 md files). `Wave G` in 2 md files as process notes, not a fixture answer. **During the run the agent itself** `rg`’d `MEMORY.md` for `research-story-speaker\|EXP-010\|compact sanity\|PulseGate`; agent said the hit was generic recovery constraints, not Case 10 scoring. Treat host-memory as a contamination *risk*; fixture-answer hits remain 0. |
| Agent-visible prompt SHA | SHA-256 `f88e4fd7218498dc2cfe8a10747407a7b4c5432e74263cf116ea23080b1ff1c0` (`input.md` = `ee515fe:docs/validation/research-intelligence/live-cases/case10/input.md`, stdin unchanged vs G1). No skip-list of gates; no expected action / PASS / typically does not address; **no expected token budget** in the prompt. |
| Grader SHA | SHA-256 `d77769c11e4a1323dc9e4307d7c57f8bdff07c1114ac28967b6613305af410f6` (`live-cases/case10/grader.md`, source / retune worktree only). |
| Files read | `AGENTS.md`; `.agents/skills/experiment-design/SKILL.md`; `experiment-execution/SKILL.md`; `result-analysis/SKILL.md`; `.research/{EXPERIMENTS,RESOURCES,STATE}.md`; `artifacts/sanity.log`; host `~/.codex/memories/MEMORY.md`. `sed -n '1,240p'` (and similar) on the three Skills — that range covers the **whole** Skill body (213 / 145 / 185 lines), including inlined full-flow prose. Did **not** `sed` `workspace-resume`, `research-loop`, `failure-diagnosis.md`, `experiment-proposal.md`, `result-diagnosis.md`, Protocol `experiment-record` / `git-linking` / `state-files`, `README.md`, or `.agents/references/research-intelligence/*`. |
| Files written | `.research/EXPERIMENTS.md` (update); `.research/STATE.md` (update). No diagnosis work file. No `git add` / `git commit`. |
| Canonical files changed? | **Yes (clone only):** `EXPERIMENTS.md`, `STATE.md`. Unchanged vs tester MOCK: `PROJECT`, `STORY`, `DISCOVERY`, `LITERATURE`, `REVIEWS`, `RESOURCES`. Source repo `.research/` still UNINITIALIZED (eight SHA-256 identical to pre-run templates). `compact` / `full` were **not** written into STATE / EXPERIMENTS / Outcome. |
| Skill/Prompt/RI loaded | compact-path Skills: `experiment-design` + `experiment-execution` + `result-analysis`. Not loaded: `workspace-resume`, `research-loop`, Layer-2 RI directory, `failure-diagnosis.md`, `experiment-proposal.md`, `result-diagnosis.md`, idea-evaluation, evidence-verification, Protocol three. |
| Token count | JSONL `turn.completed.usage`: input **199,197** (cached **164,352**), output **2,515**, reasoning **649**. Closest reconstruction of V0.2 stderr “tokens used”: non-cached input + output + reasoning = **38,009**. See comparison table. Gross input+output **201,712** is a different 0.153 cache-inclusive meter — do not compare it naively to 54k. |
| Wall time | **96 s** (start 2026-09-04T14:53:34+08, epoch 1788504814; end 14:55:10; exit 0). |
| Observed decision | Missing `experiments/EXP-010/sanity.py` → no fake rerun. **Did** treat `artifacts/sanity.log` as the usable smoke artifact. Status `completed`, Outcome `supports` **for the engineering-smoke Question only**. Chance-like 0.51 stays an observation; Story Evidence untouched. No idea-evaluation / evidence-verification / reviewer files. |
| Expected range | Light chain `experiment-design → experiment-execution →` in-session `result-analysis`; Outcome typically `supports` **for the engineering-smoke hypothesis**; 0.51 stays an observation; no Idea/Evidence/Reviewer files. Soft token target ≤30k **or** ≥40% below 54,664 (≤ ~32,798). |
| PASS/MISS | Token **soft target MISS** (38,009 is not ≤30k and not ≥40% below 54,664). Load-set and MOCK-log behavior **improved**. Protection against auto-gates **held**. **Do not claim Wave A compact success.** |
| Limitation | (1) 38,009 is **clearly down** vs G1 62,368 (−39%) and V0.2 54,664 (−30%), but the published soft target is unmet — SKILL.md still contains inlined full-flow prose, so reading the Skill is not as small as Gate A §4.4’s “SKILL-only nominal bytes.” (2) Agent queried host memories (same class as G1). (3) Isolation commit removes graders from HEAD tree; `git show ee515fe:…/grader.md` still exists in clone history. (4) `workspace-write` can see `/tmp`; jsonl shows no open of other worktrees’ graders. (5) Agent did not attempt `git commit` (expected under workspace-write). Chronicle websocket 426 + MCP http/request noise on stderr, non-blocking. |

## Comparison (honest comparable meter)

Same method as the G1 MISS report: **non-cached input + output + reasoning**.

| Run | Comparable tokens | vs 54,664 | vs 62,368 | ≤30k | ≥40% below 54,664 |
|-----|------------------:|----------:|----------:|:----:|:-----------------:|
| V0.2 Case 10 Codex (`tokens used` stderr, 0.152.1) | **54,664** | — | — | N | — |
| V0.2.1 G1 Codex MISS (`889ce04` / clone `0d452a4`) | **62,368** | **+14%** | — | N | N |
| This retune (`ee515fe` + isolate `66b991d`) | **38,009** | **−30%** | **−39%** | **N** | **N** (−30%, need −40% ≤ ~32.8k) |

Do **not** sell cache-inclusive 0.153 gross input (199,197 / 201,712) as compact success.

## Isolation (tester)

- Fresh `/tmp` clone of `origin/v0.2.1-compact-retune` at `ee515fe`; source `.research/` never written.
- Copied `input.md` + `artifacts/sanity.log` to clone root; materialized ACTIVE MOCK into `.research/` **before** the agent ran (from `input.md` canonical blocks).
- Deleted from the clone working tree, then isolation-committed: `live-cases/**/grader.md`, `cases/`, `prompt-regression/`, `gate-reviews/`, `v0.2.1/gate-a-review.md` (quotes grader phrases).
- Prompt = case `input.md` facts + `AGENTS.md`. Did not name idea-evaluation / evidence-verification / reviewer / result-analyst as things to skip. Did not put a token target in the prompt.

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
| Compact token ≤30k or ≥40% below 54,664 | **N** (38,009 comparable) |
| Sanity prediction recorded as `supports` for smoke Question | **Y** (`completed` / `supports`; MOCK log used) |

**G1 compact claim after retune: still not the Wave A token target. Do not treat 38,009 as evidence that compact reduced Case 10 to ≤30k or −40%.** It **is** honest evidence that the default load set is now the compact Skills (no `workspace-resume`, no full `failure-diagnosis.md`, no RI folder) and that the MOCK log is scored as the smoke artifact.
