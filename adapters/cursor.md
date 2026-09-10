# Cursor Adapter

Thin harness notes for Cursor IDE Agent / `cursor-agent` CLI.

## Entry

- Workspace rules: `AGENTS.md` (user rules or `.cursor/rules` may reference it).
- Open folder: `research-story-speaker`.

## Skills

- Canonical: `.agents/skills/<name>/SKILL.md`
- User may symlink to `~/.cursor/skills-cursor/` for discovery — **workspace copy remains canonical**.

## Subagents

- Cursor Task tool with subagent types or custom prompts from `.agents/subagents/`.
- Handoff: `.agents/prompts/subagent-handoff.md`

## Reviewer

- Separate Agent chat or `reviewer` subagent; prompts in `.agents/prompts/`.

## MCP

- Cursor MCP plugins (browser, GitHub, Gmail, etc.) per user enablement.
- `cursor-ide-browser` for literature search when Web tool unavailable.

## Cold start

```bash
cd /path/to/research-story-speaker
cursor-agent -p --trust --mode ask "AGENTS.md cold-start; 7-line resume from workspace files only."
```

Or: Cursor → Open project → Agent → "workspace-resume per AGENTS.md".

## Limitations

- Non-interactive `cursor-agent -p` requires `--trust` (or `-f` / `--yolo`) for workspace trust gate
- Agent may not read all skills without explicit routing — use AGENTS.md table
- Multitask subagents need explicit file paths to avoid STATE conflicts
- `monitor-experiment`: one blocking wait covering `sleep_seconds` (host wait
  / `AwaitShell` / equivalent), then one probe. Do not poll a sleeping
  command every 30s or narrate the countdown.
