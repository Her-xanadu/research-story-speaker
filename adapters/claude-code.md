# Claude Code Adapter

Thin harness notes for Anthropic Claude Code CLI.

## Entry

- `CLAUDE.md` points to `AGENTS.md` and workspace conventions.
- Open terminal in the workspace root.

## Skills

- Canonical: `.agents/skills/<name>/SKILL.md`. Claude Code 另经 `.claude/skills/<name>` 目录 symlink（相对路径 `../../.agents/skills/<name>`）访问。
- Skill names match directory names (`name` frontmatter = dirname).

## Subagents

Who gets spawned is decided by:

```text
.claude/agents/<role>.md
```

YAML `name` + `description` are the routing trigger. Body tells the subagent to read `.agents/subagents/<role>.md` — do not fork the scientific contract.

| Role | `model` in wrapper | Default class |
|------|--------------------|---------------|
| `experiment-agent` | `inherit` | workhorse |
| `literature-scout` | `inherit` | workhorse |
| `result-analyst` | `opus` | strongest |
| `research-lead` | `opus` | strongest |
| `reviewer` | `opus` | strongest |

If the account cannot spawn Opus, inherit the parent; **do not** pick Haiku. Main still follows `AGENTS.md` §模型分档.

Handoff: `.agents/prompts/subagent-handoff.md`.

## Reviewer

- Named `reviewer` subagent or a new session; avoid the same session as the executor when stakes are high.
- Output: `.research/reviews/EXP-xxx/*.md` + `REVIEWS.md` summary.

## MCP

- Claude Code MCP servers from user config (`~/.claude.json` or project config).
- Optional for cold start.

## Cold start

```bash
cd /path/to/research-story-speaker
claude
```

First message: "Follow AGENTS.md cold-start order; workspace-resume from PROJECT+STORY+STATE."

## Limitations

- Claude Code 只自动读 `CLAUDE.md`、`.claude/skills/`、`.claude/agents/`，不自动读 `AGENTS.md` 与 `.agents/skills/`；因此 `CLAUDE.md` 是 boot 指针。
- Long `EXPERIMENTS.md` — read targeted EXP sections only.
