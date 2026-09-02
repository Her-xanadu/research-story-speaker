# Claude Code harness smoke

- **日期:** 2026-09-02
- **CLI 路径:** `/Users/herxanadu/.local/bin/claude`
- **版本:** `2.1.220 (Claude Code)`（`claude --version`）

## 实际执行的完整命令行

### 第一次（失败，exit 1，2s）

`--allowedTools` 在 help 中为 `<tools...>`（variadic）。prompt 放在参数列表末尾时被当成额外 tool 名，`--print` 收不到 prompt。

```bash
cd /Users/herxanadu/Documents/story-research-workspace
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude -p --permission-mode plan --output-format text \
  --allowedTools Read,Glob,Grep \
  'You are a fresh agent with no chat history. Do NOT modify any file. Cold-start this workspace by following AGENTS.md, then answer in exactly 7 numbered lines: (1) what this project is; (2) the current Story core idea in one sentence; (3) where the work stands now (active/completed experiments, Story status); (4) the list of skills you can find under .agents/skills/; (5) a one-line summary of EXP-001 including the code repository relative path and the git commit it binds to; (6) which files you read to answer, in order; (7) the recommended next action. Answer from workspace files only.'
```

stderr：

```text
Error: Input must be provided either through stdin or as a prompt argument when using --print
```

运行前后 `git status --porcelain` 均为空。此失败是参数位置问题，不是超时；按「超时才重试」之外的命令行修正，立刻重跑一次。

### 第二次（成功，exit 0，37s）— 本产物以此次为准

用 `--` 终结选项，使 prompt 成为位置参数。

```bash
cd /Users/herxanadu/Documents/story-research-workspace
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude --permission-mode plan --output-format text \
  --allowedTools Read,Glob,Grep -p -- \
  'You are a fresh agent with no chat history. Do NOT modify any file. Cold-start this workspace by following AGENTS.md, then answer in exactly 7 numbered lines: (1) what this project is; (2) the current Story core idea in one sentence; (3) where the work stands now (active/completed experiments, Story status); (4) the list of skills you can find under .agents/skills/; (5) a one-line summary of EXP-001 including the code repository relative path and the git commit it binds to; (6) which files you read to answer, in order; (7) the recommended next action. Answer from workspace files only.'
```

只读参数：`-p`（非交互）+ `--permission-mode plan` + `--allowedTools Read,Glob,Grep` + `--output-format text`。

## 结果（成功那次）

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788361058 |
| 结束 epoch | 1788361095 |
| 耗时 | 37s |
| 运行前 `git status --porcelain` | 空 |
| 运行后 `git status --porcelain` | 空 |
| 是否越权写工作区文件 | 否（DIRTY=0，无需 checkout） |
| stdout 行数 | 7 |
| stderr 行数 | 0 |

## 原始 stdout

```text
1. This project is a Story-driven, file-as-memory autonomous research workspace; its current (MOCK) instance targets **lightweight flow-feature anomaly detection** for intrusion detection.
2. Story core idea: a lightweight classifier using only **three flow statistics** (duration, packet-count ratio, byte asymmetry) plus simple per-flow normalization can capture most attacks' main signal, with complex features adding only marginal benefit.
3. Work stands: **EXP-001 completed (MOCK)** — 3-feature LR beat the full-feature IF baseline on synthetic flows; **Story status = IN_PROGRESS**, active experiment = none, and the real CICIDS2017 subset remains unverified (Open Gap #1).
4. Skills under `.agents/skills/`: experiment-design, experiment-execution, experiment-review, framework-maintenance, literature-research, research-loop, research-memory, result-analysis, story-maintenance, workspace-resume.
5. EXP-001 = "Three-Feature vs Full-Feature Baseline" (3-feature logistic regression vs full-feature Isolation Forest on MOCK synthetic flows); code repo `../story-research-code` (entry `experiments/EXP-001/run.py`), bound to git commit `b0621e2ed266cc26020fac5b3295a588469bb495`.
6. Files read in order: `AGENTS.md` → `.research/PROJECT.md` → `.research/STORY.md` → `.research/STATE.md` → `.research/EXPERIMENTS.md` → `.research/RESOURCES.md`.
7. Recommended next action: rerun the EXP-001 protocol on a **real CICIDS2017 subset** to close Open Gap #1 (then design EXP-002 attack-family ablation), per STATE.md's recommended order.
```

## 原始 stderr

```text
(empty)
```

## 七项核对表

对照 stdout；第 (5) 项事实来源 `.research/EXPERIMENTS.md`。

| # | 项 | 判定 | 依据 |
|---|----|------|------|
| 1 | 项目是什么 | Y | Story-driven / file-as-memory 工作区；当前 MOCK 实例为轻量流特征异常检测。 |
| 2 | Story 核心想法 | Y | 三流统计（duration、packet-count ratio、byte asymmetry）+ 简单 per-flow 归一化即可捕获多数攻击主信号。 |
| 3 | 当前进展 | Y | EXP-001 completed (MOCK)；Story `IN_PROGRESS`；active experiment = none；真实 CICIDS 子集未验证。 |
| 4 | `.agents/skills/` 列表 | Y | 十项齐全，与目录 enumeration 一致。 |
| 5 | EXP-001 路径与 commit | Y | `../story-research-code` 与 `b0621e2ed266cc26020fac5b3295a588469bb495`。 |
| 6 | 阅读顺序 | Y | 自报 `AGENTS.md` → `.research/PROJECT.md` → `STORY.md` → `STATE.md` → `EXPERIMENTS.md` → `RESOURCES.md`。核心四文件顺序正确；`EXPERIMENTS.md` 为第 5 项所需；`RESOURCES.md` 为少量额外读取，不算大量加读。 |
| 7 | 推荐下一步 | Y | 真实 CICIDS2017 子集复跑 EXP-001 以关闭 Open Gap #1，与 `STATE.md` 一致。 |

**七项 Y 数:** 7/7

## 观察到的宿主限制

- `--print` 在非 TTY 下跳过 workspace trust dialog（help 已说明）。
- `--permission-mode plan` 可用，本次未写文件。
- `--allowedTools` 为 variadic：prompt 必须紧跟 `-p` 或放在 `--` 之后，否则会被吃掉并报 `--print` 缺 input。
- 成功运行 stderr 为空；未见 MCP 加载警告。
