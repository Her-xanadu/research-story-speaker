# OpenCode Adapter

Thin harness notes for OpenCode / oh-my-opencode style harnesses.

**DIY host** — not an official install path; documentation-only. Point OpenCode at `AGENTS.md` and `.agents/`. Follow `AGENTS.md` §模型分档.

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
cd /path/to/research-story-speaker
opencode   # binary name may differ
```

Document local binary with `which opencode` after install.

## Limitations

- V0.1: cold-start **documented** but not fully automated in CI — verify skill paths after OpenCode updates.
- Do not copy full Skills into OpenCode global config — link workspace path.
