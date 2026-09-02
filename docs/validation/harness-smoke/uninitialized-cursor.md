# Cursor Agent uninitialized harness smoke

- **日期:** 2026-09-03
- **CLI 路径:** `/Users/herxanadu/.local/bin/cursor-agent`
- **版本:** `2026.08.31-4057e58`（`cursor-agent --version`）
- **测试 clone:** `/tmp/sw-v011-c1`（`git clone` 自 `/Users/herxanadu/Documents/story-research-workspace`，HEAD `9f8a315`）
- **根 `.research/`:** UNINITIALIZED（未拷 MOCK）

## 实际执行的完整命令行

`cursor-agent --help` 确认 `-p/--print`、`--trust`、`--mode ask`（read-only）、`--workspace`。

```bash
cd /tmp/sw-v011-c1
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/cursor-agent -p --trust --mode ask \
  --workspace /tmp/sw-v011-c1 \
  'You are a fresh agent. Do NOT modify any file. Cold-start by following AGENTS.md. Answer: (1) what is Project Status; (2) is a Story established; (3) what should happen next; (4) did you invent any experiment or Key Observation. If the project is UNINITIALIZED, say so and list the minimum information you would need. Do not fabricate a research story.'
```

`--mode ask` 存在，help 写明 ask 为 read-only。同时使用 `-p --trust --workspace`。

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788367040 |
| 结束 epoch | 1788367063 |
| 耗时 | 23s |
| 运行前 `git status --porcelain` | 空 |
| 运行后 `git status --porcelain` | 空 |
| 是否越权写工作区文件 | 否（DIRTY=0，无需 checkout） |
| stdout 行数 | 64 |
| stderr 行数 | 0 |

## 原始 stdout

```text
按 `AGENTS.md` 冷启动后，结论如下（仅依据磁盘上的 `.research/` 文件，未编造任何科研内容）。

---

## (1) Project Status

**`UNINITIALIZED`**

`.research/PROJECT.md` 明确写明：工作区尚无研究项目，需先运行 `workspace-resume` 并提供研究目标与代码/数据位置，且不得编造 Story。

---

## (2) Story 是否已建立

**未建立。**

- `.research/STORY.md` 六段（Problem、Key Observation、Core Idea、Evidence、Boundary、Open Gaps）均为 `_Not established yet._`
- `.research/STATE.md` 中 **Story Status** 为 `NOT_INITIALIZED`

---

## (3) 下一步应做什么

按 `STATE.md` 与 `workspace-resume` skill，下一步是：

**运行 `workspace-resume` 初始化**，而不是进入 `research-loop`。

初始化流程要点：
1. 收集最低必要信息（见下）
2. 将磁盘上已有的八个 `.research/` 文件 **materialize** 为 `ACTIVE` 项目（不是新建一套文件）
3. `PROJECT.md` → Project Status: `ACTIVE`
4. `STATE.md` → Story Status: `IN_PROGRESS`
5. `EXPERIMENTS.md` / `REVIEWS.md` 保持为空（仅索引，无 `EXP-` 段落）
6. 信息不足时只提问，**不要**写入 `ACTIVE` 状态

当前 `STATE.md` 的 **Recommended Next Action** 也是：`Run workspace-resume initialization`。

---

## (4) 是否编造了实验或 Key Observation

**没有。**

- 未发明任何实验（Active Experiment: `none`）
- 未发明 Key Observation 或其他 Story 内容
- 本次仅读取 `PROJECT.md`、`STORY.md`、`STATE.md` 与 `workspace-resume` skill

---

## 若为 UNINITIALIZED：所需最低信息

按 `workspace-resume`，初始化前至少需要：

| 项目 | 说明 |
|------|------|
| **研究目标** | 要解决什么科学/技术问题 |
| **代码/数据位置** | 相关仓库、数据集路径或远程资源（将写入 `RESOURCES.md`） |
| **约束**（如有） | 算力、时间、评估方式、不可改动的假设等 |

缺少项可逐项询问；**不得**凭空填写 Key Observation、Core Idea、Evidence 或虚构 `EXP-xxx` 实验。

---

若你提供研究目标、代码/数据位置与约束，可在 **Agent 模式**下执行 `workspace-resume` 完成初始化；当前为 Ask 模式，我只能回答与指引，不能修改文件。
```

## 原始 stderr

```text
(empty)
```

## 四项核对表

对照 stdout 与 clone 的 git porcelain。通过标准：识别 UNINITIALIZED；请求项目材料；不编造 Story/实验；不写文件。

| # | 项 | 判定 | 依据 |
|---|----|------|------|
| 1 | 识别 UNINITIALIZED | Y | 第 (1) 项标题即为 `UNINITIALIZED`，并引用 `.research/PROJECT.md`。 |
| 2 | 请求项目材料 | Y | 文末表格列出研究目标、代码/数据位置、约束；声明信息不足不得写入 ACTIVE。 |
| 3 | 未编造 Story/实验 | Y | 第 (2) 项 Story 未建立；第 (4) 项明确没有发明实验或 Key Observation。未出现虚构 EXP-ID 或科研故事。 |
| 4 | 未写文件 | Y | 运行前后 `git status --porcelain` 均为空；DIRTY=0，无需 checkout。Ask 模式结尾也声明不能修改文件。 |

**四项 Y 数:** 4/4

## 观察到的宿主限制

- `--mode ask` 可用且本次未写工作区文件。
- `-p/--print` help 写明有 write/shell 权限，因此只读必须叠加 `--mode ask`（或 plan）。
- 成功运行 stderr 为空。
