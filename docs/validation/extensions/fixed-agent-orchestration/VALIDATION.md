# Validation — Fixed Multi-Agent Orchestration

Scope: prove the default-role-dispatch change actually causes **native subagents**
to own bounded work segments (not Main role-play), that high-output work stays in
the sub-context while Main receives only a short decision summary + disk pointer,
that independent tasks truly overlap, that boundaries hold (no subagent writes
canonical state), and that all three harness configs are valid per official docs.

Acceptance is **behavioral**, not file-counting.

## A. Behavioral — Cursor native path (VERIFIED here)

Low-cost mock flow on `examples/mock-flow-detection/` (a self-contained MOCK
instance; the repo-root `.research/` stays `UNINITIALIZED`). Real `Task
subagent_type=<role>` spawns were used — no Main role-play.

Flow: Main → (`experiment-agent` ∥ `literature-scout`, one parallel batch) →
`result-analyst` → Main integrates.

| Assertion | Result | Evidence |
|-----------|--------|----------|
| Native role/thread actually spawned (not Main) | PASS | 3 distinct agent ids: `bc-fec88786…` (experiment-agent), `bc-4c65ca40…` (literature-scout), `bc-dd7970b5…` (result-analyst) |
| Independent pair truly overlaps | PASS | `experiment-agent` + `literature-scout` dispatched in one batch, ran concurrently, returned independently |
| High-output work stays in sub-context; Main gets summary + pointer | PASS | Full run/consult/analysis bodies written to `…/.research/work/*.md`; each returned **only** the 7-field Two-tier decision summary |
| `experiment-agent` owns the full run segment to terminal | PASS | held the instant run to terminal, returned Finding (`acc 0.98`) + Evidence pointer |
| `result-analyst` is the default receiver and reads upstream report **from disk** (context isolation) | PASS | Main did **not** paste the executor report; analyst confirmed it opened `EXP-MOCK-01-run.md` itself and added independent value (caught the tautological/circular design the executor did not flag) |
| `literature-scout` boundary: no web search, returns `NEEDS_REFRESH` | PASS | returned `consult_status=unavailable` + `NEEDS_REFRESH`, wrote only its work file |
| Subagents never write canonical state | PASS | after the whole flow, `git status` showed **only** `…/work/EXP-MOCK-01-run.md`, `…/work/EXP-MOCK-01-analysis.md`, `…/work/LIT-MOCK-01.md`; no `PROJECT/STORY/STATE/DISCOVERY/EXPERIMENTS/LITERATURE/REVIEWS/RESOURCES` touched, root `.research/` still `UNINITIALIZED` |
| Ordinary EXP does not auto-spawn a reviewer | PASS | no `reviewer` dispatched in the ordinary flow |

The mock scratch work files were removed after capture to keep the curated
example clean; re-running the flow regenerates them.

## B. Static cross-check — all three harnesses (VERIFIED here)

- Codex `.codex/agents/*.toml` parse (Python `tomllib`); required
  `name`/`description`/`developer_instructions` present. `research-lead` &
  `reviewer` keep `model_reasoning_effort = "xhigh"`; `result-analyst` is
  dual-tier (no pinned effort; Main raises to `xhigh` for full).
- Claude `.claude/agents/*.md` and Cursor `.cursor/agents/*.md` frontmatter
  parse (YAML) with non-empty bodies (**required** for Cursor registration).
  Claude strongest roles carry `model: opus` + `effort: xhigh`; `result-analyst`
  is `model: inherit` (compact) with full raised to `opus`+`xhigh`.
- Wrapper write-permissions now consistent with the canonical role file:
  all three `experiment-agent` wrappers authorize `.research/work/` **and** the
  code/result paths for a reserved EXP-ID (previously the conflicting "Write
  only `.research/work/`").
- Skill count unchanged: **15** SKILL.md dirs, mirrored in `.claude/skills/`
  and `.cursor/skills/`.

## C. Codex & Claude — runnable checklist (RUNTIME-UNVERIFIED)

The dispatch mechanism is `description`-driven on all three vendors, so the
Cursor behavioral pass is strong evidence but each host should be smoke-tested
on an **authenticated** host. Mark these `runtime-unverified` until run.

### Attempt in the cloud-agent environment (could not complete — no credentials)

Both smokes were attempted here and are blocked by the environment, not the code:

- **Codex:** no `codex` CLI on PATH and no OpenAI/Codex credentials; the
  codex-companion runtime returned "Codex CLI isn't available in this
  environment," so no Codex thread could start.
- **Claude Code:** `npx @anthropic-ai/claude-code@latest` installs (v2.1.268)
  but `claude -p` returns "Not logged in · Please run /login"; no
  `ANTHROPIC_API_KEY` is present, and `/login` is interactive.

To run them in a Cloud Agent, add the relevant credentials in the Secrets panel
(e.g. `ANTHROPIC_API_KEY`, and Codex/OpenAI auth), or run the commands below on a
logged-in local host.

Prereqs:
- Codex: set host-local `[agents]` in `.codex/config.toml` (git-ignored):
  `enabled = true`, `max_concurrent_threads_per_session = 3`,
  `default_subagent_model = <cheap-end slug>`. Do **not** add `max_depth`
  (not in the current schema). Model selection is per-dispatch spectrum; only
  the always-floor `reviewer` keeps a native `xhigh` pin, sometimes-floor roles
  reach the floor via a strongest session or a host-local pin
  (adapters/codex.md §Model selection). Verify your CLI version can load custom
  agents (upstream #26868 / #27061).
- Claude: `claude` logged in; `.claude/agents/*.md` discovered; `reviewer`
  resolves to Opus (native floor default); other roles inherit and Main raises
  floor dispatches to `opus`+`xhigh` per dispatch.

Exact commands (run from repo root):

```bash
# Claude Code
claude -p "$(cat docs/validation/extensions/fixed-agent-orchestration/mock-flow-prompt.txt)"

# Codex
codex exec "$(cat docs/validation/extensions/fixed-agent-orchestration/mock-flow-prompt.txt)"
```

Mock-flow prompt (canonical text in `mock-flow-prompt.txt`, same as Cursor Test A):

1. Spawn `experiment-agent` (EXP-MOCK-01): run the 2-Gaussian 1-feature
   threshold on `examples/mock-flow-detection/`; write
   `…/.research/work/EXP-MOCK-01-run.md`; return only the Two-tier summary.
2. In parallel, spawn `literature-scout`: no vault → return `NEEDS_REFRESH`;
   write `…/.research/work/LIT-MOCK-01.md`.
3. Spawn `result-analyst` (compact): open `EXP-MOCK-01-run.md` **from disk**;
   write `…/.research/work/EXP-MOCK-01-analysis.md`; return only the summary.
4. Assert (same table as §A): native spawn, sub-context depth, disk-pointer
   return, parallel overlap, no canonical writes, no auto-reviewer.

Pass criteria are identical to §A. Record host + CLI version when run.
