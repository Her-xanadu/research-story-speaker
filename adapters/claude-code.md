# Claude Code Adapter

Thin harness notes for Anthropic Claude Code CLI.

## Entry

- `CLAUDE.md` points to `AGENTS.md` and workspace conventions.
- Open terminal in `story-research-workspace` root.

## Skills

- Read `.agents/skills/<name>/SKILL.md` on demand (no project `.claude/skills` mirror required for V0.1).
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
- Optional for MOCK EXP-001.

## Cold start

```bash
cd /path/to/story-research-workspace
claude
```

First message: "Follow AGENTS.md cold-start order; workspace-resume from PROJECT+STORY+STATE."

## Limitations

- Claude Code does not auto-index all Skills — cite path when routing.
- Long `EXPERIMENTS.md` — read targeted EXP sections only.
