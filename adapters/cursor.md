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

Model selection is **per dispatch** along a spectrum (cheap/weak → strongest/highest effort). All five Cursor role files ship `model: inherit`; Main sets the concrete slug + effort at dispatch time. See `AGENTS.md` §模型分档.

| Role | Native wrapper `model` | Per-dispatch model+effort | Default承接阶段 |
|------|-----------------|---------------------------|-----------------|
| `experiment-agent` | `inherit` | Main picks along the spectrum, default cheaper | W2 执行（持有整段运行） |
| `literature-scout` | `inherit` | Main picks along the spectrum, default cheaper | W1 Gap 文献（并行） |
| `result-analyst` | `inherit` | compact: cheaper; **full into Story Evidence = floor → strong slug `[effort=high]`** | W3 结果解读（默认承接） |
| `research-lead` | `inherit` | by stakes; **reframe / new Core Idea / expensive next = floor → strong slug `[effort=high]`** | W1 / 判别设计（默认承接） |
| `reviewer` | `inherit` (body pins floor) | always floor: strong slug `[effort=high]` | 高 stakes 独立批判（gated） |

Frontmatter fields per official docs: `name`, `description`, `model`
(+`[effort=high]` params), `readonly`, `is_background`; a **non-empty body is
required** to register the agent. `/name` explicitly invokes a role; `Task
subagent_type = <name>` dispatches it.

### Per-dispatch model selection (spectrum + thin floor)

**`inherit` cannot guarantee "strongest."** All five Cursor role files ship
`model: inherit`, so a role inherits the parent's model — which may be a fast
Composer. Cursor resolves the model per Task dispatch, so **Main passes the
concrete slug + effort at dispatch time**, not once per install:

- Non-floor work (implement, launch, monitor, compact result read, literature
  consult, routine discriminating design): Main picks the cheapest slug along
  the spectrum that clears the task.
- **Floor categories** — independent Review (`reviewer`), new Core Idea / route
  change / expensive next step (`research-lead`), result heading into Story
  Evidence (`result-analyst` full): Main dispatches with the **strongest**
  available slug + `[effort=high]`, e.g.

```yaml
model: <your-strongest-Cursor-slug>[effort=high]
```

`reviewer` is the one always-floor role: its body pins strongest and Main must
never dispatch it on a fast/Composer slug. Until a strong slug is passed, invoke
floor roles only from a strong parent (the role bodies already guard "do not run
this role from a fast/Composer parent"). Do **not** set `readonly: true` on any
role — they must write `.research/work/` or `.research/reviews/`, and `readonly`
blocks file edits and state-changing shell.

Default dispatch is the story-loop §阶段职责 matrix; Main decides only whether an **exception** applies. Cursor built-ins (`explore`, `generalPurpose`, `bash`, `browser`, …) are **not** the five research roles.

## Parallelism, isolation, handoff

- Cursor subagents **share the workspace checkout by default**; isolation is
  opt-in and, when enabled, covers the **workspace** repo, not a separate linked
  code repo. Multitask subagents need distinct `task-slug` file paths to avoid
  `.research/` write conflicts.
- **Parallel code-writing `experiment-agent`s must not share a working directory.**
  Because the checkout is shared by default, two `experiment-agent` runs that
  both edit the linked code repo may run in parallel **only** when each has its
  own worktree / clone / working directory (and its own branch); otherwise
  serialize them. Read-only or `.research/work/`-only tasks may share the checkout.
- One-level dispatch: Main is the only orchestrator; science roles do not spawn
  science roles. 2–3 concurrent subagents is a starting habit, not a science gate.
- Genuine parallelism is for **independent** tasks only. Serial specialist
  handoff (Planner → Implementer → Verifier, per Cursor's own docs) is also real
  multi-agent — our W1→W2→W3 default dispatch is exactly this serial form; not
  everything must overlap.

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
- `monitor-experiment`: the **run's owner** holds it (the `experiment-agent`
  Task monitors its own run; if Main launched a micro-run itself, Main waits).
  One blocking wait covering `sleep_seconds` (host wait / `AwaitShell` /
  equivalent), then one probe. Do not poll a sleeping command every 30s, narrate
  the countdown, or spawn a fresh monitor-only Task. For long runs, use
  `is_background` execution or persist the job handoff so the **job** (not the
  chat) is resumed.
