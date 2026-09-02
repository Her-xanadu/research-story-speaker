# Codex Adapter

Thin harness notes for OpenAI Codex / Codex CLI in this workspace.

## Entry

- Primary: `AGENTS.md` at workspace root (Codex auto-discovery).
- Research state: `.research/`.

## Skills

- Canonical root: `.agents/skills/<name>/SKILL.md`
- Do not duplicate into `.codex/skills` for this project — open workspace at `research-story-speaker` root.
- Route via `AGENTS.md` Skill Routing table.

## Subagents

- Use Codex Task tool with prompt from `.agents/subagents/<role>.md`.
- Handoff format: `.agents/prompts/subagent-handoff.md`
- Work artifacts: `.research/work/<role>/`

## Reviewer

- Prefer different model or fresh Task with `.agents/subagents/reviewer.md` + `.agents/prompts/method-review.md` or `result-review.md`.
- Write reviews to `.research/reviews/EXP-xxx/`.

## MCP

- Codex plugin MCP (Gmail, Drive, browser, etc.) as configured in user environment.
- Optional; not required for cold start.

## Cold start

```bash
cd /path/to/research-story-speaker
codex
# or non-interactive smoke:
codex exec "Follow AGENTS.md. Run workspace-resume: answer Current Story, Gap, Evidence, Active Experiment, Next Action from PROJECT+STORY+STATE only."
```

## Limitations

- `codex exec` may not load all MCP plugins.
- `codex exec --sandbox workspace-write` cannot create `.git/index.lock`, so it cannot `git commit`. Use full-access (`--dangerously-bypass-approvals-and-sandbox`) or let an outer process commit. Observed in C2/C4.
- Absolute paths in `RESOURCES.md` may be stale — use Preferred relative location and `git-linking.md`.
