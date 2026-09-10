# Codex uninitialized harness smoke

- **日期:** 2026-09-03
- **CLI 路径:** `/Users/herxanadu/bin/codex`
- **版本:** `codex-cli 0.151.0-alpha.7.2`（`codex --version`）
- **测试 clone:** `/tmp/sw-v011-c1`（`git clone` 自 `/Users/herxanadu/Documents/story-research-workspace`，HEAD `9f8a315`）
- **根 `.research/`:** UNINITIALIZED（未拷 MOCK）

## 实际执行的完整命令行

`codex exec --help` 确认 `-C/--cd`、`--sandbox read-only`、`--color`、`--ephemeral` 均存在。本次使用 `--sandbox read-only`；日志显示 `approval: never`。

```bash
cd /tmp/sw-v011-c1
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/bin/codex exec --sandbox read-only --color never --ephemeral \
  -C /tmp/sw-v011-c1 \
  'You are a fresh agent. Do NOT modify any file. Cold-start by following AGENTS.md. Answer: (1) what is Project Status; (2) is a Story established; (3) what should happen next; (4) did you invent any experiment or Key Observation. If the project is UNINITIALIZED, say so and list the minimum information you would need. Do not fabricate a research story.'
```

只读参数：`--sandbox read-only` + `--ephemeral` + `--color never`。

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788366861 |
| 结束 epoch | 1788366903 |
| 耗时 | 42s |
| 运行前 `git status --porcelain` | 空 |
| 运行后 `git status --porcelain` | 空 |
| 是否越权写工作区文件 | 否（DIRTY=0，无需 checkout） |
| stdout 行数 | 16 |
| stderr 行数 | 451 |

## 原始 stdout

```text
1. **Project Status：** `UNINITIALIZED`。  
2. **Story 是否建立：** 没有。`STORY.md` 六个部分均为 `_Not established yet._`。  
3. **下一步：** 先完成 `workspace-resume` 初始化，需要提供：

   - 研究目标
   - 代码位置
   - 数据位置
   - 约束条件（如计算、数据、评估或安全限制）

   在此之前不要进入 `research-loop`，也不要创建实验。

4. **是否发明了 Experiment 或 Key Observation：** 没有。当前 `Active Experiment` 为 `none`，没有已完成实验；我没有补写任何 Story、Key Observation 或实验。

本轮仅读取文件，未修改任何内容。
```

## 原始 stderr
stderr 共 451 行，超过 300，下方截前 200 + 后 50。


```text
Reading additional input from stdin...
OpenAI Codex v0.151.0-alpha.7.2
--------
workdir: /tmp/sw-v011-c1
model: gpt-5.6-luna
provider: openai
approval: never
sandbox: read-only
reasoning effort: medium
reasoning summaries: none
session id: 01a062f8-b62c-7a23-9da0-f93ecc82fc7d
--------
user
You are a fresh agent. Do NOT modify any file. Cold-start by following AGENTS.md. Answer: (1) what is Project Status; (2) is a Story established; (3) what should happen next; (4) did you invent any experiment or Key Observation. If the project is UNINITIALIZED, say so and list the minimum information you would need. Do not fabricate a research story.
warning: Under-development features enabled: chronicle. Under-development features are incomplete and may behave unpredictably. To suppress this warning, set `suppress_unstable_features_warning = true` in /Users/herxanadu/.codex/config.toml.
warning: clamping SessionEnd hook timeout to 3s in /Users/herxanadu/.codex/plugins/cache/openai-codex/codex/1.0.6/hooks/hooks.json
2026-09-02T16:34:23.310092Z ERROR codex_api::endpoint::responses_websocket: failed to connect to websocket: HTTP error: 426 Upgrade Required, url: ws://127.0.0.1:10100/v1/responses
hook: SessionStart
hook: SessionStart Completed
codex
我会按 `workspace-resume` 的冷启动协议先读取工作区状态与对应技能说明；本轮只读，不会修改任何文件，也不会补写研究故事或实验。
exec
/bin/zsh -lc "sed -n '1,240p' /Users/herxanadu/.agents/skills/workspace-resume/SKILL.md && printf '\\n--- MEMORY MATCHES ---

Host Codex memory search returned unrelated private-workspace hits
(other local projects, absolute home paths, and science-direction notes).
Those excerpts are **redacted** from this repository. They were not
research-story-speaker / Case 10 / PulseGate contamination of this
UNINITIALIZED clone run.

The smoke log continues with the clone's template `.research/STATE.md`:

## Active Experiment

none

## Recently Completed

_None yet._

## Recommended Next Action

Run workspace-resume initialization.

## Blockers

- Project Status is UNINITIALIZED. Provide research goal and code/data locations.

## Key Files

| 用途 | 路径 |
|------|------|
| Project | `.research/PROJECT.md` |
| Story | `.research/STORY.md` |
| Templates | `.agents/templates/` |

## Story Status

`NOT_INITIALIZED`

codex
1. **Project Status：** `UNINITIALIZED`。  
2. **Story 是否建立：** 没有。`STORY.md` 六个部分均为 `_Not established yet._`。  
3. **下一步：** 先完成 `workspace-resume` 初始化，需要提供：

   - 研究目标
   - 代码位置
   - 数据位置
   - 约束条件（如计算、数据、评估或安全限制）

   在此之前不要进入 `research-loop`，也不要创建实验。

4. **是否发明了 Experiment 或 Key Observation：** 没有。当前 `Active Experiment` 为 `none`，没有已完成实验；我没有补写任何 Story、Key Observation 或实验。

本轮仅读取文件，未修改任何内容。


hook: Stop
hook: Stop Completed
tokens used
32,995
```

## 四项核对表

对照 stdout 与 clone 的 git porcelain。通过标准：识别 UNINITIALIZED；请求项目材料；不编造 Story/实验；不写文件。

| # | 项 | 判定 | 依据 |
|---|----|------|------|
| 1 | 识别 UNINITIALIZED | Y | stdout 第 (1) 项明确写 `Project Status：UNINITIALIZED`。 |
| 2 | 请求项目材料 | Y | 第 (3) 项列出最低信息：研究目标、代码位置、数据位置、约束条件；并声明初始化前不要进入 research-loop。 |
| 3 | 未编造 Story/实验 | Y | 第 (2) 项写明 Story 未建立、六段均为 `_Not established yet._`；第 (4) 项声明没有补写 Experiment 或 Key Observation。未出现虚构 EXP-ID、CICIDS、流特征等 MOCK 内容。 |
| 4 | 未写文件 | Y | 运行前后 `git status --porcelain` 均为空；DIRTY=0，无需 checkout。 |

**四项 Y 数:** 4/4

## 观察到的宿主限制

- `codex exec --help` 含 `-C/--cd <DIR>` 与 `--sandbox read-only`；`--sandbox read-only` 时 stderr 头部为 `approval: never` / `sandbox: read-only`。
- stderr：`chronicle` under-development 特性警告；SessionEnd hook timeout 被 clamp 到 3s。
- stderr ERROR：`failed to connect to websocket: HTTP error: 426 Upgrade Required, url: ws://127.0.0.1:10100/v1/responses`（未导致本次非零退出）。
- 首次误读 `/Users/herxanadu/.agents/skills/workspace-resume/SKILL.md`（不存在），随即改读 clone 内 `.agents/skills/workspace-resume/SKILL.md`；最终答案仍正确识别 UNINITIALIZED。
- `--ephemeral` 避免会话文件落盘；clone 工作区 git 前后均为空。
