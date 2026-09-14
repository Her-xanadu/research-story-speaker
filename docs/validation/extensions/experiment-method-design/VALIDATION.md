# Validation — experiment-method-design

Instruction-only localization. No live scientific Gate. No runtime
scripts added to the product.

## Frozen counts (must hold)

| Item | Expected | Result |
|------|----------|--------|
| Canonical `.research/*.md` | 8 | PASS (8) |
| `research-loop` Skill | 1 | PASS |
| Subagents | 5 | PASS (5) |
| Skills | 15 | PASS (15; no 16th) |
| RI files | 6 | PASS (6) |
| Runtime scripts under `.agents` / `adapters` | 0 | PASS |

## Overlap / reject

| Upstream surface | Local decision |
|------------------|----------------|
| `experiment-design` (decision criteria, data-path-first) | **EXTEND_EXISTING** — restated as §I, §B large-gain, compact notes |
| `benchmark-and-baseline-selector` (claim type → MINIMAL/SUGGESTED) | **REJECT Skill**; restated as §J using RSS claim kinds + Must/Nice |
| `hypothesis-and-ablation-planner` | **REJECT** — `experiment-thinking.md` §G already owns this |
| `statistical-testing-guide` + `stats.py` | **REJECT** — replicate when uncertainty would change a decision; no script |
| `model-eval-error-analysis` | **REJECT this round** — result-time; not method design |
| `research-idea-stress-test` | **REJECT** — Idea-gate already owns cheapest falsifier |
| Install CLI / control plane | **REJECT** |

## Instruction contracts

### Compact `experiment-design` (above stop line)

Must still:

- keep exactly five in-session items;
- forbid opening `experiment-proposal.md`, `experiment-thinking.md`,
  `scientific-reasoning.md`, `idea-evaluation`;
- say a missing publication baseline taxonomy is **not** a full-design
  trigger;
- name pre-run method-consequence verbs and the large-gain split check
  without opening Layer 2.

Must not:

- add a sixth EXP field;
- add Status / Outcome / Idea-gate tokens;
- auto-upgrade ordinary sanity.

### Full design

- Walk is `A→J` (not `A→H`).
- `experiment-proposal.md` cites §I and §J; does not recopy claim-kind
  or Idea-gate tables.
- `method-review.md` cites the same two operators; it still skips a
  one-line sanity rerun.

### Layer 2 owner

- `experiment-thinking.md` has `## I. Pre-run method consequence` and
  `## J. Claim-matched comparison`.
- Large-gain paragraph lives under §B.
- Using-this-file still forbids firing idea-evaluation / a baseline
  essay on sanity.

## Copied-text ban (unknown license)

Local instruction files must **not** contain these upstream-distinct
strings:

```text
MINIMAL vs SUGGESTED
Trivial lower bound
fit_scaling_law
Kaplan/Chinchilla
pursue / revise / stop thresholds
Holm
```

`Must-run` / `Nice-to-have` and `keep` / `simplify` / `delete component`
/ `change mechanism` / `abandon` are local RSS vocabulary.

## Protection case (ordinary exploratory)

A one-line sanity EXP:

- stays compact;
- does not open `idea-evaluation`, `experiment-proposal.md`, or
  `experiment-thinking.md`;
- does not require a four-slot baseline taxonomy;
- still writes the three pre-run readings into the existing Expected
  outcomes note.

## Expected-use case (full isolating design)

A mechanism claim:

- loads `experiment-proposal.md` and walks A→J;
- must-run includes an isolating control (§J);
- Stop Condition / Expected outcomes name the three pre-run readings
  (§I).

## How to re-run

From the repository root, re-check frozen counts, compact stop-line
protection, A→J walk, §I/§J headings, copied-text ban, and registry row.
Do not add a product script. The 2026-09-14 run is summarized below.

## Run 2026-09-14 (static)

`ALL PASS`. Frozen counts held: canonical 8 · skills 15 · subagents 5 ·
RI 6 · research-loop 1 · scripts 0. No new skill directories. Compact
stop-line still forbids `experiment-proposal.md`, `experiment-thinking.md`,
`idea-evaluation`, and `evidence-verification`. Five in-session items
unchanged. Copied-text ban held. `.research/` untouched. Harness
`.claude/skills` and `.cursor/skills` remain 15 symlinks to `.agents/skills`.
