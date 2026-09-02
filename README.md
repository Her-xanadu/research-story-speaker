# Story-Driven Autonomous Research Workspace

V0.1 — 纯文件、纯提示词的 Story 驱动自动化科研框架。

## 开发任务（§0）

建立一套**纯文件、纯提示词、Story 驱动、多 Agent、可跨 Agent Harness 迁移**的自动化科研框架。

框架本身不开发新的 Python、Shell、Node.js 服务、数据库、Daemon 或自定义运行时。

框架由以下内容组成：

- Markdown 状态文件
- `AGENTS.md` 项目入口
- 可组合的 `SKILL.md`
- Subagent 提示词
- Reviewer 提示词
- 少量宿主适配文件
- Git 作为科研代码版本定位机制
- MCP、Web、Shell、Git、远程机器等由当前 Agent Harness 提供

论文写作不属于 V0.1。

目标：用户在 Claude Code、Codex、Cursor、OpenCode、DeepSeek Harness 或其他 Agent Harness 中打开本 workspace 后，Agent 能通过状态、Skills 和提示词理解项目历史、当前信念、当前 Story 与下一步研究，并继续推进科研。

## 核心设计（§1）

整个框架围绕唯一的当前 `STORY.md` 运转。

科研循环：

```text
STORY
  ↓
识别当前 Story 最薄弱的环节
  ↓
提出最有价值的科学问题
  ↓
Literature / Experiment / Review
  ↓
获得新证据
  ↓
更新 EXPERIMENTS
  ↓
提炼 DISCOVERY
  ↓
必要时修改 STORY
  ↓
更新 STATE
  ↓
进入下一轮
```

认知分层：

```text
PROJECT      = 我们最终想解决什么
STORY        = 我们现在相信什么
STATE        = 我们现在做到哪里
DISCOVERY    = 我们一路学到了什么
EXPERIMENTS  = 我们实际上做过什么
LITERATURE   = 外部研究告诉了我们什么
REVIEWS      = 独立 Reviewer 如何评价关键实验
RESOURCES    = 代码、数据、算力等外部资源在哪里
```

三层核心关系：

```text
EXPERIMENTS  = What happened?
DISCOVERY    = What did we learn?
STORY        = What do we currently believe?
```

三个文件不互相复制内容。

## Workspace 定位（§2）

Workspace 是科研项目的**控制平面和长期记忆主体**，不等同于实验代码仓库。

支持的代码布局：

| 模式 | 说明 |
|------|------|
| A | 代码在 workspace 内（`code/` 等） |
| B | 代码仓库与 workspace 并列（如 `research-workspace/` + `research-code/`） |
| C | 代码在远程服务器，本机仅保留 workspace |

三种模式均须支持。科研状态不依赖当前工作目录、绝对路径、单一 Harness session、单一 MCP 或 Agent 内置 memory。Workspace 才是项目入口。

## V0.1 目录结构（§3）

```text
workspace/
├── AGENTS.md
├── CLAUDE.md
├── README.md
│
├── .research/
│   ├── PROJECT.md
│   ├── STORY.md
│   ├── STATE.md
│   ├── DISCOVERY.md
│   ├── EXPERIMENTS.md
│   ├── LITERATURE.md
│   ├── REVIEWS.md
│   ├── RESOURCES.md
│   ├── reviews/          # 按 EXP-ID 存放 method/result review
│   └── work/             # Subagent 工作产物
│
├── .agents/
│   ├── skills/           # 唯一 canonical Skill 根（禁止多平台复制完整 Skills）
│   ├── subagents/
│   ├── prompts/
│   ├── references/
│   └── templates/
│
└── adapters/             # 各 Harness 薄适配说明
```

Canonical Skills（10）：

- `research-loop` — 顶层编排
- `workspace-resume` — 冷启动恢复
- `story-maintenance` — Story 语义与变更
- `literature-research` — 文献调研
- `experiment-design` — 实验设计
- `experiment-execution` — 实验执行
- `result-analysis` — 结果分析
- `experiment-review` — 实验评审协调
- `research-memory` — 状态文件更新
- `framework-maintenance` — 框架维护与审计

## Workspace Git（§4）

Workspace 与科研代码 Git 是**两个不同概念**。

建议将整个 workspace 初始化为独立 Git 仓库，主要版本控制：

- Story、State、Discovery、Experiments、Literature、Reviews、Resources
- Skills、Subagents、Prompts、Adapter

用于追溯 Story 何时改变、Discovery 何时形成、Experiment 何时加入、Skill 何时修改。

Workspace Git **不承担**大型实验结果保存。实验代码须使用独立 Git；每个正式 Experiment 至少能定位到 codebase、repository、commit、entry/config 与结果位置。

## 当前状态

V0.1 freeze: 10 canonical Skills、8 状态文件、subagents/prompts、5 harness adapters、MOCK 科研实例与 `story-research-code` EXP-001 追溯链。见 git tag `v0.1`。
