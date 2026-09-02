# Story-Driven Research Workspace

这是一个 **Story 驱动、文件即记忆** 的自主科研工作区。科研事实保存在 `.research/`，工作流在 `.agents/skills/`。

## Start Here

**默认读取顺序**（冷启动）：

1. `.research/PROJECT.md` — 长期目标
2. `.research/STORY.md` — 当前相信什么
3. `.research/STATE.md` — 现在做到哪里

随后按任务按需读取：`DISCOVERY`、`EXPERIMENTS`、`LITERATURE`、`REVIEWS`、`RESOURCES`。

规范详见 `.agents/references/state-files.md`。

## Autonomy

你拥有较大科研自主权，可以：

- 选择并组合 `.agents/skills/` 中的 Skill
- 调用 `.agents/subagents/` 中的专项 Agent
- 调整任务顺序、提出/放弃实验路线
- 使用当前 Harness 的 MCP、Web、Shell、Git
- 更新 Story 与科研状态（小改自主，大改建议 Review）

**不要求每步询问用户。** Skills 是 strong guidance，不是强制状态机。

## Research Memory

**Chat history 不是科研事实来源。** Workspace 文件才是。

重要进展后更新顺序：

```text
EXPERIMENTS → DISCOVERY → STORY（如需要）→ STATE
```

## Skill Routing

| 场景 | Skill |
|------|-------|
| 新 Session / 陌生 Agent | `workspace-resume` |
| 决定下一步科研 | `research-loop` |
| 维护文件一致性 | `research-memory` |
| 更新 Story | `story-maintenance` |
| 查文献 | `literature-research` |
| 设计实验 | `experiment-design` |
| 执行实验 | `experiment-execution` |
| 分析结果 | `result-analysis` |
| 独立 Review | `experiment-review` |
| 维护框架本身 | `framework-maintenance` |

复杂任务优先找对应 Skill，不要重复发明流程。细节见各 `SKILL.md` 与 `.agents/references/`。

## Subagents

仅当任务适合 **并行、独立上下文、独立 Reviewer、大量阅读** 时派 Subagent：

- `research-lead` — 独立判断下一步
- `literature-scout` — 大量文献检索
- `experiment-agent` — 实验执行
- `result-analyst` — 与执行分离的结果解释
- `reviewer` — 独立批判

简单任务直接执行。Handoff 格式见 `.agents/prompts/subagent-handoff.md`。

## Code & Git

实验代码在 **独立 Git 仓库**（见 `RESOURCES.md`），不在 workspace 内硬编码路径。追溯规范：`.agents/references/git-linking.md`、`experiment-record.md`。

## Story 结构

`STORY.md` 固定六段：`Problem` · `Key Observation` · `Core Idea` · `Evidence` · `Boundary` · `Open Gaps`。循环见 `story-loop.md`。

## Maintenance

- 状态模板：`.agents/templates/`
- 跨 Harness 适配：`adapters/`
- 当前 `.research/` 为 **MOCK** 实例，可替换为真实项目

## 底线

- 重要实验能定位代码、commit、结果
- 不静默删除负结果或历史 Experiment section
- Story 核心机制大改时建议 `experiment-review`
- 并行 Agent 避免同时写同一状态文件
