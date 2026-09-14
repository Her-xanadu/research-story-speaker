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

Model selection follows a **spectrum** (cheap/weak → strongest/highest effort). Only the always-floor role pins a native default; the rest inherit. See `AGENTS.md` §模型分档.

| Role | File | Native model config | Floor handling | Default承接阶段 |
|------|------|---------------------|----------------|-----------------|
| `experiment-agent` | `experiment-agent.toml` | inherit (no pin) | — spectrum, default cheaper | W2 执行（持有整段运行） |
| `literature-scout` | `literature-scout.toml` | inherit (no pin) | — spectrum, default cheaper | W1 Gap 文献（并行） |
| `result-analyst` | `result-analyst.toml` | inherit (no pin) | **full into Story Evidence = floor**: dispatch from a strongest session, or locally pin `model` + `xhigh` for full-mode use | W3 结果解读（默认承接） |
| `research-lead` | `research-lead.toml` | inherit (no pin) | **reframe / new Core Idea / expensive next = floor**: dispatch from a strongest session | W1 / 判别设计（默认承接） |
| `reviewer` | `reviewer.toml` | **`model_reasoning_effort = "xhigh"`** (native floor default, retained); `model` inherit until bound | always floor | 高 stakes 独立批判（gated） |

Required fields per role file: `name`, `description`, `developer_instructions`;
optional `model`, `model_reasoning_effort`, `sandbox_mode`, `mcp_servers`,
`skills.config`. Because a Codex `.toml` is **static config** (no documented
per-dispatch model/effort override), roles that are only *sometimes* floor
(`research-lead`, `result-analyst` full) ship **no pin** so routine work stays
cheap; their floor level is reached by dispatching from a strongest session (or
a host-local pin). Only the *always*-floor `reviewer` keeps a native `xhigh`
pin.

Scientific body remains `.agents/subagents/<role>.md` (the TOML tells the subagent to read it). Handoff: `.agents/prompts/subagent-handoff.md`.

Default dispatch is the story-loop §阶段职责 matrix (each stage → default role); Main decides only whether an **exception** applies. Do **not** use built-in generic `worker` / `explorer` as a research role.

## Global agents config (host-local, not committed)

Codex reads session-global agent settings from `[agents]` in
`.codex/config.toml` — for example:

```toml
[agents]
enabled = true
max_concurrent_threads_per_session = 3   # 2–3 concurrent as a starting habit
default_subagent_model = "<your cheap-end slug>"
```

`.codex/config.toml` is **git-ignored** in this repo (host-local), so these
globals are **not** committed — set them in your own Codex config. Only the
per-role `.codex/agents/<role>.toml` files are versioned. Keep
`max_concurrent_threads_per_session` around 2–3 as a starting habit, not a
science gate.

One-level dispatch (Main is the only orchestrator; science roles do not spawn
science roles) is enforced by **instruction** — the role files and
`subagent-handoff.md` — not by a config key. Do **not** add a `max_depth` key;
it is not part of the current Codex `[agents]` schema.

**Parallel code-writing `experiment-agent`s must not share a working directory.**
Two `experiment-agent` runs that both edit the linked code repo may run in
parallel **only** when each has its own git worktree / clone / working directory
(and its own branch); otherwise their edits, commits, and results collide —
serialize them instead. Read-only or `.research/work/`-only tasks can share.

## Model selection (spectrum + static-config caveat)

Main selects model + reasoning effort **per dispatch** along the spectrum: use
the cheapest that clears the task, and raise to the strongest + `xhigh` for the
**thin floor** (independent Review, new Core Idea / route change / expensive
next step, result heading into Story Evidence). See `AGENTS.md` §模型分档.

Codex `.toml` is **static** — there is no documented per-dispatch model/effort
override — so the floor is realized two ways:

- **Always-floor role (`reviewer`)**: keep the native `model_reasoning_effort =
  "xhigh"` pin. Optionally also bind `model` once to your strongest Codex slug
  (host-local); until then it inherits the session, so invoke it only from a
  strongest parent.
- **Sometimes-floor roles (`research-lead`, `result-analyst` full)**: ship **no
  pin** so routine dispatches stay cheap. For a floor-level dispatch, run Main
  in a strongest session (the subagent inherits it) or add a host-local
  `model`/`xhigh` pin in your own uncommitted config. Do **not** pin these
  roles to `xhigh` in the versioned template — that would force every routine
  design / compact read onto the strongest model.

```toml
# host-local only (e.g. reviewer.toml, or a floor-mode research-lead/result-analyst copy)
model = "<your strongest Codex slug>"
model_reasoning_effort = "xhigh"
```

For the cheap end of the spectrum, set a global `default_subagent_model` in the
git-ignored `[agents]` block (see above).

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
