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

| Role | `model` / `effort` in wrapper | Default class | Default承接阶段 |
|------|-------------------------------|---------------|-----------------|
| `experiment-agent` | `inherit` | workhorse | W2 执行（持有整段运行） |
| `literature-scout` | `inherit` | workhorse | W1 Gap 文献（并行） |
| `result-analyst` | `inherit` (compact); full raised to `opus` + `effort: xhigh` | dual | W3 结果解读（默认承接） |
| `research-lead` | `opus` + `effort: xhigh` | strongest | W1 / 判别设计（默认承接） |
| `reviewer` | `opus` + `effort: xhigh` | strongest | 高 stakes 独立批判（gated） |

Frontmatter fields available per official docs: `description`, `model`,
`effort` (`low|medium|high|xhigh|max`), `tools`, `disallowedTools`,
`permissionMode`, `skills`, `isolation: worktree`, `background`. Delegation is
`description`-driven. `result-analyst` is dual-tier: default `inherit` for
compact ordinary results, and Main dispatches it with `opus` + `effort: xhigh`
for full/high-stakes.

If the account cannot spawn Opus, inherit the parent; **do not** pick Haiku. Main still follows `AGENTS.md` §模型分档.

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
