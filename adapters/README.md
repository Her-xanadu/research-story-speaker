# Harness Adapters

Story Research Workspace V0.1 — **thin** host-specific notes. Canonical science logic lives in `AGENTS.md`, `.agents/`, and `.research/`. Adapters answer only: how this host finds the workspace, loads Skills, invokes Subagents/Reviewers, and exposes MCP.

**Date:** 2026-09-02. CLI 可用性来自本机 `command -v`。Harness smoke 产物待修复轮后重跑。

冷启动读序见 `AGENTS.md` Start Here。Adapter 不回答科研问题、不提供实验数字。

## CLI availability (`command -v`)

```text
/Users/herxanadu/bin/codex
/Users/herxanadu/.local/bin/claude
/Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh
/Users/herxanadu/.local/bin/cursor-agent
opencode not found
```

## Support matrix

列：已安装（`command -v` 结果）/ 本机 smoke 是否有落盘产物 / 产物路径。

| Harness | 已安装 (`command -v`) | 本机 smoke 是否有落盘产物 | 产物路径 |
|---------|------------------------|---------------------------|----------|
| **Codex** | 已安装 (`/Users/herxanadu/bin/codex`) | 无 | smoke 产物：无（待修复轮后重跑，存放 .research/work/framework-dev/harness-smoke/） |
| **Claude Code** | 已安装 (`/Users/herxanadu/.local/bin/claude`) | 无 | smoke 产物：无（待修复轮后重跑，存放 .research/work/framework-dev/harness-smoke/） |
| **DeepSeek Harness** | 已安装 (`/Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh`) | 无 | smoke 产物：无（待修复轮后重跑，存放 .research/work/framework-dev/harness-smoke/） |
| **Cursor** | 已安装 (`/Users/herxanadu/.local/bin/cursor-agent`) | 无 | smoke 产物：无（待修复轮后重跑，存放 .research/work/framework-dev/harness-smoke/） |
| **OpenCode** | 未安装 | 无 | smoke 产物：无（待修复轮后重跑，存放 .research/work/framework-dev/harness-smoke/） |

### Entry files & skill roots

canonical `.agents/skills/`；Claude Code 另经 `.claude/skills/<name>` 目录 symlink（相对路径 `../../.agents/skills/<name>`）访问。

| Harness | Entry file | Skills root | Subagents | Reviewer | MCP |
|---------|------------|-------------|-----------|----------|-----|
| Codex | `AGENTS.md` (auto) | `.agents/skills/` | Codex Task + `.agents/subagents/` | Fresh Task + `reviewer.md` | Plugin MCP |
| Claude Code | `CLAUDE.md` → `AGENTS.md` | `.agents/skills/` + `.claude/skills/<name>` symlink | `.agents/subagents/` | New session + `reviewer.md` | `~/.claude.json` |
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

## Limitations (V0.1)

- **OpenCode**: not installed (`opencode` missing); adapter is documentation-only until CLI is on PATH.
- **Claude Code**: 只自动读 `CLAUDE.md` 与 `.claude/skills/`，不自动读 `AGENTS.md` 与 `.agents/skills/`。因此 `CLAUDE.md` 是 boot 指针；`.claude/skills/<name>` 为指向 `../../.agents/skills/<name>` 的目录 symlink。
- **Cursor**: workspace trust gate blocks headless `cursor-agent` without `--trust`; IDE Agent panel does not need this flag.
- **Codex**: `codex exec` may not load all MCP plugins; may consult `workspace-resume` before strict PROJECT-first order (still correct content).
- **DSH**: use `--profile headless` for non-interactive smoke; default `dsh` boots TUI/web profile.
- **Skills**: not synced to global dirs (`~/.cursor/skills-cursor/`, etc.) — workspace `.agents/skills/` is canonical.
- **Mode B code**: prefer `../story-research-code` and `git-linking.md`; absolute path only in RESOURCES Last known local location.
- **Framework**: instruction-only; no Python/Shell services in workspace.
- **Git**: if `~/.gitignore` contains `/*`, child repos under home may need `git -c core.excludesfile=/dev/null` for first commit (observed on lead host).

## Per-host files

- [codex.md](codex.md)
- [claude-code.md](claude-code.md)
- [cursor.md](cursor.md)
- [opencode.md](opencode.md)
- [deepseek-harness.md](deepseek-harness.md)

## Adapter rules (§19)

Each adapter may **only** document host invocation. Do not duplicate experiment-record, story-loop, or state-files content — link to `.agents/references/`.
