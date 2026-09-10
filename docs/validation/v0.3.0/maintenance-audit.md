# V0.3.0 Maintenance Audit

Date: 2026-09-11  
Mode: `framework-maintenance` **standard**  
Workspace: `/tmp/rss-framework-scientific-continuity`  
Branch: `framework-scientific-continuity` (release commit on top of `04d0b70`)  
Executor: checklist in `.agents/skills/framework-maintenance/SKILL.md` before tagging `v0.3.0`.

This file is release evidence. It is not an independent live Gate.

## Scope

Tree at this release (template `.research/` only). Contrasted against Protocol owners (`state-files.md`, `experiment-record.md`, `story-loop.md`, `git-linking.md`, `.agents/subagents/reviewer.md`) and Layer 2 under `.agents/references/research-intelligence/`.

Not executed: harness CLI smoke, Method-First Cases 1–9 live Gate, independent Codex / Claude Gate.

## Framework Maintenance Audit — 2026-09-11

### Checklist

| # | Check | Status | Notes |
|---|-------|--------|-------|
| 1 | Duplicate rules | **PASS** | Outcome definition table remains in `experiment-record.md`. `EXPERIMENTS.template.md` only has an Index column and `{{per experiment-record.md}}`. Quantity / manipulation / consult / resume rules have one owner each; Skills cite. |
| 2 | Skill overlap | **PASS** | 15 Skills; `monitor-experiment` is W2 wait after launch, not a Stage; `framework-extension` is maintainer-only. `research-loop` still schedules only. |
| 3 | AGENTS size | **NOTE** | `wc -l AGENTS.md` = **215** vs §尺寸建议 < 150. Pre-existing on `origin/master` (214) from standing orders + model class + Workflow table. Not introduced by scientific-continuity. Splitting would hide Start Here. Not a release blocker. |
| 4 | STATE size | **PASS** | Template `.research/STATE.md` = **33** lines; `Story Status: NOT_INITIALIZED`; no run log. |
| 5 | STORY as log | **PASS** | Template STORY is six segments, no EXP numbers. |
| 6 | DISCOVERY vs EXPERIMENTS | **PASS** | Template DISCOVERY empty buckets; EXPERIMENTS empty Index. Protocol Outcome tokens unchanged. |
| 7 | Adapter drift | **PASS** | `CLAUDE.md` / `adapters/` remain host-invocation. `.claude/skills/` and `.cursor/skills/` are **15** symlinks to `.agents/skills/<name>`. Five host agent wrappers each; no new role. |
| 8 | Unnecessary scripts | **PASS** | No framework `.py` / `.sh` / `.js` / `.ts` / Makefile outside `docs/` / `examples/` / `.git`. |
| 9 | Cross-file terminology | **PASS** | Eight canonical files. Status vs Outcome unchanged. No new enum. |
| 10 | Skill count is **15** | **PASS** | `.agents/skills/` has exactly 15 directories including `workspace-setup`, `framework-extension`, and `monitor-experiment`. No 16th Skill. |
| 11 | Intelligence refs are Layer 2 | **PASS** | Six RI files. New scientific-continuity prose does not redefine Outcome / Verdict / Story six segments. |
| 12 | Gates selective | **PASS** | Idea-gate / Evidence-gate / deep literature remain non-default. Ordinary exploratory still starts small. |
| 13 | Scorer independence | **NOTE** | No skill-evolution candidate in this tag. N/A. |
| 14 | Meta-rule duplication | **PASS** | Continuity packages cite owners rather than pasting seven packages into every file. |

### Also verify

| Item | Status | Evidence |
|------|--------|----------|
| `.agents/skills/` unique canonical Skill root | **PASS** | Host skill dirs are symlinks. |
| Subagent handoff uses `.research/work/` | **PASS** | `subagent-handoff.md` unchanged path contract; consults stay one decision. |
| Template Project Status UNINITIALIZED | **PASS** | `.research/PROJECT.md`. |
| No extra canonical `.research` file | **PASS** | Eight `*.md` only. |
| No framework runtime scripts | **PASS** | See #8. |
| Historical tags unmoved | **PASS** | This audit does not retag `v0.1`–`v0.2.2`. |

### Findings

- [MINOR] `AGENTS.md` is 215 lines (target < 150) — owner: `AGENTS.md` — suggested fix: later extract Workflow 表 / 模型分档 to a cited reference without losing Start Here. **Not fixed in this tag.**

No **CRITICAL**. No **MAJOR**.

### Passed checks

- Frozen counts 8 / 1 / 5 / 15 / 6 / 0 match the tree.
- Host skill directories are symlinks, not forks.
- Template `.research/` is UNINITIALIZED.
- Scientific-continuity validation recorded as Agent walk, not claimed as live Gate.

### Release recommendation

**freeze**
