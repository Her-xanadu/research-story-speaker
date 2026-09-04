# research-story-speaker

Framework base: v0.2 — 纯文件、纯提示词的 Story 驱动自动化科研框架。

## Quick Start

1. 以本仓库为模板（clone 或复制）。
2. 用任一已支持的 Agent Harness 打开该文件夹（Codex、Claude Code、Cursor、DeepSeek Harness；OpenCode 仅文档）。
3. 对 Agent 说：「Read AGENTS.md and initialize this research project.」
4. 提供研究目标、代码/数据位置、长期约束。信息不足时 Agent 只追问缺的项。
5. Agent 将根 `.research/` 从 `UNINITIALIZED` **materialize** 为 `ACTIVE`（八个文件已在，不是新生成），然后进入第一个 Story loop。不要期待编造完整 Story；缺证据的段保持 `_Not established yet._`。

MOCK 闭环示例：[`examples/mock-flow-detection/`](examples/mock-flow-detection/)。框架审核与 Harness 证据：[`docs/validation/`](docs/validation/)。

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

三分：框架层 / 项目层 / 示例与验证。

| 层 | 内容 |
|----|------|
| 框架层 | `AGENTS.md`、`CLAUDE.md`、`.agents/`、`.claude/`、`adapters/` |
| 项目层 | `.research/`（当前真实项目；初始 `UNINITIALIZED`） |
| 示例与验证 | `examples/mock-flow-detection/`；`docs/validation/` |

```text
workspace/
├── AGENTS.md / CLAUDE.md / README.md
├── .agents/              # 框架层：skills、templates、references
├── .claude/              # 框架层：Claude Code 发现入口
├── adapters/             # 框架层：薄宿主适配
├── .research/            # 项目层；初始 UNINITIALIZED
├── examples/mock-flow-detection/
└── docs/validation/
```

Canonical Skills（12）：`workspace-resume`、`research-loop`、`story-maintenance`、`idea-evaluation`、`literature-research`、`experiment-design`、`experiment-execution`、`result-analysis`、`evidence-verification`、`experiment-review`、`research-memory`、`framework-maintenance`（仅维护框架、升级 Harness 或发布版本时使用）。

## Workspace Git（§4）与升级边界

Workspace Git 与科研代码 Git 是两个概念。升级时**只合并框架层**（`AGENTS.md`、`CLAUDE.md`、`.agents/`、`.claude/`、`adapters/`），**永不覆盖** `.research/`。

## 跨 Harness

- UNINITIALIZED cold-start portability: validated on Codex, Claude Code, Cursor, and DeepSeek Harness.
- Initialized write/handoff portability: validated on Codex → Claude Code.
- Research Intelligence live: Codex and Claude Code (Wave G / E-B1 / Case 10); Cursor / DSH not re-run for V0.2 gates.
- OpenCode: documentation-only, not tested.

## 当前状态

V0.2（本 tag）在 v0.1.1 之上加 Research Intelligence Layer：6 份 Layer 2、恰好 2 个新 Skill（`idea-evaluation`、`evidence-verification`）、8 个 task prompt。FROZEN CORE 与 v0.1.1 byte-identical。根 `.research/` 仍为干净 UNINITIALIZED 模板。V0.1（`8db3b30`）与 V0.1.1（`762deb4c`）对象不变。MOCK 闭环在 `examples/mock-flow-detection/`；验证证据在 `docs/validation/`。

行为证据边界（不要把指令层行数增长当成「更会推理」）：

- Gate D：10 个 case 对 v0.1.1 的 **instruction dry-read**（不是 live）。
- Wave G live：harness 可移植、输出形状、写纪律。2026-09-04 05:31 T1/T2 判断类 PASS 为 **contaminated**（评分规则进了 agent prompt）。
- E-B1 去泄漏复跑：Codex / Claude Code 各 1 × Case 01 / Case 03 命中（T1 `REVISE`，T2 honest-baseline `does not address`）。fixture 仍含提问者层面的自曝；Case 02（ADVANCE）为 tag 后 held-out。
- Wave H：`skill-evolution` dogfood **reject**。
- Case 10：Codex live **file-level PASS**（无 Idea/Evidence/Reviewer 工作文件）。agent 仍打开了 fixture README 评分段，**不是** unleaked protection。

已知债务（v0.2.1）：OpenCode 实测；Case 10 / Case 01 unleaked 夹具；Claude live 读文件轨迹；`skill-evolution` 评分者独立性；deep-lit 预算行；元规则复制收敛。
