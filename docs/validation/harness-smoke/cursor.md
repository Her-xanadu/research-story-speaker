# Cursor Agent harness smoke

- **日期:** 2026-09-02
- **CLI 路径:** `/Users/herxanadu/.local/bin/cursor-agent`
- **版本:** `2026.08.31-4057e58`（`cursor-agent --version`）

## 实际执行的完整命令行

`--mode ask` 存在，help 写明 ask 为 read-only。同时使用 `-p --trust --workspace`。

```bash
cd /Users/herxanadu/Documents/story-research-workspace
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/cursor-agent -p --trust --mode ask \
  --workspace /Users/herxanadu/Documents/story-research-workspace \
  'You are a fresh agent with no chat history. Do NOT modify any file. Cold-start this workspace by following AGENTS.md, then answer in exactly 7 numbered lines: (1) what this project is; (2) the current Story core idea in one sentence; (3) where the work stands now (active/completed experiments, Story status); (4) the list of skills you can find under .agents/skills/; (5) a one-line summary of EXP-001 including the code repository relative path and the git commit it binds to; (6) which files you read to answer, in order; (7) the recommended next action. Answer from workspace files only.'
```

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788361117 |
| 结束 epoch | 1788361144 |
| 耗时 | 27s |
| 运行前 `git status --porcelain` | 空 |
| 运行后 `git status --porcelain` | 空 |
| 是否越权写工作区文件 | 否（DIRTY=0，无需 checkout） |
| stdout 行数 | 7 |
| stderr 行数 | 0 |

## 原始 stdout

```text
1. 这是一个 **MOCK** 科研工作区：用「轻量流统计特征异常检测」在 CICIDS2017 等基准上验证，能否以少量可解释流级统计量达到接近强基线的检出能力，同时显著降低计算与部署成本（代码并列于 `../story-research-code`）。
2. Story 核心想法：仅用流持续时间、包数比率、字节不对称三个流统计特征，配合简单 per-flow 归一化的轻量分类器，即可捕获多数常见攻击的主要信号，复杂特征主要带来边际收益。
3. 当前进展：无活跃实验；EXP-001（MOCK）已完成，method/result review 已完成；Story 状态为 `IN_PROGRESS`，真实 CICIDS 子集尚未验证，Open Gap #1 仍开放。
4. `.agents/skills/` 下技能：`workspace-resume`、`research-loop`、`research-memory`、`story-maintenance`、`literature-research`、`experiment-design`、`experiment-execution`、`result-analysis`、`experiment-review`、`framework-maintenance`。
5. EXP-001：在 MOCK 合成流上对比三特征 LR 与全特征 IF 基线，三特征 F1=1.0000、IF F1=0.5455；代码库 `../story-research-code`，绑定 commit `b0621e2ed266cc26020fac5b3295a588469bb495`。
6. 读取顺序：`AGENTS.md` → `.research/PROJECT.md` → `.research/STORY.md` → `.research/STATE.md` → `.agents/skills/` 目录列举 → `.research/EXPERIMENTS.md` → `.research/RESOURCES.md`。
7. 建议下一步：将合成流替换为真实 CICIDS 子集，按 EXP-001 协议复跑（Open Gap #1）。
```

## 原始 stderr

```text
(empty)
```

## 七项核对表

对照 stdout；第 (5) 项事实来源 `.research/EXPERIMENTS.md`。

| # | 项 | 判定 | 依据 |
|---|----|------|------|
| 1 | 项目是什么 | Y | MOCK 科研工作区，轻量流统计特征异常检测，代码并列于 `../story-research-code`。 |
| 2 | Story 核心想法 | Y | 流持续时间、包数比率、字节不对称 + per-flow 归一化捕获多数常见攻击主信号。 |
| 3 | 当前进展 | Y | 无活跃实验；EXP-001 MOCK 已完成且 review 完成；Story `IN_PROGRESS`；Open Gap #1 仍开放。 |
| 4 | `.agents/skills/` 列表 | Y | 十项齐全，与目录 enumeration 一致。 |
| 5 | EXP-001 路径与 commit | Y | `../story-research-code` 与 `b0621e2ed266cc26020fac5b3295a588469bb495`。 |
| 6 | 阅读顺序 | Y | 自报 `AGENTS.md` → `.research/PROJECT.md` → `STORY.md` → `STATE.md` → `.agents/skills/` 目录列举 → `EXPERIMENTS.md` → `RESOURCES.md`。核心四文件顺序正确；skills 列举服务第 4 项；EXPERIMENTS 服务第 5 项；RESOURCES 为少量额外读取。 |
| 7 | 推荐下一步 | Y | 真实 CICIDS 子集按 EXP-001 协议复跑（Open Gap #1），与 `STATE.md` 一致。 |

**七项 Y 数:** 7/7

## 观察到的宿主限制

- 无 `--trust` 时 headless 会被 workspace trust gate 挡住（adapters 既有 Limitation；本次带 `--trust` 后顺利进入）。
- `--mode ask` 可用且本次未写工作区文件；help 同时提供 `--mode plan` / `--plan` 作为另一只读入口。
- `-p/--print` help 写明「Has access to all tools, including write and shell」，因此只读不能只靠 `-p`，必须叠加 `--mode ask`（或 plan）。
- 成功运行 stderr 为空；未见 MCP 加载警告。
