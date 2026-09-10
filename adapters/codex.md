# Codex Adapter

Thin harness notes for OpenAI Codex / Codex CLI in this workspace.

## Entry

- Primary: `AGENTS.md` at workspace root (Codex auto-discovery).
- Research state: `.research/`.

## Skills

- Canonical root: `.agents/skills/<name>/SKILL.md`
- Do not duplicate into `.codex/skills` for this project — open workspace at the workspace root.
- Route via `AGENTS.md` Skill table.

## Subagents

Who gets spawned is decided by project files:

```text
.codex/agents/<role>.toml
```

Required fields: `name`, `description`, `developer_instructions`.
Codex uses `description` to decide when to spawn that named agent.

| Role | File | Default model class |
|------|------|---------------------|
| `experiment-agent` | `experiment-agent.toml` | workhorse (inherit parent) |
| `literature-scout` | `literature-scout.toml` | workhorse (inherit parent) |
| `result-analyst` | `result-analyst.toml` | strongest (`model_reasoning_effort = "xhigh"`) |
| `research-lead` | `research-lead.toml` | strongest (`xhigh`) |
| `reviewer` | `reviewer.toml` | strongest (`xhigh`) |

Scientific body remains `.agents/subagents/<role>.md` (the TOML tells the subagent to read it). Handoff: `.agents/prompts/subagent-handoff.md`.

Main decides **whether** to spawn. Do **not** use built-in generic `worker` / `explorer` as a research role.

## Reviewer

- Spawn named `reviewer` with a fresh context + `.agents/prompts/method-review.md` or `result-review.md`.
- Write reviews to `.research/reviews/EXP-xxx/`.

## MCP

- Codex plugin MCP as configured in the user environment.
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
- `monitor-experiment` / live run: Main MUST keep the **same Codex conversation**
  and run one shell line `sleep N; <probe>` (training default `N=300`). When
  it returns, if still running, immediately run the next `sleep N; probe`.
  Do **not** end the turn after 1–2 minutes of narration. Do not start a new
  Codex thread to "continue monitoring." That wait is **workhorse**, Main-only.
