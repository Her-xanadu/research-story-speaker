# DeepSeek Harness Adapter

Thin harness notes for deepseek-harness (DSH) plugin workflows.

## Entry

- `AGENTS.md` when workspace opened in DSH-enabled environment.
- Project skills: `.agents/skills/`.

## Skills

- Canonical workspace skills only.
- DSH plugin skills live in harness repo — do not merge workflow logic into adapters.

## Subagents

- DSH agent routing to roles defined in `.agents/subagents/`.
- Handoff: `.agents/prompts/subagent-handoff.md`

## Reviewer

- Fresh DSH session with `reviewer.md` or adversarial review plugin if configured.

## MCP

- DSH-exposed MCP per plugin manifest.

## Cold start

```bash
cd /path/to/research-story-speaker
dsh          # or deepseek-harness CLI per local install
```

Smoke: read AGENTS.md + PROJECT + STORY + STATE without other files.

## Limitations

- CLI name varies (`dsh`, plugin-specific).
- V0.1 smoke: read-only resume packet only; full loop tested via workspace files.
