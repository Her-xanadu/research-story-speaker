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

| Role | File | Default model class | Default承接阶段 |
|------|------|---------------------|-----------------|
| `experiment-agent` | `experiment-agent.toml` | workhorse (inherit parent) | W2 执行（持有整段运行） |
| `literature-scout` | `literature-scout.toml` | workhorse (inherit parent) | W1 Gap 文献（并行） |
| `result-analyst` | `result-analyst.toml` | dual: compact→workhorse (inherit) / full→strongest (`model_reasoning_effort = "xhigh"`) | W3 结果解读（默认承接） |
| `research-lead` | `research-lead.toml` | strongest (`xhigh`) | W1 / 判别设计（默认承接） |
| `reviewer` | `reviewer.toml` | strongest (`xhigh`) | 高 stakes 独立批判（gated） |

Required fields per role file: `name`, `description`, `developer_instructions`;
optional `model`, `model_reasoning_effort`, `sandbox_mode`, `mcp_servers`,
`skills.config`. `result-analyst` is dual-tier: its file no longer pins
`xhigh`, so compact ordinary runs inherit the default; Main raises to
`model_reasoning_effort = "xhigh"` for full/high-stakes dispatch.

Scientific body remains `.agents/subagents/<role>.md` (the TOML tells the subagent to read it). Handoff: `.agents/prompts/subagent-handoff.md`.

Default dispatch is the story-loop §阶段职责 matrix (each stage → default role); Main decides only whether an **exception** applies. Do **not** use built-in generic `worker` / `explorer` as a research role.

## Global agents config (host-local, not committed)

Codex reads session-global agent settings from `[agents]` in
`.codex/config.toml` — for example:

```toml
[agents]
enabled = true
max_concurrent_threads_per_session = 3   # 2–3 concurrent as a starting habit
max_depth = 1                            # one-level dispatch: science roles do not spawn science roles
default_subagent_model = "<your workhorse slug>"
```

`.codex/config.toml` is **git-ignored** in this repo (host-local), so these
globals are **not** committed — set them in your own Codex config. Only the
per-role `.codex/agents/<role>.toml` files are versioned. Keep `max_depth = 1`
(Main is the only orchestrator) and `max_concurrent_threads_per_session` around
2–3 as a starting habit, not a science gate.

Known CLI caveat: some Codex CLI versions have had bugs loading custom agents
or spawning a second agent of the same type (upstream #26868 / #27061). Verify
your installed version can spawn these named roles before relying on parallel
dispatch; fall back to sequential dispatch if not.

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
- `monitor-experiment` / live run: **the run's owner holds it** (the
  `experiment-agent` thread monitors its own run; if Main launched a micro-run
  itself, Main keeps the **same Codex conversation**). Run one shell line
  `sleep N; <probe>` (training default `N=300`); when it returns, if still
  running, immediately run the next `sleep N; probe`. Do **not** end the turn
  after 1–2 minutes of narration, do **not** start a new Codex thread to
  "continue monitoring," and do **not** spawn a fresh monitor-only agent.
- Single-conversation runs vs long training: if a Codex thread cannot outlive a
  long run, the execution agent persists a job handoff
  `{ownership, real job id, code/input version, results path, how to
  check/recover}` and hands the **job** (not the thread) back to Main; use
  `wait_agent` / host-native background where available.
