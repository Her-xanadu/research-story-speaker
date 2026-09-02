# Harness Adapters

Thin host-specific notes. Canonical science logic lives in `AGENTS.md`, `.agents/`, and `.research/`. Adapters answer only how this host finds the workspace, loads Skills, invokes Subagents/Reviewers, and exposes MCP.

Last tested: 2026-09-02 (UNINITIALIZED re-test in V0.1.1 Wave C). Evidence: [`docs/validation/harness-smoke/`](../docs/validation/harness-smoke/).

Cold-start read order: `AGENTS.md` Start Here. Adapters do not answer research questions.

## Mapping

| Harness | Entry | Skills | Subagent | Reviewer | MCP | Last tested | Evidence |
|---------|-------|--------|----------|----------|-----|-------------|---------|
| Codex | `AGENTS.md` (auto) | `.agents/skills/` | Codex Task + `.agents/subagents/` | Fresh Task + `reviewer.md` | Plugin MCP | 2026-09-02 | [codex.md](../docs/validation/harness-smoke/codex.md) |
| Claude Code | `CLAUDE.md` → `AGENTS.md` | `.agents/skills/` via `.claude/skills/<name>` symlink | `.agents/subagents/` | New session + `reviewer.md` | `~/.claude.json` | 2026-09-02 | [claude-code.md](../docs/validation/harness-smoke/claude-code.md) |
| Cursor | `AGENTS.md` / rules | `.agents/skills/` | Task / subagents | Separate chat | Cursor MCP | 2026-09-02 | [cursor.md](../docs/validation/harness-smoke/cursor.md) |
| DeepSeek Harness | `AGENTS.md` | `.agents/skills/` | DSH routing + `.agents/subagents/` | Fresh session + `reviewer.md` | DSH plugins | 2026-09-02 | [dsh.md](../docs/validation/harness-smoke/dsh.md) |
| OpenCode | `AGENTS.md` | `.agents/skills/` | `.agents/subagents/` | Fresh context | OpenCode MCP | not tested | documentation-only |

## Structural limitations

- **OpenCode**: CLI not installed on the lead host; adapter is documentation-only.
- **Claude Code**: auto-reads only `CLAUDE.md` and `.claude/skills/`; does not auto-read `AGENTS.md` or `.agents/skills/`. `CLAUDE.md` is the boot pointer; `.claude/skills/<name>` is a directory symlink to `../../.agents/skills/<name>`.
- **Cursor**: headless `cursor-agent` needs `--trust`; IDE Agent panel does not.
- **Skills**: not synced to global dirs — workspace `.agents/skills/` is canonical.
- **Framework**: instruction-only; no Python/Shell services in workspace.
- **Git**: if `~/.gitignore` contains `/*`, child repos under home may need `git -c core.excludesfile=/dev/null` for first commit.

CLI commands, host paths, versions, stderr, and smoke checklists live in [`docs/validation/harness-smoke/README.md`](../docs/validation/harness-smoke/README.md).

## Per-host files

- [codex.md](codex.md)
- [claude-code.md](claude-code.md)
- [cursor.md](cursor.md)
- [opencode.md](opencode.md)
- [deepseek-harness.md](deepseek-harness.md)
