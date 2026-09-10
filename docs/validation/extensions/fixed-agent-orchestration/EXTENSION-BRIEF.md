# Extension Brief — fixed-agent-orchestration (Default Role Dispatch)

## Request

**User need:** Make the multi-agent structure the *default execution model*, not an
occasional helper. Main should coordinate, integrate decisions, and own the eight
canonical files; the five named subagents should own complete, bounded work
segments (design, literature, implement+run, interpret, review); stages hand off
by predefined rules instead of Main re-deciding "what next / whom to call" each turn.

**Target users/projects:** Every workspace using research-story-speaker across
Codex / Claude Code / Cursor; primary harness is Codex.

**Why existing behavior is insufficient:** The role files and native configs
already exist, but the current rules pull work back to Main:
- `AGENTS.md` §Autonomy + §Subagents: "Main 自行决定派不派"; `result-analyst` only
  on high-stakes; ordinary work stays Main-side.
- `AGENTS.md` §Main三条常驻规则 rule 1 + `monitor-experiment`: after launch the
  execution agent stops and Main runs `sleep; probe` in the main conversation,
  pulling logs/paths/failure-triage back into Main.
- All three `experiment-agent` wrappers say "Write only `.research/work/`", which
  contradicts the canonical role that authorizes linked-code-repo writes.
Net effect: the default path is Main-designs, Main-runs, Main-reads, Main-updates.

## Classification

**Primary type:** Protocol/core change (routing + autonomy) plus role/prompt/adapter edits.

**Deployment scope:** framework core (affects every workspace).

**Extension-ladder rung:** CORE_CANDIDATE (top rung).

**Decision:**
`CORE_CANDIDATE`

**Why lower rungs are insufficient:** The user is intentionally changing the
default execution structure and the autonomy rule in `AGENTS.md`. A local
customization, optional Skill, or prompt-only enrichment cannot change what the
core does by default. This is explicitly user-authorized as a core change; it is
routed through this Brief + `framework-maintenance` audit rather than silently merged.

## Existing Owner and Overlap

**Closest existing owner:**
- Workflow-stage → owner mapping: `.agents/references/story-loop.md` §阶段职责.
- Routing table + autonomy: `AGENTS.md` §Workflow / §Autonomy / §Subagents.
- Dispatch/return contract: `.agents/prompts/subagent-handoff.md`.
- Role contracts: `.agents/subagents/<role>.md`.
- Run-wait semantics: `.agents/skills/monitor-experiment/SKILL.md`.

**Overlap found:** `AGENTS.md` §Workflow table and `story-loop.md` §阶段职责 both
describe stage→owner. To avoid duplicated workflow (framework-maintenance checks
#1/#2), the authoritative dispatch matrix is owned by `story-loop.md`; `AGENTS.md`
keeps a compact pointer + the autonomy default and cites it.

**What will be reused:** the five existing roles, the eight canonical files, the
single `research-loop`, the existing handoff block, `monitor-experiment` as the
minimal-probe capability, all four native harness config layouts.

**What will not be duplicated:** no new orchestrator, no new Skill (count stays
15), no ninth state file, no runtime script. The dispatch matrix lives in exactly
one file.

## Capability Contract

**Trigger:** every Main turn in an `ACTIVE` project reads `STATE.md` Workflow
Position and applies the default dispatch matrix.

**Inputs:** current Workflow Position, Recommended Next Action, Story gap, reserved EXP-ID.

**Outputs:** a filled handoff block to the mapped role; after return, a Main
integration + canonical-state update.

**Read scope:** Main reads the working set + subagent decision summaries + (on
demand) work reports. Subagents read only their role's declared files.

**Write scope:** Main writes the eight canonical files. Subagents write
`.research/work/` (and, for `experiment-agent` with a reserved EXP-ID, authorized
code/result paths per `RESOURCES.md` + `git-linking.md`); `reviewer` writes
`.research/reviews/<EXP-ID>/`.

**External dependencies:** none new. Host subagent mechanism only.

**Permissions:** unchanged canonical-write boundary; the only fix is aligning the
`experiment-agent` wrappers with the canonical code-write authorization.

**Fallback/degraded behavior:** if the harness has no subagent mechanism, Main
runs the role's task prompt in a fresh session/context and records the same
handoff/return; determinism degrades to "strong guidance."

**Failure semantics:** a missing work-file body ⇒ consult incomplete (unchanged);
a subagent that cannot name "Decision This Task Can Change" is not dispatched.

**Provenance:** dispatch matrix + role edits are canonical framework content;
records/audits live under `docs/validation/`.

**Observability:** native role/thread rows in the host UI; Main's turn shows
question → role → returned summary → decision → state update → next dispatch.

**Uninstall/rollback:** revert this PR; the previous Main-heavy defaults return.
No data migration (no state schema change).

## Architecture Fit

- [x] Story-driven loop preserved
- [x] Eight canonical files preserved
- [x] Main canonical writer preserved
- [x] Single `research-loop` preserved
- [x] Prompt/file/config-first preserved
- [x] Canonical-first / adapters-thin preserved
- [x] Progressive disclosure preserved
- [x] No second truth or state source

## Minimal File Plan

**Files added:**
- `docs/validation/extensions/fixed-agent-orchestration/EXTENSION-BRIEF.md`
- `docs/validation/extensions/fixed-agent-orchestration/VALIDATION.md` (behavioral evidence + static cross-check + Codex/Claude runnable checklist)

> A separate `dispatch-matrix.md` reference copy was **intentionally not created**:
> it would be a second stage→owner table and fail the framework-maintenance
> no-duplication check. The dispatch matrix lives only in `story-loop.md` §阶段职责.

**Files modified:**
- `AGENTS.md` (§Autonomy, §Subagents, §模型分档, §Main三条常驻规则 rule 1, §Workflow pointer)
- `.agents/references/story-loop.md` (§阶段职责 → authoritative dispatch matrix; run ownership)
- `.agents/subagents/{research-lead,literature-scout,experiment-agent,result-analyst,reviewer}.md`
- `.agents/skills/experiment-execution/SKILL.md`, `.agents/skills/monitor-experiment/SKILL.md`
- `.agents/prompts/subagent-handoff.md`
- `.codex/agents/*.toml`, `.claude/agents/*.md`, `.cursor/agents/*.md`
- `adapters/codex.md`, `adapters/claude-code.md`, `adapters/cursor.md`

**Files intentionally untouched:** the eight `.research/` files, `README.md` frozen
counts (still 15 skills / 5 subagents / 0 scripts), the scientific-continuity edits.

**Rules linked rather than copied:** dispatch matrix owned by `story-loop.md`;
Verdict vocabulary stays in `reviewer.md`; Outcome stays in `experiment-record.md`.

**New terms:** "run ownership", "decision summary" (return tier), "default dispatch matrix".

**New dependencies:** none.

## Complexity Budget

**Permanent files added:** 2 (both under `docs/validation/`, no core additions).

**Core files touched:** AGENTS.md, story-loop.md, 5 role files, 2 skills, handoff,
12 harness configs, 3 adapters.

**Default-context impact:** neutral to positive — Main context stays cleaner because
high-output work moves into sub-contexts.

**Ordinary-path impact:** ordinary results now default to a compact `result-analyst`
task instead of Main; ordinary mechanical smoke still returns pass/fail without
forcing analyst/reviewer.

**Network/tool impact:** none new.

**External permissions:** none new.

**New failure modes:** execution agent may not outlive a long run (single-conversation
harness) → mitigated by persistent-job handoff; over-dispatch of trivial tasks →
mitigated by "mechanical smoke returns pass/fail, no forced analyst" rule.

**Maintenance owner:** framework maintainer via `framework-maintenance`.

**Complexity avoided or removed:** removes the wrapper-vs-canonical permission
contradiction; de-duplicates the two stage→owner tables.

## Validation

**Expected-use case:** Main → `experiment-agent` (implement+run) → decision summary
→ `result-analyst` (interpret) → Main writes mock state, on the mock sample.

**Must-not-trigger protection case:** ordinary mechanical smoke (no new scientific
question) does not force `result-analyst` + `reviewer`; a numbered clear Next does
not spawn `research-lead`.

**Dependency-missing case:** no subagent mechanism → Main runs role prompt in fresh
session; handoff/return still recorded.

**Partial-failure case:** subagent returns no work-file body → consult incomplete;
Main does not invent progress.

**Duplicate/retry case:** recovery after a new Main session resumes the persisted
job by id/paths; does not relaunch.

**Held-out case:** a stage not in the matrix (e.g., W5 handoff) stays Main-owned.

**Cross-Harness case:** Cursor validated behaviorally here; Codex/Claude static +
runnable checklist, marked runtime-unverified.

**Security/privacy checks:** no credentials added; `.codex/config.toml` stays
git-ignored (global `[agents]` documented in adapter only).

**Rollback/removal test:** revert PR restores prior defaults; no state migration.

## Source and License

**Upstream source:** official docs — Claude Code Subagents, Cursor Subagents, Codex Subagents.

**Specific file/Skill:** n/a (no external Skill localized).

**License:** n/a.

**Concept adapted:** native subagent config conventions per each vendor's docs.

**Copied text:** `no`

**Local wording/structure:** all wording original to this repo.

## Release Class

`core candidate`

## Handoff

**framework-maintenance mode:**
`standard` (+ `regression-eval` for dispatch behavior before/after)

**Next action:** implement the six modification groups on
`cursor/fixed-agent-orchestration-82c1`, validate on Cursor + static cross-check,
then run `framework-maintenance` and open the PR.

## framework-maintenance outcome (standard + regression-eval)

**Standard audit findings & resolution:**
- No-duplication (#1/#2): PASS — the stage→owner dispatch matrix has a single
  owner (`story-loop.md` §阶段职责); `AGENTS.md` §Workflow keeps a pointer only,
  and no `dispatch-matrix.md` copy was created.
- Adapter drift: PASS — all three adapters document their vendor's real
  model/effort/permission fields, shared-checkout/opt-in isolation (Cursor),
  host-local git-ignored `[agents]` (Codex), and one-level dispatch.
- Skill inventory: PASS — still **15** SKILL.md, mirrored in `.claude/skills/`
  and `.cursor/skills/`.
- Wrapper↔canonical consistency: FIXED — removed the "Write only `.research/work/`"
  contradiction in all three `experiment-agent` wrappers.
- Return-contract consistency: FIXED — `method-review.md`, `result-review.md`,
  and `next-research-move.md` prompts now defer to the Two-tier product instead
  of "return the same N sections."
- Canonical `.research/` untouched: PASS — repo-root `.research/` stays
  `UNINITIALIZED`; no live science committed.

**regression-eval (dispatch behavior before → after):**
- Before: default path was Main-designs / Main-runs / Main-reads / Main-updates;
  `result-analyst` only on high-stakes; execution agent stopped at launch and
  Main ran the wait.
- After (verified on the mock flow): W2 → `experiment-agent` holds the full run
  segment; W3 result → `result-analyst` by default (compact); high-output work
  stays in sub-contexts and Main receives only the Two-tier decision summary;
  independent tasks overlap; no subagent wrote canonical state; ordinary EXP did
  not auto-spawn a reviewer. Evidence: `VALIDATION.md` §A.

**Release class:** `core candidate` — behaviorally verified on Cursor;
Codex/Claude runtime-unverified pending an authenticated host smoke test.

## Pre-merge review fixes (post-acceptance)

Applied after the reviewer accepted the core design:
1. **Codex `max_depth` removed** — not in the current Codex `[agents]` schema;
   one-level dispatch is now enforced by instruction only.
2. **"Fix the concrete model" → one-time host model binding** — adapters now
   state honestly that Codex/Cursor strongest roles ship `inherit` and give a
   one-time per-install `model` binding step (Codex `model=` / Cursor
   `model: <slug>[effort=high]`); Claude ships concrete `model: opus`.
3. **Parallel code-writing `experiment-agent` isolation** — two code-writing
   `experiment-agent`s may run in parallel only in separate
   worktree/clone/working directories (own branch); otherwise serialize.
   Documented in AGENTS.md, story-loop.md, and all three adapters.

**Codex/Claude native smoke:** attempted in the cloud-agent environment and
blocked by missing credentials (no Codex CLI/creds; Claude CLI installs but not
logged in). Exact one-command reproductions and a canonical `mock-flow-prompt.txt`
are provided in `VALIDATION.md` §C for an authenticated host. Both remain
`RUNTIME-UNVERIFIED`; the Cursor pass is not extrapolated to the other two.
