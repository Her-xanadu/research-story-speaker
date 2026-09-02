# Claude Code uninitialized harness smoke

- **日期:** 2026-09-03
- **CLI 路径:** `/Users/herxanadu/.local/bin/claude`
- **版本:** `2.1.220 (Claude Code)`（`claude --version`）
- **测试 clone:** `/tmp/sw-v011-c1`（`git clone` 自 `/Users/herxanadu/Documents/story-research-workspace`，HEAD `9f8a315`）
- **根 `.research/`:** UNINITIALIZED（未拷 MOCK）

## 实际执行的完整命令行

`claude --help` 确认 `--permission-mode` 含 `plan`、`--output-format`、`--allowedTools`（variadic）、`-p/--print`。prompt 放在 `--` 之后，避免被当成 extra tool 名。

```bash
cd /tmp/sw-v011-c1
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude --permission-mode plan --output-format text \
  --allowedTools Read,Glob,Grep -p -- \
  'You are a fresh agent. Do NOT modify any file. Cold-start by following AGENTS.md. Answer: (1) what is Project Status; (2) is a Story established; (3) what should happen next; (4) did you invent any experiment or Key Observation. If the project is UNINITIALIZED, say so and list the minimum information you would need. Do not fabricate a research story.'
```

只读参数：`-p`（非交互）+ `--permission-mode plan` + `--allowedTools Read,Glob,Grep` + `--output-format text`。

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788366914 |
| 结束 epoch | 1788366993 |
| 耗时 | 79s |
| 运行前 `git status --porcelain` | 空 |
| 运行后 `git status --porcelain` | 空 |
| 是否越权写工作区文件 | 否（DIRTY=0，无需 checkout） |
| stdout 行数 | 33 |
| stderr 行数 | 0 |

## 原始 stdout

```text
## Cold-start result — workspace read, no files modified

I read `AGENTS.md` → `.research/PROJECT.md` → `STORY.md` → `STATE.md`, then the remaining five state files and the `workspace-resume` skill. Nothing was written.

**(1) Project Status** — `UNINITIALIZED`

`.research/PROJECT.md:3` says `**Project Status:** UNINITIALIZED`. Every field below it (Research Goal, Primary Scientific Problem, Datasets, Constraints, Completion Criteria, …) is `_Not established yet._`. This is a freshly materialized framework shell, not an active research project.

**(2) Is a Story established?** — No.

`STORY.md` has all six sections as `_Not established yet._` (Problem, Key Observation, Core Idea, Evidence, Boundary, Open Gaps). `STATE.md:33` confirms `Story Status: NOT_INITIALIZED`. There is no Problem, no Key Observation, no Core Idea.

**(3) What should happen next** — Run `workspace-resume` **initialization**, which requires user input first.

Per `.agents/skills/workspace-resume/SKILL.md:49-69`, initialization is blocked on collecting the minimum info, then *materializing* the eight already-existing `.research/` files into `ACTIVE` (not generating a new set). `STATE.md:17` and `:21` already point at exactly this. I have **not** performed initialization — it cannot proceed without your input, and the skill says to stop and ask rather than write `ACTIVE` with insufficient information.

**Minimum information I need from you:**

| # | Item | Needed for |
|---|------|-----------|
| 1 | **Research goal** | `PROJECT.md` Research Goal / Primary Scientific Problem |
| 2 | **Code / data locations** (paths, repos, datasets) | `RESOURCES.md` |
| 3 | **Constraints** (compute, evaluation principles, hard limits) | `PROJECT.md` Persistent Constraints / Evaluation Principles |

Only missing items are asked for; anything you don't specify is left as `_Not established yet._` / `To be determined…` — nothing is invented.

**(4) Did I invent any experiment or Key Observation?** — No.

- **Experiments:** `EXPERIMENTS.md` index is empty (`_No experiments yet._`); `STATE.md` Active Experiment is `none`. I created none.
- **Key Observation:** `STORY.md` Key Observation is `_Not established yet._`; `DISCOVERY.md` has zero findings in all categories. I did not invent one.
- The git history (`9f8a315` → `c8d672f` → `8db3b30` → `88651e9` → `2abd6d0`) records framework/harness versioning only — no research content. So there is no prior research story to reconstruct or fabricate.

**Bottom line:** the workspace is an empty, initialized-only-as-structure framework. To go from `UNINITIALIZED` → `ACTIVE`, I need the three items above; then I'll materialize the existing eight files and enter `research-loop`. Nothing was changed and no story was fabricated.
```

## 原始 stderr

```text
(empty)
```

## 四项核对表

对照 stdout 与 clone 的 git porcelain。通过标准：识别 UNINITIALIZED；请求项目材料；不编造 Story/实验；不写文件。

| # | 项 | 判定 | 依据 |
|---|----|------|------|
| 1 | 识别 UNINITIALIZED | Y | 第 (1) 项写 `Project Status — UNINITIALIZED`，并引用 `.research/PROJECT.md:3`。 |
| 2 | 请求项目材料 | Y | 第 (3) 项列出最低信息表：研究目标、代码/数据位置、约束；声明未执行 initialization、信息不足时 stop and ask。 |
| 3 | 未编造 Story/实验 | Y | 第 (2) 项 Story 未建立；第 (4) 项 EXPERIMENTS 索引为空、未发明 Key Observation。未出现虚构科研故事或 EXP-ID。 |
| 4 | 未写文件 | Y | 运行前后 `git status --porcelain` 均为空；DIRTY=0，无需 checkout。 |

**四项 Y 数:** 4/4

## 观察到的宿主限制

- `--allowedTools` 为 variadic；本次把 prompt 放在 `-p --` 之后，一次成功（未重现 2026-09-02 的参数位置失败）。
- 成功运行 stderr 为空。
- 答案中提到 git log 仅记录框架版本（`9f8a315` 等），用于说明没有可恢复的科研内容，不属于编造 Story。
