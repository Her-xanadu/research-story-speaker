# V0.2.1 Wave G1 — Codex Case 10 compact retune2

Scorer only. Grader was **not** in the agent-visible tree or prompt.
Skill commit first: `a81f0ca` (`V0.2.1 Wave A: compact skill stop-fold`) on
`v0.2.1-compact-retune`. This report is the evidence commit on the same
branch. Do **not** claim compact success.

| Field | Value |
|------|--------|
| Harness | Codex CLI `/Users/herxanadu/bin/codex` (`codex-cli 0.153.0-alpha.5`); tester git `/usr/bin/git` (`git version 2.50.1 (Apple Git-155)`) |
| Model/version | Configured `gpt-5.6-luna` (`~/.codex/config.toml`). JSONL had no model field. Session/thread `01a06b3b-92c3-7431-8bc7-eaaa07f69c0c`. Sandbox `--sandbox workspace-write --ephemeral --json`. |
| Context relation | **fresh clone** `/tmp/rss-v021-g-codex-c10-retune2` of **this** retune branch after the stop-fold Skill commit. Skill commit `a81f0ca693ed1d7c5dcbde09f681dca09e1b29df`. Tester isolation commit `ea83e7f2176f48a13c604a64ee9d817ca76b4730` (strip graders/scoring fixtures so `git diff` cannot leak `grader.md`). Agent HEAD after = same `ea83e7f` (unchanged). |
| Host memory check | Path inspected: `~/.codex/memories/` (md only; graphify cache index ignored). Pre-run fixture/method/expected-action `rg`: **0 hits** (no PulseGate / EXP-010 / EXP-201 / `expected action` / `typically does not address` / `macro_f1=0.5100`). Generic `REVISE`/`ADVANCE` appear in unrelated memories (3 / 4 md files). `Wave G` in 2 md files as process notes, not a fixture answer. **During the run the agent itself** `rg`’d `MEMORY.md` for `research-story-speaker\|EXP-010\|workspace-resume\|experiment-execution\|result-analysis\|sanity`. Treat host-memory as a contamination *risk*; fixture-answer hits remain 0. |
| Agent-visible prompt SHA | SHA-256 `f88e4fd7218498dc2cfe8a10747407a7b4c5432e74263cf116ea23080b1ff1c0` (`input.md` = `a81f0ca:docs/validation/research-intelligence/live-cases/case10/input.md`, stdin unchanged vs G1 / retune1). No skip-list of gates; no expected action / PASS / typically does not address; **no expected token budget** in the prompt. |
| Grader SHA | SHA-256 `d77769c11e4a1323dc9e4307d7c57f8bdff07c1114ac28967b6613305af410f6` (`live-cases/case10/grader.md`, source / retune worktree only). |
| Files read | `AGENTS.md` (Codex auto-load); `.agents/skills/experiment-design/SKILL.md`; `experiment-execution/SKILL.md`; `result-analysis/SKILL.md`; `.research/{EXPERIMENTS,RESOURCES,STATE}.md`; `artifacts/sanity.log`; host `~/.codex/memories/MEMORY.md`. `sed -n '1,240p'` / `'1,280p'` on the three Skills — that range still covers the **whole** Skill body (226 / 165 / 196 lines), **including the below-fold full-flow remainder**. The stop line was in the file; the agent did **not** stop. Did **not** `sed` `workspace-resume`, `research-loop`, `failure-diagnosis.md`, `experiment-proposal.md`, `result-diagnosis.md`, Protocol `experiment-record` / `git-linking` / `state-files`, root `README.md`, or `.agents/references/research-intelligence/*`. `rg --files` listed paths only (including leftover `docs/validation/v0.2.1/wave-g/g1-case10-codex-retune.md` from this branch). |
| Files written | `.research/EXPERIMENTS.md` (update); `.research/STATE.md` (update). No diagnosis work file. No `git add` / `git commit`. |
| Canonical files changed? | **Yes (clone only):** `EXPERIMENTS.md`, `STATE.md`. Unchanged vs tester MOCK: `PROJECT`, `STORY`, `DISCOVERY`, `LITERATURE`, `REVIEWS`, `RESOURCES`. Source repo `.research/` still UNINITIALIZED (eight SHA-256 identical to pre-run templates). `compact` / `full` were **not** written into STATE / EXPERIMENTS / Outcome. |
| Skill/Prompt/RI loaded | compact-path Skills: `experiment-design` + `experiment-execution` + `result-analysis`, but **as whole files** (stop-fold ignored). Not loaded: `workspace-resume`, `research-loop`, Layer-2 RI directory, `failure-diagnosis.md`, `experiment-proposal.md`, `result-diagnosis.md`, idea-evaluation, evidence-verification, Protocol three. |
| Token count | JSONL `turn.completed.usage`: input **272,220** (cached **234,496**), output **2,123**, reasoning **474**. Closest reconstruction of V0.2 stderr “tokens used”: non-cached input + output + reasoning = **40,321**. See comparison table. Gross input+output **274,343** is a different 0.153 cache-inclusive meter — do not compare it naively to 54k. |
| Wall time | **92 s** (start 2026-09-04T15:04:21+08, epoch 1788505461; end 15:05:53; exit 0). |
| Observed decision | Missing `experiments/EXP-010/sanity.py` → no fake rerun. **Did** treat `artifacts/sanity.log` as the usable smoke artifact. Status `completed`, Outcome `supports` **for the engineering-smoke Question only**. Chance-like 0.51 stays an observation; Story Evidence untouched. No idea-evaluation / evidence-verification / reviewer files. |
| Expected range | Light chain `experiment-design → experiment-execution →` in-session `result-analysis`; Outcome typically `supports` **for the engineering-smoke hypothesis**; 0.51 stays an observation; no Idea/Evidence/Reviewer files. Soft token target ≤30k **or** ≥40% below 54,664 (≤ ~32,798). |
| PASS/MISS | Token **soft target MISS** (40,321 is not ≤30k and not ≥40% below 54,664). Load-set vs G1 still improved (no `workspace-resume` / no `failure-diagnosis.md` / no RI folder). Protection against auto-gates **held**. MOCK-log `supports` **held**. **Do not claim Wave A compact success.** Stop-fold did **not** shrink the Skill bytes actually ingested. |
| Limitation | (1) 40,321 is **down** vs G1 62,368 (−35%) and vs V0.2 54,664 (−26%), but **slightly up** vs retune1 38,009. Same class of load: the agent `sed`’d past the stop line through the full-flow remainder. Instruction-only fold is not a physical compact file. (2) Soft target unmet — do **not** retune fixtures to chase 30k. (3) Agent queried host memories (same class as G1 / retune1). (4) Isolation commit removes graders from HEAD tree; `git show a81f0ca:…/grader.md` still exists in clone history. Previous retune report `g1-case10-codex-retune.md` remains on this branch and was **path-listed**, not opened. (5) `workspace-write` can see `/tmp`; jsonl shows no open of other worktrees’ graders. (6) Agent did not attempt `git commit` (expected under workspace-write). Chronicle websocket 426 on stderr, non-blocking. |

## Comparison (honest comparable meter)

Same method as the G1 MISS report and retune1: **non-cached input + output + reasoning**.

| Run | Comparable tokens | vs 54,664 | vs 62,368 | ≤30k | ≥40% below 54,664 |
|-----|------------------:|----------:|----------:|:----:|:-----------------:|
| V0.2 Case 10 Codex (`tokens used` stderr, 0.152.1) | **54,664** | — | — | N | — |
| V0.2.1 G1 Codex MISS (`889ce04` / clone `0d452a4`) | **62,368** | **+14%** | — | N | N |
| Retune1 (`ee515fe` + isolate `66b991d`) | **38,009** | **−30%** | **−39%** | **N** | **N** |
| This retune2 (`a81f0ca` + isolate `ea83e7f`) | **40,321** | **−26%** | **−35%** | **N** | **N** (−26%, need −40% ≤ ~32.8k) |

Do **not** sell cache-inclusive 0.153 gross input (272,220 / 274,343) as compact success.
Do **not** treat 40,321 vs 38,009 as a Skill regression that should be “fixed” by rewriting Case 10.

## Isolation (tester)

- Fresh `/tmp` clone of `origin/v0.2.1-compact-retune` at `a81f0ca`; source `.research/` never written.
- Copied `input.md` + `artifacts/sanity.log` to clone root; materialized ACTIVE MOCK into `.research/` **before** the agent ran (from `input.md` canonical blocks).
- Deleted from the clone working tree, then isolation-committed: `live-cases/` (entire tree after copy), `cases/`, `prompt-regression/`, `gate-reviews/`, `v0.2.1/gate-a-review.md`.
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
| Compact token ≤30k or ≥40% below 54,664 | **N** (40,321 comparable) |
| Sanity prediction recorded as `supports` for smoke Question | **Y** (`completed` / `supports`; MOCK log used) |

**G1 compact claim after stop-fold retune2: still not the Wave A token target.** The compact *decision* (MOCK log as smoke artifact; no auto-gates) held. The compact *byte load* did not: SKILL.md was still ingested whole. **Do not treat 40,321 as evidence that compact reduced Case 10 to ≤30k or −40%.**
