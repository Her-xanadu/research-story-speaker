# DeepSeek Harness (dsh) harness smoke

- **日期:** 2026-09-02
- **CLI 路径:** `/Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh`
- **版本:** `0.1.0-rc.7`（`dsh --version`）

## 实际执行的完整命令行

`dsh --help` 与 `dsh --profile headless --help` 均无只读 / no-edit / sandbox 旗标；headless 的 `--help` 只有 `-h`。本次仅 `--profile headless`，只读靠 prompt 约束，并用 git porcelain 复核。

```bash
cd /Users/herxanadu/Documents/story-research-workspace
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh --profile headless \
  'You are a fresh agent with no chat history. Do NOT modify any file. Cold-start this workspace by following AGENTS.md, then answer in exactly 7 numbered lines: (1) what this project is; (2) the current Story core idea in one sentence; (3) where the work stands now (active/completed experiments, Story status); (4) the list of skills you can find under .agents/skills/; (5) a one-line summary of EXP-001 including the code repository relative path and the git commit it binds to; (6) which files you read to answer, in order; (7) the recommended next action. Answer from workspace files only.'
```

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788361097 |
| 结束 epoch | 1788361113 |
| 耗时 | 16s |
| 运行前 `git status --porcelain` | 空 |
| 运行后 `git status --porcelain` | 空 |
| 是否越权写工作区文件 | 否（DIRTY=0，无需 checkout） |
| stdout 行数 | 13（7 项之间有空行） |
| stderr 行数 | 0 |

## 原始 stdout

```text
1. **Project:** This is a Story-driven research workspace (file-as-memory control plane); its current `.research/` is a **MOCK** instance simulating the project "Lightweight Flow-Feature Anomaly Detection" — testing whether a minimal set of flow statistics can rival deep/full-feature IDS detectors on standard benchmarks (CICIDS2017, optional UNSW-NB15).

2. **Current Story core idea:** One classifier using only three flow statistics — flow duration, packet-ratio, byte-asymmetry — with simple per-flow normalization captures most common attack signal, so complex/full-feature models add mostly marginal gain.

3. **Where work stands:** EXP-001 (Three-Feature vs Full-Feature Baseline) is **completed on MOCK synthetic flows** with method-review and result-review done (REVIEWS.md); no active/planned experiment is running; Story status `IN_PROGRESS` — real CICIDS2017 subset validation is still open (Open Gap #1).

4. **Skills under `.agents/skills/`:** experiment-design, experiment-execution, experiment-review, framework-maintenance, literature-research, research-loop, research-memory, result-analysis, story-maintenance, workspace-resume.

5. **EXP-001 summary:** On MOCK synthetic flows, 3-feature logistic regression beat the full-feature Isolation Forest baseline (test F1 1.0000 vs 0.5455, gap −45.45 pp) — code repository at `../story-research-code`, bound to commit `b0621e2ed266cc26020fac5b3295a588469bb495` (entry `experiments/EXP-001/run.py`).

6. **Files read (in order):** `AGENTS.md` (start-here routing), `.research/PROJECT.md`, `.research/STORY.md`, `.research/STATE.md`, glob of `.agents/skills/*/SKILL.md`, `.research/EXPERIMENTS.md`, `.research/RESOURCES.md`.

7. **Recommended next action:** Re-run the EXP-001 protocol on a real CICIDS2017 subset against an effective full-feature baseline (Open Gap #1, per STATE) — i.e., plan/execute the real-data EXP-001 follow-up before touching EXP-002 attack-family ablation.
```

## 原始 stderr

```text
(empty)
```

## 七项核对表

对照 stdout；第 (5) 项事实来源 `.research/EXPERIMENTS.md`。

| # | 项 | 判定 | 依据 |
|---|----|------|------|
| 1 | 项目是什么 | Y | Story-driven / file-as-memory 控制平面；当前 `.research/` 为 MOCK「Lightweight Flow-Feature Anomaly Detection」。 |
| 2 | Story 核心想法 | Y | 三流统计（duration、packet-ratio、byte-asymmetry）+ per-flow 归一化捕获多数攻击主信号。 |
| 3 | 当前进展 | Y | EXP-001 MOCK 已完成且 review done；无活跃实验；Story `IN_PROGRESS`；Open Gap #1 仍开放。 |
| 4 | `.agents/skills/` 列表 | Y | 十项齐全，与目录 enumeration 一致。 |
| 5 | EXP-001 路径与 commit | Y | 出现 `../story-research-code` 与 `b0621e2ed266cc26020fac5b3295a588469bb495`。答案还夹带了 F1 数字（来自 EXPERIMENTS.md），但不影响路径/commit 核对。 |
| 6 | 阅读顺序 | Y | 自报 `AGENTS.md` → `PROJECT.md` → `STORY.md` → `STATE.md`，随后 glob `.agents/skills/*/SKILL.md`、`EXPERIMENTS.md`、`RESOURCES.md`。核心四文件顺序正确；skills glob 服务第 4 项；EXPERIMENTS 服务第 5 项；RESOURCES 为少量额外读取。 |
| 7 | 推荐下一步 | Y | 真实 CICIDS2017 子集复跑 EXP-001（Open Gap #1），然后再考虑 EXP-002，与 `STATE.md` 一致。 |

**七项 Y 数:** 7/7

## 观察到的宿主限制

- `--profile headless` 是非交互 one-shot 的正确入口（与 adapters 既有说明一致）。
- headless `--help` 无只读/no-edit 参数；本次未改工作区文件，但不能从 CLI 旗标上强制只读。
- 成功运行 stderr 为空；未见 MCP 加载警告。
