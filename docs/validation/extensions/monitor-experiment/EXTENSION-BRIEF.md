# Extension Brief — monitor-experiment

## Request

**User need:** After an experiment is launched, the Agent should monitor
progress with silent waits and minimum probes instead of continuously
reasoning (token waste).

**Target users/projects:** Any research-story-speaker workspace with
local or remote long-running jobs.

**Why existing behavior is insufficient:** `experiment-execution` launches
and records Runs, then currently hands off as if results were already
there. There is no wait/probe contract. Agents fill the gap with thinking,
new audits, extra EXPs, or per-poll subagents.

## Classification

**Primary type:** existing-Skill mode (handoff from execution) + new core
Skill for the wait/probe owner

**Deployment scope:** framework core (inner-loop W2 wait; not a new Stage)

**Extension-ladder rung:** 7 (new Skill) with a rung-4 handoff in
`experiment-execution`

**Decision:**  
`CORE_CANDIDATE` (user-authorized core add; wait operator is broadly reusable)

**Why lower rungs are insufficient:**

- Rung 1–2: RESOURCES already names SSH/local; that does not stop thinking.
- Rung 3: a reference paragraph would not be discovered as a trigger.
- Rung 4 alone: stuffing the adaptive-wait contract into
  `experiment-execution` would break its compact stop-line.
- Rung 6 optional pack: every long GPU run needs this, not a niche provider.
- Did not import ARIS control plane (orchestrator, `.aris/`, claim gates).

## Existing Owner and Overlap

**Closest existing owner:** `experiment-execution` (launch); `result-analysis`
(after terminal artifacts).

**Overlap found:** execution already says remote runs may stay `running`
until synced — no wait protocol.

**What will be reused:** EXP-ID, RESOURCES Compute Access, Runs fields,
W2 TEST Position, support/bounded-debug return to the same EXP,
`result-analysis` for Outcome.

**What will not be duplicated:** ARIS agent-team, EXPERIMENT_PLAN/LOG,
Feishu, W&B-as-required, auto-review-loop, CLAIM_GATE, new STATE fields.

## Capability Contract

**Trigger:** launched job still alive; Status `running`; user asks 监控/进度.

**Default owner:** Main Agent. Do not dispatch a subagent to monitor.

**Inputs:** target EXP section, RESOURCES Compute, existing monitor work file.

**Outputs:** overwritten `.research/work/<EXP-ID>-monitor.md`; class;
handoff to analysis or support.

**Read scope:** target EXP + RESOURCES + one work file.

**Write scope:** monitor work file; mechanical Runs note only on class change.

**External dependencies:** host shell / SSH as already in RESOURCES. None new.

**Permissions:** same as execution (read logs, list jobs). No secrets.

**Fallback/degraded behavior:** persist `next_poll_at` and yield if the
host cannot keep a foreground wait.

**Failure semantics:** `failed_or_suspect` → same-EXP support, not new EXP-ID.

**Provenance:** localized wait/probe operator from ARIS
`Documents/ARIS/.agents/skills/monitor-experiment/SKILL.md`.

**Observability:** one progress line when progress changed; no poll diary
in STATE.

**Uninstall/rollback:** delete Skill + Claude symlink; revert execution /
AGENTS / README cross-links; frozen count 15→14. No `.research/` migration.

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

**Files added:** `.agents/skills/monitor-experiment/SKILL.md`; Claude symlink;
this brief; spec cases.

**Files modified:** `experiment-execution`, `result-analysis`,
`workspace-resume`, `research-loop`, `story-loop` (owner table only),
`experiment-agent`, `research-lead`, `AGENTS.md`, `README.md`,
`framework-maintenance` count, `banner.svg`, thin adapter notes.

**Files intentionally untouched:** Protocol Outcome/Status tables; W0–W5
enum; `.research/` templates beyond existing Runs; ARIS files.

**Rules linked rather than copied:** story-loop Parallel Drift; execution
support/debug; result-analysis Outcome rule.

**New terms:** monitor class labels (Skill-internal; not STATE/Outcome).

**New dependencies:** none.

## Complexity Budget

**Permanent files added:** 1 Skill directory + extension brief/cases.

**Core files touched:** routing + execution handoff only.

**Default-context impact:** AGENTS routing row; Skill count 15.

**Ordinary-path impact:** after async launch, wait instead of think; sync
smoke unchanged (skip).

**Network/tool impact:** existing SSH/local probes only.

**External permissions:** none new.

**New failure modes:** false terminal from file existence — Skill forbids it.

**Maintenance owner:** `experiment-execution` + this Skill; audit via
`framework-maintenance`.

**Complexity avoided or removed:** ARIS control plane not imported.

## Validation

**Expected-use case:** long job launched → silent profile wait → terminal
→ `result-analysis`.

**Must-not-trigger protection case:** epoch progress ≠ Outcome; no Reviewer;
no new EXP; no per-poll subagent; sync smoke skips monitor.

**Dependency-missing case:** unknown probe → reconstruct once or ask once.

**Partial-failure case:** crash mid-run → support on same EXP.

**Duplicate/retry case:** overwrite one work file.

**Held-out case:** n/a (instruction-only).

**Cross-Harness case:** wait how-to in adapters; Skill stays tool-agnostic.

**Security/privacy checks:** no credentials; RESOURCES alias only.

**Rollback/removal test:** delete Skill; routing lines revert; count 14.

## Source and License

**Upstream source:** `/Users/herxanadu/Documents/ARIS/.agents/skills/monitor-experiment/`

**Specific file/Skill:** `SKILL.md` Adaptive Running Monitor Mode + wait contract

**License:** project-local ARIS skill; concept adapted, not copied wholesale

**Concept adapted:** silent adaptive wait, minimum probe, no per-poll agent,
engineering success ≠ scientific success

**Copied text:** `no`

**Local wording/structure:** RSS EXP-ID, RESOURCES, W2, result-analysis

## Release Class

`core candidate` (user-authorized; spec-level cases, not a live Gate)

## Handoff

**framework-maintenance mode:**  
`standard`

**Next action:** freeze count 15; do not add a 16th Skill for doctor/modes.
