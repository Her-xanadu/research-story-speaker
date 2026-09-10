# Claude Code Adapter

Thin harness notes for Anthropic Claude Code CLI.

## Entry

- `CLAUDE.md` points to `AGENTS.md` and workspace conventions.
- Open terminal in the workspace root.

## Skills

- Canonical: `.agents/skills/<name>/SKILL.md`. Claude Code 另经 `.claude/skills/<name>` 目录 symlink（相对路径 `../../.agents/skills/<name>`）访问。
- Skill names match directory names (`name` frontmatter = dirname).

## Subagents

Who gets spawned is decided by:

```text
.claude/agents/<role>.md
```

YAML `name` + `description` are the routing trigger. Body tells the subagent to read `.agents/subagents/<role>.md` — do not fork the scientific contract.

Model selection is **per dispatch** along a spectrum (cheap/weak → strongest/highest effort); native wrappers ship `inherit` except the one floor role. See `AGENTS.md` §模型分档.

| Role | Native `model` / `effort` in wrapper | Per-dispatch model+effort | Default承接阶段 |
|------|-------------------------------|---------------------------|-----------------|
| `experiment-agent` | `inherit` | Main picks along the spectrum, default cheaper | W2 执行（持有整段运行） |
| `literature-scout` | `inherit` | Main picks along the spectrum, default cheaper | W1 Gap 文献（并行） |
| `result-analyst` | `inherit` | compact: cheaper; **full into Story Evidence = floor → `opus` + `effort: xhigh`** | W3 结果解读（默认承接） |
| `research-lead` | `inherit` | by stakes; **reframe / new Core Idea / expensive next = floor → `opus` + `effort: xhigh`** | W1 / 判别设计（默认承接） |
| `reviewer` | **`opus` + `effort: xhigh`** (native floor default, retained) | always floor | 高 stakes 独立批判（gated） |

Frontmatter fields available per official docs: `description`, `model`,
`effort` (`low|medium|high|xhigh|max`), `tools`, `disallowedTools`,
`permissionMode`, `skills`, `isolation: worktree`, `background`. Delegation is
`description`-driven. Because Claude Code resolves `model`/`effort` per
dispatched subagent, Main sets the concrete model + effort at dispatch time for
every role except `reviewer`.

Only `reviewer` ships a **concrete** `model: opus` (+`xhigh`) in its wrapper —
it is the single always-floor role, so its native default is pinned to the
strongest. Every other role ships `inherit` and Main raises it to `opus` +
`effort: xhigh` **at dispatch time** whenever a floor category applies
(independent Review, new Core Idea / route change / expensive next step,
result heading into Story Evidence); otherwise Main picks the cheapest model
that clears the task. If the account cannot spawn Opus, pin/raise to your
strongest available model instead; **do not** silently fall back to Haiku.

Default dispatch is the story-loop §阶段职责 matrix; Main decides only whether an **exception** applies. `context: fork` / `context: agent` are **skill** fields (run a skill inside a subagent), not agent frontmatter — do not present them as a cross-harness agent setting.

## Parallelism, isolation, permissions

- One-level dispatch: Main is the only orchestrator; science roles do not spawn
  science roles. 2–3 concurrent subagents is a starting habit, not a science gate.
- `isolation: worktree` isolates the **workspace** repo, not a separate linked
  code repo. `experiment-agent` writes both `.research/work/` (the disk report
  Main reads) **and** the external code repo; a worktree would fork the
  workspace and hide those `.research/work/` writes from Main until merged, so
  it is **off by default**. Turn it on only when you also arrange to surface the
  work file back to Main. Code-repo isolation comes from `git-linking.md`
  branches/commits, not from `isolation`.
- **Parallel code-writing `experiment-agent`s must not share a working directory.**
  Two `experiment-agent` runs that both edit the linked code repo may run in
  parallel **only** when each has its own worktree / clone / working directory
  (and its own branch) — e.g. `isolation: worktree` plus a per-run code checkout;
  otherwise serialize them. Read-only or `.research/work/`-only tasks may share.
- Read-only enforcement: every science role must still write its `.research/work/`
  or `.research/reviews/` report, so do **not** blanket-remove Write via
  `disallowedTools`. Canonical-file protection is by instruction (Main owns the
  eight), not a tool block.

Handoff: `.agents/prompts/subagent-handoff.md`.

## Reviewer

- Named `reviewer` subagent or a new session; avoid the same session as the executor when stakes are high.
- Output: `.research/reviews/EXP-xxx/*.md` + `REVIEWS.md` summary.

## MCP

- Claude Code MCP servers from user config (`~/.claude.json` or project config).
- Optional for cold start.

## Cold start

```bash
cd /path/to/research-story-speaker
claude
```

First message: "Follow AGENTS.md cold-start order; workspace-resume from PROJECT+STORY+STATE."

## Limitations

- Claude Code 只自动读 `CLAUDE.md`、`.claude/skills/`、`.claude/agents/`，不自动读 `AGENTS.md` 与 `.agents/skills/`；因此 `CLAUDE.md` 是 boot 指针。
- Long `EXPERIMENTS.md` — read targeted EXP sections only.
- `monitor-experiment` / live run: the **run's owner** holds it (the
  `experiment-agent` subagent monitors its own run; if Main launched a
  micro-run itself, Main waits in-conversation with `sleep N; probe`). Do not
  spawn a fresh monitor-only subagent. For long runs, prefer `background: true`
  execution and let the completion result return, or persist the job handoff so
  the **job** (not the chat) is resumed.
