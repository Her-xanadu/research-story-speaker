# Harness Adapters

Thin host-specific notes. Canonical science logic lives in `AGENTS.md`, `.agents/`, and `.research/`. Adapters answer only how this host finds the workspace, loads Skills, invokes Subagents/Reviewers, and exposes MCP.

One install: clone this workspace. Codex, Claude Code, and Cursor configs all live in-tree. Other hosts are DIY.

Last tested: 2026-09-03 (UNINITIALIZED cold-start on four harnesses). Evidence: [`docs/validation/harness-smoke/uninitialized-README.md`](../docs/validation/harness-smoke/uninitialized-README.md). V0.1 MOCK-root smoke remains under the same directory without the `uninitialized-` prefix.

Cold-start read order: `AGENTS.md` Start Here. **UNINITIALIZED** projects run
`workspace-setup` first (compute + code Git layout → `.research/RESOURCES.md`),
then `workspace-resume`. Adapters do not answer research questions.

Model class (`workhorse` vs `strongest`) is defined in `AGENTS.md`. Main decides
whether to spawn; the host config decides **which named role** is spawned.

## Official (same clone)

| Harness | Entry | Skills | Who gets spawned | Reviewer | MCP | Last tested | Evidence |
|---------|-------|--------|------------------|----------|-----|-------------|---------|
| Codex | `AGENTS.md` (auto) | `.agents/skills/` | `.codex/agents/<role>.toml` (`description` routes) | named `reviewer` + `xhigh` | Plugin MCP | 2026-09-03 | [uninitialized-codex.md](../docs/validation/harness-smoke/uninitialized-codex.md) |
| Claude Code | `CLAUDE.md` → `AGENTS.md` | `.claude/skills/<name>` symlink | `.claude/agents/<role>.md` | named `reviewer` (`opus`) | `~/.claude.json` | 2026-09-03 | [uninitialized-claude-code.md](../docs/validation/harness-smoke/uninitialized-claude-code.md) |
| Cursor | `AGENTS.md` | `.agents/skills/` + `.cursor/skills/<name>` symlink | Task `subagent_type=<role>` from `.cursor/agents/<role>.md` | named `reviewer` | Cursor MCP | 2026-09-03 | [uninitialized-cursor.md](../docs/validation/harness-smoke/uninitialized-cursor.md) |

Do **not** spawn Codex/Cursor built-in generic `worker` / `explorer` / `explore` / `generalPurpose` as a research role.

## DIY (not an official install path)

| Harness | Entry | Skills | Subagent | Reviewer | MCP | Last tested | Evidence |
|---------|-------|--------|----------|----------|-----|-------------|---------|
| DeepSeek Harness | `AGENTS.md` | `.agents/skills/` | Map to `.agents/subagents/` | Fresh session + `reviewer.md` | DSH plugins | 2026-09-03 | [uninitialized-dsh.md](../docs/validation/harness-smoke/uninitialized-dsh.md) |
| OpenCode | `AGENTS.md` | `.agents/skills/` | Map to `.agents/subagents/` | Fresh context | OpenCode MCP | not tested | documentation-only |

## Structural limitations

- **OpenCode**: CLI not installed on the lead host; adapter is documentation-only.
- **Claude Code**: auto-reads only `CLAUDE.md` and `.claude/skills/` (and `.claude/agents/`); does not auto-read `AGENTS.md` or `.agents/skills/`. `CLAUDE.md` is the boot pointer.
- **Cursor**: headless `cursor-agent` needs `--trust`; IDE Agent panel does not. Built-in explore/generalPurpose are not the five research roles.
- **Skills**: not synced to global dirs — workspace `.agents/skills/` is canonical.
- **Framework**: instruction-only; no Python/Shell services in workspace.
- **Git**: if `~/.gitignore` contains `/*`, child repos under home may need `git -c core.excludesfile=/dev/null` for first commit.
- **Codex sandbox**: `codex exec --sandbox workspace-write` cannot create `.git/index.lock`, so it cannot `git commit`. Workspace commits need `--dangerously-bypass-approvals-and-sandbox` (or equivalent full-access) or an outer process. Observed in C2/C4 (`docs/validation/handoff-tests/`).
- **Claude `opus` on strongest roles**: if the account cannot spawn Opus, inherit the parent but do **not** pick Haiku / fast. Policy still lives in `AGENTS.md`.

CLI commands, host paths, versions, stderr, and smoke checklists live in [`docs/validation/harness-smoke/README.md`](../docs/validation/harness-smoke/README.md).

## Per-host files

- [codex.md](codex.md)
- [claude-code.md](claude-code.md)
- [cursor.md](cursor.md)
- [opencode.md](opencode.md) (DIY)
- [deepseek-harness.md](deepseek-harness.md) (DIY)
