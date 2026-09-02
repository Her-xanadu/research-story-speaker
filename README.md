# Story-Driven Autonomous Research Workspace

Framework base: v0.1.1 — 纯文件、纯提示词的 Story 驱动自动化科研框架。

## Quick Start

1. Use this repository as a template (clone or copy).
2. Open the folder in any supported Agent Harness (Codex, Claude Code, Cursor, DeepSeek Harness; OpenCode documented only).
3. Tell the Agent: “Read AGENTS.md and initialize this research project.”
4. Provide the research goal and code/data locations.
5. Agent populates `.research/` from UNINITIALIZED to ACTIVE and starts the first Story loop. Do not expect a fabricated Story.

MOCK closed-loop example: [`examples/mock-flow-detection/`](examples/mock-flow-detection/). Framework review and harness evidence: [`docs/validation/`](docs/validation/).

## 核心设计（§1）

整个框架围绕唯一的当前 `STORY.md` 运转。循环见 `.agents/references/story-loop.md` §核心循环。

八个状态文件的职责与认知分层见 `.agents/references/state-files.md`。EXPERIMENTS 记录发生了什么；DISCOVERY 记录学到了什么；STORY 记录当前信念（不含具体性能数字）；STATE 只回答现在在哪、下一步做什么。三层不互相复制数字。

## Workspace 定位（§2）

Workspace 是科研项目的**控制平面和长期记忆主体**，不等同于实验代码仓库。

| 模式 | 说明 |
|------|------|
| A | 代码在 workspace 内（`code/` 等） |
| B | 代码仓库与 workspace 并列 |
| C | 代码在远程服务器，本机仅保留 workspace |

科研状态不依赖当前工作目录、绝对路径、单一 Harness session。Workspace 才是项目入口。

## 目录结构

```text
workspace/
├── AGENTS.md
├── CLAUDE.md
├── README.md
├── .research/            # 当前真实项目；初始 UNINITIALIZED
├── .agents/
├── .claude/skills/       # symlink → .agents/skills
├── adapters/
├── examples/mock-flow-detection/
└── docs/validation/
```

Canonical Skills（10）：`workspace-resume`、`research-loop`、`story-maintenance`、`literature-research`、`experiment-design`、`experiment-execution`、`result-analysis`、`experiment-review`、`research-memory`、`framework-maintenance`（仅维护框架时使用）。

## Workspace Git（§4）

Workspace Git 与科研代码 Git 是两个概念。升级时只合并框架层（`AGENTS.md`、`CLAUDE.md`、`.agents/`、`.claude/`、`adapters/`），**永不覆盖** `.research/`。

## 跨 Harness（V0.1.1 声明占位，Gate B 用验证证据填实）

- UNINITIALIZED cold-start portability: pending Wave C1
- Initialized write/handoff portability: pending Wave C2–C4
- Research-loop portability: partially validated
- OpenCode: documentation-only, not tested

## 当前状态

V0.1（tag `v0.1`，对象不变）已通过 Gate 1–3。V0.1.1 将根 `.research/` 改为干净 UNINITIALIZED 模板；MOCK 闭环移至 `examples/mock-flow-detection/`；验证证据在 `docs/validation/`。

已知债务（V0.2）：Test F/G/J；Subagent 狗食落盘；四 Harness 全写闭环；OpenCode 实测。
