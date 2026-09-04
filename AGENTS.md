# research-story-speaker

Framework base: v0.2.1-in-progress

这是一个 **Story 驱动、文件即记忆** 的自主科研工作区。科研事实保存在 `.research/`，工作流在 `.agents/skills/`。

## Workspace Identity

本工作区是当前科研项目的控制平面与长期记忆。先读 `.research/PROJECT.md` 的 **Project Status**（仅 `UNINITIALIZED` | `ACTIVE`）。

- `UNINITIALIZED`：走 `workspace-resume` **初始化**。收集最低必要信息（研究目标、代码/数据位置、约束），**materialize** 已经存在的八个 `.research/` 文件为 `ACTIVE` 项目状态（不是新生成八文件），STATE Story Status → `IN_PROGRESS`。不要编造 Story 或实验。
- `ACTIVE`：按 Start Here 冷启动后继续科研。

## Start Here

未初始化时不要直接进 `research-loop`。初始化协议见 `workspace-resume`。

**默认读取顺序**（冷启动，`ACTIVE` 之后）：

1. `.research/PROJECT.md` — 长期目标与 Project Status
2. `.research/STORY.md` — 当前相信什么
3. `.research/STATE.md` — 现在做到哪里

随后按任务按需读取：`DISCOVERY`、`EXPERIMENTS`、`LITERATURE`、`REVIEWS`、`RESOURCES`。

`ACTIVE` 且 STATE 已指明普通 sanity / exploratory EXP 时：直接走 compact
`experiment-design` / `result-analysis`，不经 `workspace-resume` /
`research-loop`。各 Skill 自己声明 compact 不读什么。

代码仓库由 `RESOURCES.md` 定位，可为 workspace 内 / 并列 / 远程。Git 追溯与状态规范在 `.agents/references/`。

## Autonomy

你拥有较大科研自主权：选择并组合 `.agents/skills/`、调用 `.agents/subagents/`、调整任务顺序、提出或放弃实验路线、使用当前 Harness 的 MCP / Web / Shell / Git、更新 Story 与科研状态（小改自主，大改建议 Review）。

**不要求每步询问用户。** Skills 是 strong guidance，不是强制状态机。重要实验须能定位代码、commit、结果；Story 核心机制大改时建议 `experiment-review`。

## Research Memory

**Chat history 不是科研事实来源。** Workspace 文件才是。重要进展后的更新顺序见 `state-files.md` §更新顺序。`STORY.md` 六段见该文件与 `story-maintenance`。

并行 Agent 避免同时写同一状态文件。

## Skill Routing

| 场景 | Skill |
|------|-------|
| 未初始化（PROJECT Status = `UNINITIALIZED`） | `workspace-resume` |
| ACTIVE 且 STATE 下一步已是普通 sanity / exploratory | compact `experiment-design` →（跑代码才）`experiment-execution` → compact `result-analysis`；不要 `workspace-resume` / `research-loop` |
| 新 Session / 陌生 Agent（ACTIVE 但下一步不清） | `workspace-resume` 后按 STATE 或 `research-loop` |
| 决定下一步科研 | `research-loop` |
| 维护文件一致性 | `research-memory` |
| 更新 Story | `story-maintenance` |
| 新 Core Idea / 换路线 / 高代价实验 | `idea-evaluation` |
| 查文献 | `literature-research` |
| 设计实验 | `experiment-design` |
| 执行实验 | `experiment-execution` |
| 分析结果 | `result-analysis` |
| 结果拟进 Story Evidence / 高风险结果 | `evidence-verification` |
| 独立 Review | `experiment-review` |
| 仅维护框架、升级 Harness 或发布版本时使用 | `framework-maintenance` |

复杂任务优先找对应 Skill，不要重复发明流程。细节见各 `SKILL.md` 与 `.agents/references/`。`.agents/references/research-intelligence/` 与 `.agents/prompts/` 随对应 Skill 按需加载，冷启动不必通读。

## Subagents

仅当任务适合 **并行、独立上下文、独立 Reviewer、大量阅读** 时派 Subagent：

- `research-lead` — 独立判断下一步
- `literature-scout` — 大量文献检索
- `experiment-agent` — 实验执行
- `result-analyst` — 与执行分离的结果解释
- `reviewer` — 独立批判

简单任务直接执行。Handoff 格式见 `.agents/prompts/subagent-handoff.md`。

**写权限：** Subagent 只写 `.research/work/` 或 `.research/reviews/<EXP-ID>/`；八个 canonical 状态文件由 Main Agent 更新。

## Maintenance

- 状态模板：`.agents/templates/`
- 跨 Harness 适配：`adapters/`
- 不静默删除负结果或历史 Experiment section
- 升级框架层（`AGENTS.md`、`CLAUDE.md`、`.agents/`、`.claude/`、`adapters/`）时**永不覆盖** `.research/`
