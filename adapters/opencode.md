# OpenCode Adapter

Thin harness notes for OpenCode / oh-my-opencode style harnesses.

## Entry

- `AGENTS.md` at workspace root.
- Legacy long configs archived outside this repo — do not duplicate here.

## Skills

- Canonical: `.agents/skills/<name>/SKILL.md`
- OpenCode skill discovery path varies; configure project to include `.agents/skills` if supported.

## Subagents

- Map `.agents/subagents/*.md` to OpenCode agent roles per local config.
- Handoff: `.agents/prompts/subagent-handoff.md`

## Reviewer

- Independent agent role with `reviewer.md` + review prompts.

## MCP

- Per OpenCode MCP plugin configuration.

## Cold start

```bash
cd /path/to/story-research-workspace
opencode   # binary name may differ
```

Document local binary with `which opencode` after install.

## Limitations

- V0.1: cold-start **documented** but not fully automated in CI — verify skill paths after OpenCode updates.
- Do not copy full Skills into OpenCode global config — link workspace path.
