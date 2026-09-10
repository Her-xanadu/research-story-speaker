# Cursor Adapter

Thin harness notes for Cursor IDE Agent / `cursor-agent` CLI.

## Entry

- Workspace rules: `AGENTS.md`.
- Open folder: this workspace root.

## Skills

- Canonical: `.agents/skills/<name>/SKILL.md`
- Native discovery: `.cursor/skills/<name>` directory symlink → `../../.agents/skills/<name>`
- Workspace copy remains canonical. Do not copy into `~/.cursor/skills-cursor/`.

## Subagents

Who gets spawned is decided by:

```text
.cursor/agents/<role>.md
```

Task tool: `subagent_type` = the YAML `name` (`research-lead`, `literature-scout`, `experiment-agent`, `result-analyst`, `reviewer`).

Cursor built-ins (`explore`, `generalPurpose`, `bash`, `browser`, …) are **not** the five research roles. Do not substitute them.

| Role | Wrapper `model` | Default class |
|------|-----------------|---------------|
| `experiment-agent` | `inherit` | workhorse |
| `literature-scout` | `inherit` | workhorse |
| `result-analyst` | `inherit` (must not pick fast Composer) | strongest |
| `research-lead` | `inherit` (must not pick fast Composer) | strongest |
| `reviewer` | `inherit` (must not pick fast Composer) | strongest |

Main chooses the concrete Cursor model slug; the class is mandatory.

Handoff: `.agents/prompts/subagent-handoff.md`.

## Reviewer

- Named `reviewer` Task or a separate Agent chat; prompts in `.agents/prompts/`.

## MCP

- Cursor MCP plugins per user enablement.
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
  command every 30s or narrate the countdown. Workhorse, Main-only.
