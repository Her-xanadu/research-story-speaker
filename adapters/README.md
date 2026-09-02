# Harness Adapters

Story Research Workspace V0.1 — **thin** host-specific notes. Canonical science logic lives in `AGENTS.md`, `.agents/`, and `.research/`. Adapters answer only: how this host finds the workspace, loads Skills, invokes Subagents/Reviewers, and exposes MCP.

**Worker G cross-harness cold-start** (2025-09-02, host: lead integrator Mac): non-interactive CLI smoke from workspace root `/Users/herxanadu/Documents/story-research-workspace`, plus file-only bootstrap expectation from `AGENTS.md` read order.

## CLI availability (`which`)

```text
/Users/herxanadu/bin/codex
/Users/herxanadu/.local/bin/claude
/Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh
/Users/herxanadu/.local/bin/cursor-agent
opencode not found
```

## Cold-start checklist (7 items)

Each harness should recover these **only from workspace files** (`PROJECT → STORY → STATE`, then targeted `EXPERIMENTS` / `RESOURCES`):

| # | Item | Primary sources |
|---|------|-----------------|
| 1 | 识别项目 | `.research/PROJECT.md`, `AGENTS.md` |
| 2 | 识别 Story | `.research/STORY.md` |
| 3 | 识别当前 State | `.research/STATE.md` |
| 4 | 发现 Skills | `AGENTS.md` routing + `.agents/skills/*/SKILL.md` |
| 5 | 读取 Experiment EXP-001 | `.research/EXPERIMENTS.md` § EXP-001, `.research/reviews/EXP-001/` |
| 6 | 找到代码资源 | `.research/RESOURCES.md` → `../story-research-code` |
| 7 | 理解下一步 | `.research/STATE.md` Recommended Next Action |

**Expected answers (MOCK ground truth):**

- **项目**: Lightweight Flow-Feature Anomaly Detection (MOCK)
- **Story**: Three Flow Statistics May Suffice for Attack Detection
- **State**: EXP-001 closed; focus EXP-002 ablation / optional real CICIDS re-run; `IN_PROGRESS`
- **Skills**: 10 dirs under `.agents/skills/` (see matrix notes)
- **EXP-001**: 3-feature LR F1=1.0 vs IF F1=0.5455 on MOCK 50-row flows; commit `b0621e2`
- **代码**: `../story-research-code` (`flow-detector`)
- **下一步**: 设计 EXP-002；可选真实 CICIDS 子集复验

## Support matrix (cold-start × harness)

Legend: **Y** = recovered in CLI smoke; **Y\*** = file-only / documented (no CLI on host); **P** = partial / caveat.

| Harness | CLI | 1 项目 | 2 Story | 3 State | 4 Skills | 5 EXP-001 | 6 代码 | 7 下一步 | V0.1 |
|---------|-----|--------|---------|---------|----------|-----------|--------|----------|------|
| **Codex** | `codex exec` | Y | Y | Y | Y | Y | Y | Y | **verified** |
| **Claude Code** | `claude -p` | Y | Y | Y | Y | Y | Y | Y | **verified** |
| **DeepSeek Harness** | `dsh --profile headless` | Y | Y | Y | Y | Y | Y | Y | **verified** |
| **Cursor** | `cursor-agent -p --trust` | Y | Y | Y | Y | Y | Y | Y | **verified** |
| **OpenCode** | *(not installed)* | Y\* | Y\* | Y\* | Y\* | Y\* | Y\* | Y\* | **documented** |

### Entry files & skill roots

| Harness | Entry file | Skills root | Subagents | Reviewer | MCP |
|---------|------------|-------------|-----------|----------|-----|
| Codex | `AGENTS.md` (auto) | `.agents/skills/` | Codex Task + `.agents/subagents/` | Fresh Task + `reviewer.md` | Plugin MCP |
| Claude Code | `CLAUDE.md` → `AGENTS.md` | `.agents/skills/` (no `.claude/` mirror in repo) | `.agents/subagents/` | New session + `reviewer.md` | `~/.claude.json` |
| Cursor | `AGENTS.md` / rules | `.agents/skills/` | Task / subagents | Separate chat | Cursor MCP |
| DSH | `AGENTS.md` | `.agents/skills/` | DSH routing + `.agents/subagents/` | Fresh session + `reviewer.md` | DSH plugins |
| OpenCode | `AGENTS.md` | `.agents/skills/` | `.agents/subagents/` | Fresh context | OpenCode MCP |

## Non-interactive cold-start commands (smoke)

```bash
cd /Users/herxanadu/Documents/story-research-workspace

# Codex
codex exec "Follow AGENTS.md. workspace-resume from PROJECT+STORY+STATE only; list skills; summarize EXP-001 and RESOURCES; state next action."

# Claude Code
claude -p "Follow AGENTS.md cold-start; answer project, story, state, skills, EXP-001, code path, next action from workspace files only."

# DeepSeek Harness (one-shot)
dsh --profile headless "Read AGENTS.md + PROJECT+STORY+STATE; 7-line resume packet; no edits."

# Cursor Agent CLI
cursor-agent -p --trust --mode ask "AGENTS.md cold-start; 7-line resume from workspace files only."

# OpenCode (when installed)
# opencode  # interactive; CLI name varies by install — see adapters/opencode.md
```

## CLI smoke notes (this run)

| Harness | Result | Notes |
|---------|--------|-------|
| Codex | Pass | `codex exec` completed; read `workspace-resume` SKILL then PROJECT/STORY/STATE/EXPERIMENTS/RESOURCES; websocket `426` warning on local bridge (non-fatal). |
| Claude | Pass | `claude -p` returned full 7-item packet without `.claude/skills` symlinks. |
| DSH | Pass | `dsh --profile headless` one-shot exit 0. |
| Cursor | Pass | `cursor-agent -p` **requires** `--trust` (or `-f` / `--yolo`) for non-interactive use. |
| OpenCode | N/A | Binary missing on host. |

## Limitations (V0.1)

- **OpenCode**: not installed (`opencode` missing); adapter is documentation-only until CLI is on PATH.
- **Claude Code**: `.claude/skills/` → `.agents/skills/` symlinks added in Phase 10 for native discovery; `CLAUDE.md` still points to `AGENTS.md` as canonical guide.
- **Cursor**: workspace trust gate blocks headless `cursor-agent` without `--trust`; IDE Agent panel does not need this flag.
- **Codex**: `codex exec` may not load all MCP plugins; may consult `workspace-resume` before strict PROJECT-first order (still correct content).
- **DSH**: use `--profile headless` for non-interactive smoke; default `dsh` boots TUI/web profile.
- **Skills**: not synced to global dirs (`~/.cursor/skills-cursor/`, etc.) — workspace `.agents/skills/` is canonical.
- **Mode B code**: absolute paths in `RESOURCES.md` can go stale; prefer `../story-research-code` and `git-linking.md`.
- **Framework**: instruction-only; no Python/Shell services in workspace.
- **Git**: if `~/.gitignore` contains `/*`, child repos under home may need `git -c core.excludesfile=/dev/null` for first commit (observed on lead host).

## Canonical read order

```text
AGENTS.md
.research/PROJECT.md
.research/STORY.md
.research/STATE.md
→ DISCOVERY / EXPERIMENTS / LITERATURE / REVIEWS / RESOURCES as needed
```

## Per-host files

- [codex.md](codex.md)
- [claude-code.md](claude-code.md)
- [cursor.md](cursor.md)
- [opencode.md](opencode.md)
- [deepseek-harness.md](deepseek-harness.md)

## Adapter rules (§19)

Each adapter may **only** document host invocation. Do not duplicate experiment-record, story-loop, or state-files content — link to `.agents/references/`.
