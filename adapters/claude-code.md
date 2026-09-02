# Claude Code Adapter

Thin harness notes for Anthropic Claude Code CLI.

## Entry

- `CLAUDE.md` points to `AGENTS.md` and workspace conventions.
- Open terminal in `story-research-workspace` root.

## Skills

- Canonical: `.agents/skills/<name>/SKILL.md`. Claude Code 另经 `.claude/skills/<name>` 目录 symlink（相对路径 `../../.agents/skills/<name>`）访问。
- Skill names match directory names (`name` frontmatter = dirname).

## Subagents

- Invoke with Claude Code subagent mechanism or paste `.agents/subagents/<role>.md` into a focused session.
- Parallel scouts: `literature-scout`, `experiment-agent`, `result-analyst`.
- Handoff: `.agents/prompts/subagent-handoff.md`

## Reviewer

- New session or subagent with `reviewer.md`; avoid same session as executor when stakes are high.
- Output: `.research/reviews/EXP-xxx/*.md` + `REVIEWS.md` summary.

## MCP

- Claude Code MCP servers from user config (`~/.claude.json` or project config).
- Optional for cold start.

## Cold start

```bash
cd /path/to/story-research-workspace
claude
```

First message: "Follow AGENTS.md cold-start order; workspace-resume from PROJECT+STORY+STATE."

## Limitations

- Claude Code 只自动读 `CLAUDE.md` 与 `.claude/skills/`，不自动读 `AGENTS.md` 与 `.agents/skills/`；因此 `CLAUDE.md` 是 boot 指针，`.claude/skills/` 为目录 symlink。
- Long `EXPERIMENTS.md` — read targeted EXP sections only.
