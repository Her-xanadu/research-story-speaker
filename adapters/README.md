# Harness Adapters

Story Research Workspace V0.1 — **thin** host-specific notes. Canonical science logic lives in `AGENTS.md`, `.agents/`, and `.research/`. Adapters answer only: how this host finds the workspace, loads Skills, invokes Subagents/Reviewers, and exposes MCP.

**Date:** 2026-09-02. CLI 可用性来自本机 `command -v`。Harness smoke 产物见 `.research/work/framework-dev/harness-smoke/`（2026-09-02）。

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
| **Codex** | 已安装 (`/Users/herxanadu/bin/codex`) | 有（2026-09-02，退出码 0，七项 Y 数 6/7） | `.research/work/framework-dev/harness-smoke/codex.md` |
| **Claude Code** | 已安装 (`/Users/herxanadu/.local/bin/claude`) | 有（2026-09-02，退出码 0，七项 Y 数 7/7） | `.research/work/framework-dev/harness-smoke/claude-code.md` |
| **DeepSeek Harness** | 已安装 (`/Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh`) | 有（2026-09-02，退出码 0，七项 Y 数 7/7） | `.research/work/framework-dev/harness-smoke/dsh.md` |
| **Cursor** | 已安装 (`/Users/herxanadu/.local/bin/cursor-agent`) | 有（2026-09-02，退出码 0，七项 Y 数 7/7） | `.research/work/framework-dev/harness-smoke/cursor.md` |
| **OpenCode** | 未安装 | 无 | 未测 |

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

# Codex (`codex exec --help` 无 --ask-for-approval；用 --sandbox read-only)
codex exec --sandbox read-only --color never --ephemeral -C /Users/herxanadu/Documents/story-research-workspace "Follow AGENTS.md. workspace-resume from PROJECT+STORY+STATE only; list skills; summarize EXP-001 and RESOURCES; state next action."

# Claude Code (`--allowedTools` 为 variadic，prompt 须在 -p 之后或 -- 之后)
claude --permission-mode plan --output-format text --allowedTools Read,Glob,Grep -p -- "Follow AGENTS.md cold-start; answer project, story, state, skills, EXP-001, code path, next action from workspace files only."

# DeepSeek Harness (one-shot; headless --help 无只读旗标)
dsh --profile headless "Read AGENTS.md + PROJECT+STORY+STATE; 7-line resume packet; no edits."

# Cursor Agent CLI
cursor-agent -p --trust --mode ask --workspace /Users/herxanadu/Documents/story-research-workspace "AGENTS.md cold-start; 7-line resume from workspace files only."

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
- **Codex**: `codex exec --help` 不含 `-a/--ask-for-approval`（该旗标只在交互式 `codex`）；`--sandbox read-only` 时日志为 `approval: never`。2026-09-02 smoke 另见 chronicle 未稳定特性警告，以及本机 `ws://127.0.0.1:10100/v1/responses` HTTP 426（未阻断退出码 0）。
- **Claude Code**: `--allowedTools` 为 variadic（`<tools...>`）；prompt 须紧随 `-p` 或放在 `--` 之后，否则会被当成 tool 名，`--print` 报缺少 input。
- **DSH**: `dsh --profile headless --help` 仅有 `-h`，无只读/no-edit 旗标；只读靠 prompt 约束，须用 git porcelain 复核。
- **Cursor**: `-p/--print` 本身「Has access to all tools, including write and shell」；只读必须叠加 `--mode ask` 或 `--mode plan`。

## Per-host files

- [codex.md](codex.md)
- [claude-code.md](claude-code.md)
- [cursor.md](cursor.md)
- [opencode.md](opencode.md)
- [deepseek-harness.md](deepseek-harness.md)

## Adapter rules (§19)

Each adapter may **only** document host invocation. Do not duplicate experiment-record, story-loop, or state-files content — link to `.agents/references/`.
