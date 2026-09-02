# Harness Adapters

Story Research Workspace V0.1 — **thin** host-specific notes. Canonical science logic lives in `AGENTS.md`, `.agents/`, and `.research/`. Adapters answer only: how this host finds the workspace, loads Skills, invokes Subagents/Reviewers, and exposes MCP.

## Support matrix (Phase 9/10 cold-start)

| Harness | Entry file | Skills root | Subagent | Reviewer | MCP | Cold-start CLI (from workspace root) | V0.1 status |
|---------|------------|-------------|----------|----------|-----|--------------------------------------|-------------|
| **Codex** | `AGENTS.md` | `.agents/skills/` | Harness Task / subagent | Fresh session or Task `reviewer` | Plugin MCP | `codex` (interactive) or `codex exec "read AGENTS.md and workspace-resume"` | **verified** |
| **Claude Code** | `CLAUDE.md` → `AGENTS.md` | `.agents/skills/` | `claude` subagents in `.agents/subagents/` | New session + `reviewer.md` | Claude MCP | `claude` (interactive) | **verified** |
| **Cursor** | `AGENTS.md` (rules) | `.agents/skills/` | Cursor subagents / Task tool | Separate chat or subagent | Cursor MCP plugins | `cursor-agent` or IDE Agent panel | **verified** |
| **OpenCode** | `AGENTS.md` | `.agents/skills/` | `.agents/subagents/` prompts | Fresh context prompt | OpenCode MCP | `opencode` | **documented** (CLI varies by install) |
| **DeepSeek Harness** | `AGENTS.md` | `.agents/skills/` | DSH agent routing | Prompt + `reviewer.md` | DSH MCP | `dsh` or `deepseek-harness` per local plugin | **verified** (read-only smoke) |

Cold-start expectation: agent reads `PROJECT → STORY → STATE`, answers resume packet (`workspace-resume`), does **not** invent state.

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

## Limitations (V0.1)

- No framework Python/Shell services; instruction-only.
- Skills are not auto-synced to global skill dirs — workspace `.agents/skills/` is canonical.
- OpenCode CLI not installed on lead integrator host (`opencode` missing); adapter documented only.
- If `~/.gitignore` contains `/*`, child repos under home may need `git -c core.excludesfile=/dev/null` for first commit (observed on this host).
- Remote code repos (Mode B/C) require `RESOURCES.md` path recovery per `git-linking.md`.
