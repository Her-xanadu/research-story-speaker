# V0.2.1 Wave G2 — Codex Case 01 (unleaked cosmetic successor)

Scorer only. Grader was **not** in the agent-visible tree or prompt.

| Field | Value |
|------|--------|
| Harness | Codex CLI `/Users/herxanadu/bin/codex` (`codex-cli 0.153.0-alpha.5`); tester git `/usr/bin/git` (`2.50.1 (Apple Git-155)`) |
| Model/version | Configured `gpt-5.6-luna`. JSONL had no model field. Thread `01a06b1f-5e78-70a3-83de-65f92a74de1e`. `--sandbox workspace-write --ephemeral --json`. |
| Context relation | **fresh clone** `/tmp/rss-v021-g-codex-c01`. Base `e612f8ee340130d00b01e0d9346db0096db2b6a2`. Isolation HEAD `0d452a4be31e0bdd5a6d7cd500d0f2d64db05940` (unchanged after). On-disk canonical eight left as UNINITIALIZED templates; MOCK only in `input.md` + `./artifacts/`. |
| Host memory check | Pre-run `~/.codex/memories/` fixture/method/expected-action: **0 hits**. This run’s jsonl: **no** `MEMORY.md` query. Generic unrelated REVISE/ADVANCE memories exist on the host; they were not opened. |
| Agent-visible prompt SHA | SHA-256 `2b804cc61e7ef0f2958d2ae946c0fad23e3a80b356be119b0ae753c86caa0de7` (`case01/input.md` at `e612f8e`, stdin unchanged). Author pitch asks to ADVANCE; no “ADVANCE is wrong” / expected action / PASS in the prompt. |
| Grader SHA | SHA-256 `e1b3e50d44bf2ab69095ba7786e777bbe64501716207d2d62352a9d2159325a2`. |
| Files read | `idea-evaluation/SKILL.md`; `idea-evaluation.md` prompt; Layer 2 named by those docs: `scientific-reasoning.md`, `idea-and-mechanism.md`, `experiment-thinking.md`; `artifacts/{wes.yaml,aea.yaml,author-diagram.md}`; UNINITIALIZED canonical eight (read, not used as science). Did **not** open `grader.md` or historical `cases/*/README.md`. |
| Files written | **Only** `.research/work/idea-evaluation-aea.md`. |
| Canonical files changed? | **No.** Eight files byte-identical to source UNINITIALIZED templates. No EXP row. HEAD unchanged. |
| Skill/Prompt/RI loaded | idea-evaluation Skill + prompt; RI `idea-and-mechanism`, `scientific-reasoning`, `experiment-thinking`. Not the full six-file RI boot set (`evidence-and-claim`, `deep-literature-mode`, `skill-evolution` unread). |
| Token count | input **213,355** (cached **174,592**), output **3,748**, reasoning **1,070**. Comparable non-cached+output+reasoning = **43,581**. (Not a compact-target case. V0.2 clean Case 01 was 38,136 stderr tokens — different meter.) |
| Wall time | **95 s** (2026-09-04T14:33:33–14:35:08+08; epoch 1788503613; exit 0). |
| Observed decision | `Recommended Action:` **REVISE** citing `idea-and-mechanism.md` §H. Mechanism Distinction: frozen λ 0.5→0.3, rename of mix→attention, §G deletion test fails. Author 50-seed grid refused. Four-line glossary **not** recopied. |
| Expected range | **REVISE or PARK** (ADVANCE is wrong for this fixture). |
| PASS/MISS | **PASS** |
| Limitation | Same model family as V0.2 Wave G Codex (`gpt-5.6-luna`) — `Model relation: same-model`; `Context relation: fresh-context`. Clone git history still contains graders at `e612f8e` (not at HEAD). Agent did not attempt `git commit`. JSONL has no model field. |

## Isolation (tester)

- Fresh `/tmp` clone; source `.research/` UNINITIALIZED and untouched.
- Copied `input.md` + `artifacts/{wes,aea,author-diagram}` to clone root; did **not** write MOCK into canonical eight.
- Isolation-committed deletion of graders, old `cases/` READMEs, `prompt-regression/`, `gate-reviews/`, `gate-a-review.md`.
- Prompt is the unleaked `input.md` (author pitch + configs), not the grader.

## Grader checklist (scorer)

| Check | Result |
|------|--------|
| Work file with Skill headings | Y (11 headings) |
| Recommended Action REVISE or PARK | Y (**REVISE**) |
| Not ADVANCE | Y |
| Distinction names rename and/or frozen λ | Y (`lambda_frozen`, λ 0.3 vs 0.5, Attention Pool box) |
| No new EXP; canonical eight untouched | Y |
| §H cited; glossary not recopied | Y |

**G2 judgment: PASS.**
