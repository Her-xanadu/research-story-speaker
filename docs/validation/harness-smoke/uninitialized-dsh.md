# DeepSeek Harness (dsh) uninitialized harness smoke

- **日期:** 2026-09-03
- **CLI 路径:** `/Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh`
- **版本:** `0.1.0-rc.7`（`dsh --version`）
- **测试 clone:** `/tmp/sw-v011-c1`（`git clone` 自 `/Users/herxanadu/Documents/story-research-workspace`，HEAD `9f8a315`）
- **根 `.research/`:** UNINITIALIZED（未拷 MOCK）

## 实际执行的完整命令行

`dsh --help` 与 `dsh --profile headless --help` 均无只读 / no-edit / sandbox 旗标；headless 的 `--help` 只有 `-h`。本次仅 `--profile headless`，只读靠 prompt 约束，并用 git porcelain 复核。

```bash
cd /tmp/sw-v011-c1
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh --profile headless \
  'You are a fresh agent. Do NOT modify any file. Cold-start by following AGENTS.md. Answer: (1) what is Project Status; (2) is a Story established; (3) what should happen next; (4) did you invent any experiment or Key Observation. If the project is UNINITIALIZED, say so and list the minimum information you would need. Do not fabricate a research story.'
```

headless 无只读旗标；只读靠 prompt + porcelain。

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788367002 |
| 结束 epoch | 1788367032 |
| 耗时 | 30s |
| 运行前 `git status --porcelain` | 空 |
| 运行后 `git status --porcelain` | 空 |
| 是否越权写工作区文件 | 否（DIRTY=0，无需 checkout） |
| stdout 行数 | 31 |
| stderr 行数 | 0 |

## 原始 stdout

```text
Cold-start complete — read `AGENTS.md`, then the boot set (`PROJECT.md` → `STORY.md` → `STATE.md`) plus the remaining five `.research/` files and the `workspace-resume` skill. **No files were modified.**

## Answers

**1. Project Status: `UNINITIALIZED`**
`.research/PROJECT.md` line 3 states `Project Status: UNINITIALIZED`, with every field (`Research Goal`, `Primary Scientific Problem`, `Target Performance Direction`, `Key Datasets`, `Evaluation Principles`, `Persistent Constraints`, `Story Completion Criteria`) set to "_Not established yet._" / "_None yet._". README confirms this repo is framework v0.1.1 whose root `.research/` is intentionally shipped as a clean UNINITIALIZED template.

**2. Is a Story established? No.**
`STORY.md` has all six segments (`Problem`, `Key Observation`, `Core Idea`, `Evidence`, `Boundary`, `Open Gaps`) as "_Not established yet._". `STATE.md` reports Story Status `NOT_INITIALIZED`. Consistent with that, `DISCOVERY.md`, `EXPERIMENTS.md` (index empty, no EXP-ID), `LITERATURE.md`, `REVIEWS.md`, and `RESOURCES.md` (no codebases/datasets/compute) are all empty placeholders. There is no research story, no experiment history, and no discovery content anywhere in `.research/`.

**3. What should happen next**
The initialization protocol per `workspace-resume` (the AGENTS.md-mandated route for UNINITIALIZED):

1. **You provide the minimum information** (below). If any item is missing, I ask only for that item — nothing is invented.
2. **Materialize** the eight `.research/` files already on disk (not a new file set) into an `ACTIVE` project; `EXPERIMENTS.md`/`REVIEWS.md` stay empty (index only).
3. Project Status → `ACTIVE`; STATE Story Status → `IN_PROGRESS`; missing Story segments remain "_Not established yet._" or explicit "Current hypothesis …" — never fabricated.
4. Suggest a workspace git commit, then enter the first `research-loop` in the same turn.

I did not perform step 2 because you instructed me not to modify any file.

**4. Did you invent any experiment or Key Observation? No.**
Nothing above was fabricated: no EXP-ID, no Key Observation/Core Idea/Evidence claims, no experiment or discovery content was created or assumed. Everything reported comes verbatim from workspace files.

## Minimum information needed to initialize

- **Research goal** — the one-sentence scientific objective to pursue.
- **Primary scientific problem** — the core question.
- **Code/data locations** — which codebase(s) and dataset(s), and the workspace mode: code inside this workspace (A), sibling repo (B), or remote (C).
- **Persistent constraints** — compute, data access, ethics, time limits, etc.

Useful (optional) to fill the PROJECT template well: target performance direction (qualitative is fine), evaluation principles, story completion criteria, and domain background.
```

## 原始 stderr

```text
(empty)
```

## 四项核对表

对照 stdout 与 clone 的 git porcelain。通过标准：识别 UNINITIALIZED；请求项目材料；不编造 Story/实验；不写文件。

| # | 项 | 判定 | 依据 |
|---|----|------|------|
| 1 | 识别 UNINITIALIZED | Y | 第 1 项写 `Project Status: UNINITIALIZED`，并引用 `.research/PROJECT.md` 第 3 行。 |
| 2 | 请求项目材料 | Y | 文末列出最低信息：研究目标、主科学问题、代码/数据位置与 workspace mode、持久约束。声明因用户禁止改文件而未 materialize。 |
| 3 | 未编造 Story/实验 | Y | 第 2 项 Story 未建立；第 4 项明确 No，无 EXP-ID、无 Key Observation。未出现虚构科研内容。 |
| 4 | 未写文件 | Y | 运行前后 `git status --porcelain` 均为空；DIRTY=0，无需 checkout。 |

**四项 Y 数:** 4/4

## 观察到的宿主限制

- `--profile headless` 是非交互 one-shot 的正确入口。
- headless `--help` 无只读/no-edit 参数；本次未改工作区文件，但不能从 CLI 旗标上强制只读。
- 成功运行 stderr 为空；未见 MCP 加载警告。
