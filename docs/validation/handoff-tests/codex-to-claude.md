# Wave C4 — Codex → Claude 跨 Harness 交接

- **日期:** 2026-09-03
- **同一 clone:** `/tmp/sw-v011-c2`（C2 之后继续；源仓库 HEAD 仍为 `9f8a315`）
- **判定:** **PASS**（EXP-ID = `EXP-001`；Reviewer **未越权**；commit A/B 分离）

前置：C2 已把 Project Status 置为 `ACTIVE`，但 `workspace-write` sandbox 未能 `git commit`（见 [`init-codex.md`](init-codex.md)）。因此 commit A 同时带上了未提交的初始化 diff。这不影响 C4 的 EXP-001 / Reviewer / canonical-Main 规则。

## Harness 调用总表

| 步 | 角色 | CLI | 退出码 | 耗时 | 写权限 |
|----|------|-----|--------|------|--------|
| 1 | Codex Main（experiment-design + commit A） | `codex exec --dangerously-bypass-approvals-and-sandbox` | 0 | 95s | clone 可写；git 成功 |
| 2 | Claude Main 冷启动 | `claude -p` + `--allowedTools Read,Glob,Grep` | 0 | 18s | 只读（未用 `--permission-mode plan`） |
| 3 | Claude Reviewer（fresh `-p`） | `claude -p --permission-mode acceptEdits` + Write/Edit | 0 | 81s | 仅允许写文件；无 Bash / 无 commit |
| 4 | Claude Main 整合 + commit B | `claude -p --permission-mode bypassPermissions` + Bash | 0 | 302s | 可写 + git |

PATH `git` 为 2.15；所有 git 命令使用 `/usr/bin/git`（2.50.1）。未使用 Cursor。

---

## 1. Codex Main — 设计 EXP-001（commit A）

### 命令

C2 的 `workspace-write` 无法创建 `.git/index.lock`，本步改用 `--dangerously-bypass-approvals-and-sandbox`（stderr 记录 `sandbox: danger-full-access`）。

```bash
export PATH="/usr/bin:$PATH"
cd /tmp/sw-v011-c2
gtimeout --signal=TERM --kill-after=15 900 \
  /Users/herxanadu/bin/codex exec \
    --dangerously-bypass-approvals-and-sandbox \
    --color never \
    --ephemeral \
    -C /tmp/sw-v011-c2 \
    --add-dir /tmp/sw-v011-c2-code \
    -o /tmp/sw-v011-c2-logs/c4-codex-last-message.txt \
    "$(cat /tmp/sw-v011-c2-logs/c4-codex-prompt.txt)"
```

Prompt 要求：按 `experiment-design` 写 **EXP-001**（禁止 EXP-002）；`Status=planned`、`Outcome=not-assessed`（词表仅 `experiment-record.md`）；更新 STATE Current Focus / Next；`/usr/bin/git commit` = commit A；不跑代码、不写 reviews。

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始/结束 epoch | 1788367147 / 1788367242 |
| 模型 | gpt-5.6-luna |
| session id | 01a062fd-139b-77a1-ad00-bb9da2e4e0c2 |
| commit A | `8976b83` / `8976b83e9927bde7f54f1874d6d75ad4d4a215dd` |
| 运行后 porcelain | 空 |

### Codex 原始输出摘要

```text
已完成 EXP-001 设计并提交。

- EXP-ID：`EXP-001`
- Status：`planned`
- Outcome：`not-assessed`
- Commit A：`8976b83e9927bde7f54f1874d6d75ad4d4a215dd`
- 未运行代码、未创建 Review、未创建 EXP-002
- 工作区当前干净
```

stderr 头：`OpenAI Codex v0.151.0-alpha.7.2`；`approval: never`；`sandbox: danger-full-access`。可见 chronicle 警告与 websocket 426（与 smoke 相同，未阻塞）。

### commit A `git log -p` 摘要

```text
8976b83 Design EXP-001 checksum screening experiment
  .research/DISCOVERY.md   |  2 +-
  .research/EXPERIMENTS.md | 44 +++++++++++++++++++++++++++++++++++++++++++-
  .research/PROJECT.md     | 30 +++++++++++++++++++-----------
  .research/RESOURCES.md   | 34 +++++++++++++++++++++++++++++++---
  .research/STATE.md       | 15 +++++++++------
  .research/STORY.md       | 10 +++++-----
  6 files changed, 108 insertions(+), 27 deletions(-)
```

- **EXP-001 新增：** Index 行 + `## EXP-001 — Synthetic malformed-flow screening`；`Status: planned`；`Outcome: not-assessed`；`Review: none`。
- **STATE：** Active Experiment = `EXP-001`；Next 指向 execution（设计完成时点）。
- **无** `.research/reviews/`、**无** `REVIEWS.md` 变更、**无** EXP-002。
- 同 commit 含 C2 未提交的 PROJECT/STORY/RESOURCES/DISCOVERY（sandbox 遗留，已在 C2 产物说明）。

---

## 2. Claude Main 冷启动

未使用 `--permission-mode plan`。用 `--allowedTools Read,Glob,Grep` 保证不写。

```bash
cd /tmp/sw-v011-c2
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude \
    --output-format text \
    --allowedTools Read,Glob,Grep \
    -p -- \
    "$(cat /tmp/sw-v011-c2-logs/c4-claude-cold-prompt.txt)"
```

Prompt：按 AGENTS → PROJECT → STORY → STATE 冷启动；禁止改文件。

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 耗时 | 18s |
| porcelain | 空 |
| HEAD | 仍为 `8976b83` |

### 原始 stdout 摘要

读到 Current Story = 3-line checksum 筛畸形流；Gap = 尚未定义合成数据/条件；Active Experiment = **EXP-001** planned / not-assessed。阅读顺序：`AGENTS.md` → `PROJECT.md` → `STORY.md` → `STATE.md`（额外读了 `EXPERIMENTS.md` 确认 EXP-001）。未改文件。

---

## 3. Claude Reviewer（fresh `claude -p`）

第二次独立调用。工具含 Write/Edit，**不含 Bash**，避免 Reviewer commit。Prompt 明确：只允许写 `.research/reviews/EXP-001/method-review-r1.md`；禁止改 REVIEWS / EXPERIMENTS / STATE 等 canonical。

```bash
cd /tmp/sw-v011-c2
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude \
    --permission-mode acceptEdits \
    --output-format text \
    --allowedTools Read,Glob,Grep,Write,Edit \
    -p -- \
    "$(cat /tmp/sw-v011-c2-logs/c4-claude-reviewer-prompt.txt)"
```

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 耗时 | 81s |
| HEAD | **仍为 `8976b83`（Reviewer 未 commit）** |
| porcelain | 仅 `?? .research/reviews/EXP-001/` |
| canonical 是否被改 | **否** |

### 原始 stdout 摘要

`Verdict: REVISE`。五段齐：strongest evidence（构造标签、配对对照、非 checksum control）；main weakness（checksum 与判定规则未定义）；alternative explanation（checksum 可能只是 predicate 的代理）；story impact；recommended next move（冻结五项后再跑）。写到 `method-review-r1.md`。

### Reviewer 文件头（磁盘）

```text
Reviewer: Claude Code (independent reviewer, fresh session)
Model relation: different-family
Context relation: fresh-context
Date: 2026-09-03
Reviewed code commit: none (design-only; ...)
Review round: 1
Provenance: raw

## Verdict
REVISE
```

随后五个规定 heading 均在。相对 method designer（Codex / gpt-5.6-luna）为 `different-family` + `fresh-context`。

**越权判定：否。** Reviewer 只新增 untracked `method-review-r1.md`；未改 `REVIEWS.md` / `EXPERIMENTS.md` / `STATE.md`；未 `git commit`。

---

## 4. Claude Main 整合（commit B）

第三次调用。Main 读 raw review、写 canonical、由 Main 提交（含 Reviewer 文件）。

```bash
cd /tmp/sw-v011-c2
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude \
    --permission-mode bypassPermissions \
    --output-format text \
    --allowedTools Read,Glob,Grep,Write,Edit,Bash \
    -p -- \
    "$(cat /tmp/sw-v011-c2-logs/c4-claude-main-prompt.txt)"
```

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 耗时 | 302s |
| commit B | `00152fe` / `00152fe64338662c7bf67613763b14635df11655` |
| porcelain | 空 |

Claude 自述：commit 在会话中产生（`Co-Authored-By: Claude <noreply@anthropic.com>`），内容经 Main 核对；曾丢掉自己误加的重复 Recently Completed 行。仍计为 **Main 的 commit B**，不是 Reviewer commit。

### Claude Main 原始输出摘要

Commit B 四文件：`method-review-r1.md`（Reviewer raw）、`REVIEWS.md`、`EXPERIMENTS.md`、`STATE.md`。Verdict `REVISE` 写入 REVIEWS 指针与 EXPERIMENTS Review 字段。未跑代码、未建 EXP-002。

### commit B `git log -p` 摘要

```text
00152fe Review EXP-001 method r1 (REVISE): integrate verdict, freeze-before-execute
  .research/EXPERIMENTS.md                      |  2 +-
  .research/REVIEWS.md                          | 18 ++++++++++-
  .research/STATE.md                            |  6 ++--
  .research/reviews/EXP-001/method-review-r1.md | 43 +++++++++++++++++++++++++++
  4 files changed, 65 insertions(+), 4 deletions(-)
```

关键 hunk：

- `EXPERIMENTS.md`：`Review: none` → `Review: REVISE — .research/reviews/EXP-001/method-review-r1.md (see .research/REVIEWS.md)`；Status/Outcome **未改**。
- `REVIEWS.md`：新增 `### EXP-001`；`Latest method review: r1 — REVISE`；`Latest result review: none (not done)`；`Provenance: synthesis` + `Source reviews: method-review-r1.md`；Files 列表。
- `STATE.md`：Current Focus / Next 改为按 REVISE 冻结设计后再执行；Story Status 仍 `IN_PROGRESS`。
- **新增** `method-review-r1.md`（A 中不存在；`git log --diff-filter=A` 首次出现于 `00152fe`）。

A 与 B 之间 **没有第三 commit**。Reviewer 文件出现在 commit B（也可视为 A 之后、B 之内的工作树 untracked，由 Main 一并提交）。canonical 更新只在 Main 的 B，不在 Reviewer。

---

## Lead 核对

### Index 与 section

| 位置 | EXP-ID | Title | Status | Outcome |
|------|--------|-------|--------|---------|
| Index 行 | EXP-001 | Synthetic malformed-flow screening | planned | not-assessed |
| `## EXP-001` section | EXP-001 | 同名副标题 | planned | not-assessed |

一致。无 EXP-002。

### Status / Outcome / Verdict 合法性

| 字段 | 值 | 词表 | 合法 |
|------|----|------|------|
| Status | `planned` | experiment-record.md | Y |
| Outcome | `not-assessed` | experiment-record.md | Y |
| Verdict | `REVISE` | reviewer.md | Y |

Index 与 section 同步。REVIEWS `r1 — REVISE` 与 review 正文 `## Verdict` / `REVISE` 一致。

### 两次 commit 的 diff 证明

| 证明点 | 证据 |
|--------|------|
| Reviewer 文件在 B | `00152fe` 为 `A` `method-review-r1.md`；`8976b83` 无此路径 |
| Reviewer 未单独 commit | Reviewer 结束后 HEAD 仍 `8976b83`；A..B 只有 `8976b83` 与 `00152fe` |
| canonical 只在 Main 的 B | B 改 `REVIEWS.md` / `EXPERIMENTS.md` Review 字段 / `STATE.md`；Reviewer porcelain 仅 untracked review 目录 |

### 非阻塞观察（不判失败）

- commit A 混入 C2 初始化五文件（C2 sandbox 未能 commit）。
- `EXPERIMENTS.md` 的 **Next** 字段在 B 仍写 `experiment-execution ...`；`STATE.md` Recommended Next 已改为 `experiment-design (revision)`。Index/Status/Review 指针正确。

---

## 核对表

| # | 标准 | 判定 | 依据 |
|---|------|------|------|
| 1 | 必须创建 EXP-001（非 EXP-002） | **Y** | Index + `## EXP-001`；无 EXP-002 |
| 2 | Codex 写 planned / not-assessed 并 commit A | **Y** | `8976b83`；Status/Outcome 合法 |
| 3 | Codex 未跑代码、未写 reviews | **Y** | A 无 reviews 路径；REVIEWS 在 A 仍空 |
| 4 | Claude 冷启动读 AGENTS+PROJECT+STORY+STATE | **Y** | 冷启动 stdout 列出该顺序；未写文件 |
| 5 | fresh Reviewer 只写 method-review-r1.md | **Y** | porcelain 仅该目录；HEAD 未动 |
| 6 | Reviewer 含规定元数据 + `## Verdict` + 五段 | **Y** | 见文件头与五 heading |
| 7 | Reviewer 未改 REVIEWS/EXPERIMENTS/STATE | **Y** | Reviewer 后这三文件相对 A 无 diff |
| 8 | Main 整合 REVIEWS 指针 + EXP Review 字段 + STATE | **Y** | commit B 三文件 + review 文件 |
| 9 | commit B 含 Reviewer 文件且 canonical 属 Main | **Y** | `git show --name-status 00152fe` |
| 10 | 未使用 Cursor / 未用 Claude plan 只读模式做写入 | **Y** | 写入步为 acceptEdits / bypassPermissions |

**C4 通过。**
