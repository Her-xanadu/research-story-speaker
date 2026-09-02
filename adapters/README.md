# Harness Adapters

Thin host-specific notes. Canonical science logic lives in `AGENTS.md`, `.agents/`, and `.research/`. Adapters answer only how this host finds the workspace, loads Skills, invokes Subagents/Reviewers, and exposes MCP.

Last tested: 2026-09-03 (UNINITIALIZED cold-start on four harnesses). Evidence: [`docs/validation/harness-smoke/uninitialized-README.md`](../docs/validation/harness-smoke/uninitialized-README.md). V0.1 MOCK-root smoke remains under the same directory without the `uninitialized-` prefix.

Cold-start read order: `AGENTS.md` Start Here. Adapters do not answer research questions.

## Mapping

| Harness | Entry | Skills | Subagent | Reviewer | MCP | Last tested | Evidence |
|---------|-------|--------|----------|----------|-----|-------------|---------|
| Codex | `AGENTS.md` (auto) | `.agents/skills/` | Codex Task + `.agents/subagents/` | Fresh Task + `reviewer.md` | Plugin MCP | 2026-09-03 | [uninitialized-codex.md](../docs/validation/harness-smoke/uninitialized-codex.md) |
| Claude Code | `CLAUDE.md` → `AGENTS.md` | `.agents/skills/` via `.claude/skills/<name>` symlink | `.agents/subagents/` | New session + `reviewer.md` | `~/.claude.json` | 2026-09-03 | [uninitialized-claude-code.md](../docs/validation/harness-smoke/uninitialized-claude-code.md) |
| Cursor | `AGENTS.md` / rules | `.agents/skills/` | Task / subagents | Separate chat | Cursor MCP | 2026-09-03 | [uninitialized-cursor.md](../docs/validation/harness-smoke/uninitialized-cursor.md) |
| DeepSeek Harness | `AGENTS.md` | `.agents/skills/` | DSH routing + `.agents/subagents/` | Fresh session + `reviewer.md` | DSH plugins | 2026-09-03 | [uninitialized-dsh.md](../docs/validation/harness-smoke/uninitialized-dsh.md) |
| OpenCode | `AGENTS.md` | `.agents/skills/` | `.agents/subagents/` | Fresh context | OpenCode MCP | not tested | documentation-only |

## Structural limitations

- **OpenCode**: CLI not installed on the lead host; adapter is documentation-only.
- **Claude Code**: auto-reads only `CLAUDE.md` and `.claude/skills/`; does not auto-read `AGENTS.md` or `.agents/skills/`. `CLAUDE.md` is the boot pointer; `.claude/skills/<name>` is a directory symlink to `../../.agents/skills/<name>`.
- **Cursor**: headless `cursor-agent` needs `--trust`; IDE Agent panel does not.
- **Skills**: not synced to global dirs — workspace `.agents/skills/` is canonical.
- **Framework**: instruction-only; no Python/Shell services in workspace.
- **Git**: if `~/.gitignore` contains `/*`, child repos under home may need `git -c core.excludesfile=/dev/null` for first commit.
- **Codex sandbox**: `codex exec --sandbox workspace-write` cannot create `.git/index.lock`, so it cannot `git commit`. Workspace commits need `--dangerously-bypass-approvals-and-sandbox` (or equivalent full-access) or an outer process. Observed in C2/C4 (`docs/validation/handoff-tests/`).

CLI commands, host paths, versions, stderr, and smoke checklists live in [`docs/validation/harness-smoke/README.md`](../docs/validation/harness-smoke/README.md).

## Per-host files

- [codex.md](codex.md)
- [claude-code.md](claude-code.md)
- [cursor.md](cursor.md)
- [opencode.md](opencode.md)
- [deepseek-harness.md](deepseek-harness.md)
