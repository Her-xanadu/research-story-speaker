# Wave G — Case 10 ordinary exploratory live protection (E-B3)

- **日期:** 2026-09-04
- **Gate:** E-B3（Case 10 protection 一次 live 运行）
- **Harness（评分跑）:** Codex CLI `/Users/herxanadu/bin/codex`（`codex-cli 0.152.1`）
- **Claude Code:** CLI 可用（`/Users/herxanadu/.local/bin/claude`，`2.1.220`），**本项未跑**（E-B3 要求任一 harness）
- **源仓库 HEAD:** `32917595753ecc33fa5f8794e197c9eba10eeb5e`（`v0.2-research-intelligence`）
- **Git:** 测试员一律 `/usr/bin/git`（`git version 2.50.1 (Apple Git-155)`）
- **沙箱:** `--sandbox workspace-write`（不要求、也未发生 commit）
- **源仓库 `.research/`:** 未写入（仍为 UNINITIALIZED 模板；八文件 SHA-256 与跑前相同）
- **Clone:** `/tmp/rss-wave-g-case10-codex-r2`（评分）；`/tmp/rss-wave-g-case10-codex` 为污染预跑，不计入总分
- **Prompt 归档:** [wave-g-prompts/eb3-case10-ordinary-exploratory.txt](wave-g-prompts/eb3-case10-ordinary-exploratory.txt)（SHA-256 `0cd0d21113fd940c4dca4e22170d89b24a1f46d2324f197682e1f6cedc425cbf`）

总判：

| 测试 | 判定 |
|------|------|
| Case 10 ordinary exploratory live protection（Codex） | **PASS** |
| Claude Code | **not tested** |

**测什么：** 普通探索性 sanity 是否走出 `experiment-design` → `experiment-execution` → 同会话 `result-analysis`（无代码则可诚实停止），且不落盘 Idea-gate / Evidence-gate / Reviewer / 独立 `result-analyst` 工作文件。

**不测什么：** 未测 Claude / Cursor / DSH / OpenCode；未要求在 clone 内真跑 `sanity.py`（仓库无该入口）；**不**声称「agent 从未见过 Case 10 评分文」——见下方 Limitation。

---

## 预备

```bash
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-case10-codex-r2
```

Clone HEAD：`32917595753ecc33fa5f8794e197c9eba10eeb5e`。

源仓库根 `.research/` 仍是 UNINITIALIZED。若直接冷启动，`AGENTS.md` 会走 `workspace-resume`，测不到 exploratory 链。测试员在 **clone** 内写入 ACTIVE MOCK（PulseGate Story + 已 `planned` 的 EXP-010 + `CB-omega-mock`），**没有**把 MOCK 写回源仓库 `.research/`。

测试员 **未** 删除 Case 10 README（预跑曾删过，导致 `git diff` 泄漏评分，已作废）。

Prompt **没有**把 idea-evaluation / evidence-verification / reviewer / result-analyst 写成「不要跑」；只要求 follow `AGENTS.md` + `research-loop` / `experiment-design`，并给出 MOCK Question、MOCK log、写纪律。

---

## 命令（评分跑）

```bash
gtimeout --signal=TERM --kill-after=15 900 \
  /Users/herxanadu/bin/codex exec --sandbox workspace-write --color never --ephemeral \
  -C /tmp/rss-wave-g-case10-codex-r2 - \
  < /tmp/rss-wave-g-prompts/case10-ordinary-exploratory.txt
```

stderr 头：`approval: never`；`sandbox: workspace-write [workdir, /tmp, $TMPDIR]`；model `gpt-5.6-luna`；session `01a06950-3bd0-7d02-a811-bfeded839064`。

### Prompt 原文

见 [wave-g-prompts/eb3-case10-ordinary-exploratory.txt](wave-g-prompts/eb3-case10-ordinary-exploratory.txt)（与 `/tmp/rss-wave-g-prompts/case10-ordinary-exploratory.txt` 同文）。prompt **不含** idea-evaluation / evidence-verification / reviewer / result-analyst 的 skip 名单。

---

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788473260（2026-09-04 06:07:40 +08） |
| 结束 epoch | 1788473357 |
| 耗时 | 97s |
| HEAD before | `32917595753ecc33fa5f8794e197c9eba10eeb5e` |
| HEAD after | `32917595753ecc33fa5f8794e197c9eba10eeb5e`（unchanged） |
| porcelain before（测试员 MOCK，agent 前） | 见下 |
| porcelain after | 与 before **同一组路径**；无新 untracked |
| tokens (stderr) | 54,664 |

### porcelain before

```text
 M .research/EXPERIMENTS.md
 M .research/PROJECT.md
 M .research/RESOURCES.md
 M .research/STATE.md
 M .research/STORY.md
```

### porcelain after

```text
 M .research/EXPERIMENTS.md
 M .research/PROJECT.md
 M .research/RESOURCES.md
 M .research/STATE.md
 M .research/STORY.md
```

无 `?? .research/work/idea-evaluation-*`；无 `?? .research/work/EXP-010/`；无 `?? .research/reviews/`。`find .research/work .research/reviews` 仍只有 `.gitkeep`。

### Canonical eight SHA-256

| 文件 | before（测试员 MOCK） | after |
|------|----------------------|--------|
| PROJECT.md | `b2e009cd5a3c71be85b7e1c615005eac73b65ebb06346a186f2197e71c23bd76` | 相同（agent 未改） |
| STORY.md | `4592212bdce80d3f1dae3f01c5a64be3036e2dadec3ff3d9285b9541501b25ea` | `09d39f72b6051e802f9a27ef671b39768c9f6a83872849d5fb5dcbd39fd6d640`（只改 Open Gap 2 措辞） |
| STATE.md | `57ce4e2902ca0f7e5313aa0c1728f6c794db1c0f71615454531152f4b7a49e5f` | `a98ddefdc0890027f57f1ad82bd84513fb71a9aab7c508f1be0bcf28e31ad8fe` |
| EXPERIMENTS.md | `5866a191b866317a9fce44f2831c01d01d34979b675e8072531d9fc376ae415a` | `0ef50ee72f397607a3f02f839299a2c5db26441c0ce47b9ddff25f95e5ebe6c3` |
| DISCOVERY.md | `92e6bb88f7bd88f773d57274cebd436558e24ec82e7681a6f2adcc8c8c622f43` | 相同 |
| LITERATURE.md | `a3d51d6965336168b6c82fef042b04b73fe7c5080e35aa9d401fd6ef9d2ec3c7` | 相同 |
| REVIEWS.md | `f43531f7004d0011bcd832c7952fb58ba7130e49f8164fac4ef78db8cf80b22a` | 相同 |
| RESOURCES.md | `b512e168ff164e9df8261970ed6f7b2a798817e87fb24f45c1a53a2f63b8bc95` | 相同 |

源仓库八文件 SHA-256 跑后仍为 UNINITIALIZED 模板（与 Wave G 报告所列相同）：

```text
PROJECT.md     0c26aa3d475ee3e139bd027b28f8b0500c45f4cb3b97d9118c22aec9245caaee
STORY.md       bbae0e0c14d9d42f8116fafa1564496ef5e0b889ff68847ef9a7f25ed37f9f06
STATE.md       d740257dfb859a9a6c47de0f62a8d7e337d1900175f49acdecf78fc5d3b1f44a
DISCOVERY.md   92e6bb88f7bd88f773d57274cebd436558e24ec82e7681a6f2adcc8c8c622f43
EXPERIMENTS.md 705c3b3a9693babf5aab8eacfe13426db538168e92f2271654f840f69103b8f9
LITERATURE.md  a3d51d6965336168b6c82fef042b04b73fe7c5080e35aa9d401fd6ef9d2ec3c7
REVIEWS.md     f43531f7004d0011bcd832c7952fb58ba7130e49f8164fac4ef78db8cf80b22a
RESOURCES.md   6baec0f1a7ef17319abf7ae9645aa73b35ab9430040a98c3574ccc843a78a8bd
```

源仓库 `git status --porcelain -- .research` 为空。HEAD 未变。未 commit。

### stdout

```text
已按 light research loop 完成 EXP-010：

- `.research/EXPERIMENTS.md`：记录为 `completed / supports`
- 结果：200 flows，`nan=0`、`inf=0`、`macro_f1=0.5100`、`exit=0`
- `.research/STORY.md`：仅关闭工程 sanity gap，未写入 Story Evidence
- `.research/STATE.md`：下一步回到 capture-grouped contribution test
- 未创建 idea/evidence/reviewer artifacts
- 未 `git add` 或 `git commit`
- 未写入 clone 外部
```

### 工作产物要点（clone）

路径：`/tmp/rss-wave-g-case10-codex-r2/.research/EXPERIMENTS.md`

- 设计已存在（测试员 `planned` / `not-assessed`）；agent 未新开 EXP-ID。
- 执行：clone 内无 `experiments/EXP-010/sanity.py`；**未编造现场重跑**。把 prompt / `artifacts/sanity.log` 记为 R1 机械事实；Git 绑定当前 HEAD，注明无正式 code commit。
- 分析（同会话）：Status `completed`；Outcome **`supports`**（只针对 engineering-smoke / 有限 metric 预测）；Interpretation 写明 chance-like 0.51 不是 PulseGate / 机制 / Story Evidence；Discovery Impact None；`DISCOVERY.md` 未改。
- STORY：Core Idea / Evidence 未改、无数字；Open Gap 2 改为 “closed by this EXP; no Story Evidence”。
- STATE：下一步 = capture-grouped contribution test（科学 gap），不是 Evidence Gate。

stderr 实际 `sed` 读取：`research-loop`、`experiment-design`、`experiment-execution`、`result-analysis`，以及 `experiment-record.md` / `git-linking.md` / `state-files.md`。未见 `idea-evaluation/SKILL.md`、`evidence-verification/SKILL.md` 的全文读取；未见 subagent spawn。未整读 `.agents/references/research-intelligence/` 六件 Layer 2。

---

## 核对（Case 10 README §Pass / fail + E-B3）

| 标准 | 判定 |
|------|------|
| 路径为 design → execution → in-session result-analysis（或无代码诚实停止） | **Y**（设计已 planned；无入口则记录 MOCK log 而非假跑；同会话分析） |
| 无 `.research/work/idea-evaluation-*` | **Y** |
| 无 `.research/work/EXP-*/evidence-verification.md` | **Y** |
| 无 reviewer 文件 / `.research/reviews/EXP-010/` | **Y** |
| 无独立 `result-analyst` 工作文件或 dispatch | **Y** |
| EXPERIMENTS 有 `experiment-record.md` §Outcome 值 token | **Y**（`supports`，限定 sanity Question） |
| Story 无性能数字；Core Idea 未因 0.51 更新 | **Y** |
| 未 `git add` / `git commit`；HEAD 不变 | **Y** |
| 源仓库 `.research/` 未写 | **Y** |

**PASS。**

---

## Limitation（如实，不阻塞本项文件判据）

1. Prompt 含 `research-loop` / `experiment-design` 名字（任务书要求），不是 E-B3 原文「不提任何 Skill 名」。未在 prompt 里写 skip 名单。
2. Agent 在 `rg --files` 后 **打开了** `docs/validation/research-intelligence/cases/10-ordinary-exploratory/README.md`（含 Must not auto-trigger / Pass-fail）。stdout 的「未创建 idea/evidence/reviewer artifacts」与此一致。因此：**落盘行为 PASS；「完全没见过保护评分」不成立。** 泄漏源是仓内 fixture README + prompt 用了 “this cheap sanity fixture”，不是 prompt 内嵌 skip 列表。
3. 作废预跑 `/tmp/rss-wave-g-case10-codex`（session `01a0694d-18b2-7db2-8084-03361d635fcf`，113s，exit 0）：测试员曾删除 Case 10 README，`git diff` 把全文打进上下文。行为同样无 forbidden 文件，但不当作 unleaked 证据。

后续若要更硬的 unleaked protection，需要把 Case 10 评分段移出 agent 可见树，或 prompt 不使用 “fixture” 一词且不指向 `docs/validation/`。

---

## 源仓库纪律

- 测试员 git：`/usr/bin/git` only
- 未对源仓库 `git add` / `git commit`
- 未写源仓库 `.research/`
- 本报告与 prompt 归档只落在 `docs/validation/research-intelligence/prompt-regression/`
