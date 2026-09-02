# Round 3 审核报告 — Tester T2 + Reviewer R3

- **角色：** 独立 Reviewer R3 兼 Tester T2（fresh context）
- **审核时点：** 2026-09-02 22:35 左右（审核过程中 workspace 被其他 Agent 并发改写/暂存，下文以本时点磁盘 + git 为准）
- **规格：** `/Users/herxanadu/.cursor/plans/story_workspace_多agent开发_7d4d7dfb.plan.md` §5 / §7.4–7.7 / §14–16 / §18–19 / §21 Phase 8–11 / §22 Test A/B/E/H / §24 A–G
- **对象：** workspace `/Users/herxanadu/Documents/story-research-workspace`；toy 代码 `/Users/herxanadu/Documents/story-research-code`
- **只读：** 本审核未改任何其他文件、未执行 git 写操作

开发 Agent 声称对照（本审核结论：**不能原样接受**）：

| 声称 | 核实 |
|------|------|
| EXP-001 MOCK 闭环已跑完 | **部分成立**（HEAD 上有可重建链路，但 v0.1 冻结快照内部自相矛盾） |
| Gate 3 通过 | **不成立**（冻结 commit 的 EXPERIMENTS 与 reviews/metrics 对不上） |
| adapters 与 harness 矩阵已写 | **文件存在**；矩阵「verified」**无独立实测产物** |
| framework-maintenance 审计通过 | **声明文件存在且结论错误** |
| tag `v0.1` → `12c132b` | **成立**（annotated tag `v0.1^{commit}` = `12c132b4b1969d28589faa2323af7a0d0efbc679`） |

---

## T2 全链路重建

重建原则：一个全新 Agent 打开 workspace 时读的是**工作区文件**；冻结声明以 **git tag `v0.1`** 为准。当前二者不是同一快照。

**Git 分层（必须先读这一段，否则后续数字会对不上）：**

```text
代码仓库 HEAD = b0621e2  （metrics: LR 1.0 / IF 0.5455 / n_train=40 / n_test=10）
代码仓库 superseded-initial        （metrics: LR 1.0 / IF 0.6154 / n_train=480 / n_test=120）

workspace tag v0.1 = 12c132b
  EXPERIMENTS 绑定 superseded-initial / IF 0.6154 / n_test=120
  reviews/EXP-001 绑定 b0621e2 / IF 0.5455 / n_test=10   ← 同一冻结点已经打架

workspace HEAD = a2e948b  （v0.1 之后一笔 chore）
  EXPERIMENTS/STORY/STATE/DISCOVERY 已改写成 b0621e2 / 0.5455
  但删掉了 Index + Status；STORY 写入了具体 F1

index（已暂存未提交）：
  .claude/skills/* 十个目录 symlink
  adapters/README.md、adapters/cursor.md
  .research/work/{framework-dev/round-1-review.md, mock-test-scenarios.md, v0.1-maintenance-audit.md}
```

### 1. EXPERIMENTS.md 字段与 Status

**当前磁盘 / HEAD `a2e948b`（新 Agent 会读到的版本）**

| 字段 | 有无 | 证据 |
|------|------|------|
| Question | 有 | L9：三特征 vs full-feature IF 的 F1 差距 |
| Motivation | 有 | L11：对应 STORY Open Gap #1 / Core Idea |
| Method | 有 | L13–16：3-feature LR + z-score vs IF |
| Comparisons | 有 | L18 |
| Data / Setup | 有 | L20：MOCK 合成 50 行，80/20，seed 42 |
| Runs | 有 | L22–24：R1 seed 42，日期写成 **2025-09-02**（commit 日期实为 **2026-09-02**） |
| Code | 有 | L26：`flow-detector` |
| Git | 有 | L28–32：repo `../story-research-code`，commit `b0621e2…`，Entry `experiments/EXP-001/run_config.yaml` |
| Results | 有 | L34：`../story-research-code/results/EXP-001/metrics.json` |
| Main Findings | 有 | L36–40：LR 1.0000 / IF 0.5455 / gap −45.45 pp / n=40/10 |
| Interpretation | 有 | L42 |
| Discovery Impact | 有 | L44 |
| Story Impact | 有 | L46 |
| Review | 有 | L48：`.research/reviews/EXP-001/` |
| Next | 有 | L50：EXP-002 ablation |
| **Status** | **无** | section 无 `**Status:**`；顶部 **无 Index 表** |
| **Index** | **无** | 违反 `.agents/references/experiment-record.md`（要求顶部索引表 + Status 枚举） |

**v0.1 / `12c132b`（声称的冻结点）**

- 有 Index 表；section **Status: completed**
- Git commit 记为 `superseded-initial-commit`
- Entry：`experiments/EXP-001/run.py`（config: `run_config.yaml`）
- Main Findings：IF F1 = **0.6154**，n_test=**120**，gap ≈ −38.46 pp

**Status 结论：** 冻结点为 `completed`；当前 HEAD **未写 Status**。T2 不能把「Status=completed」当作当前文件事实。

### 2. RESOURCES.md 能否定位 Codebase

Codebase ID = **`flow-detector`**（与 EXPERIMENTS `Code:` 一致）。

定位提示（HEAD）：

- Preferred relative：`../story-research-code`
- Last known local：`/Users/herxanadu/Documents/story-research-code`
- Git：local，commit `b0621e2…`，branch `master`
- Remote：`N/A (MOCK local toy repo)`
- Entry points：`run_config.yaml` / `run.py` / `README.md`

换机器后：**相对路径 + Codebase ID 足够在并列布局下重找**；无 Git remote，不能靠 clone 恢复。这符合 MOCK 玩具仓库，但 §6「路径失效时自行重定位」在无 remote 时会停在「询问用户」。

HEAD 额外写入了多处绝对路径（数据集、compute working directory、Results Artifacts 表）。模板要求「资源身份 + 定位提示，非绝对路径依赖」——相对路径仍在，但绝对路径会在换机后误导。

**T2 判定：** 本机可定位。换机器：**部分足够**（相对路径有；remote 无）。

### 3. 代码仓库 git 核实

命令证据：

```text
git -C story-research-code log --oneline --all
  b0621e2 EXP-001: add baseline.py, 50-row mock data, run metrics
  4b30289 Add .gitignore; drop pycache from tracking
  superseded-initial Initial MOCK EXP-001 flow-detector baseline

git show --stat b0621e2  → 存在
  含 experiments/EXP-001/run.py、run_config.yaml、results/EXP-001/metrics.json、src/baseline.py
代码仓库 working tree clean；HEAD = b0621e2
```

| 检查项 | HEAD 记录 (b0621e2) | v0.1 EXPERIMENTS 记录 (superseded-initial) |
|--------|---------------------|----------------------------------|
| commit 存在 | 是 | 是 |
| Entry 在该 commit 中 | `run_config.yaml` 为 blob；`run.py` 也在 | `run.py` 为 blob；`run_config.yaml` 也在 |
| `results/EXP-001/` 存在 | 是 | 是（该 commit 内 metrics 不同） |
| metrics 与 EXPERIMENTS 一致 | **是**：`three_feature_lr_f1: 1.0, full_feature_if_f1: 0.5455, n_train: 40, n_test: 10` | **是（对 superseded-initial 自身）**：IF 0.6154 / n_train 480 / n_test 120 |
| 与 **同 tag 的 reviews** 一致 | 是 | **否** — reviews 写的是 0.5455 / n_test=10 / commit b0621e2 |

布局符合 §15：`src/`、`experiments/EXP-001/`、`results/EXP-001/`。`run.py` 属于实验代码，允许。

**额外：** `superseded-initial` 把 `src/__pycache__/*.pyc` 提交进仓库；`4b30289` 才 drop。v0.1 若按 EXPERIMENTS 回溯到 `superseded-initial`，会落到含 pycache 的初始快照，而磁盘当前 results 已是 `b0621e2`。

### 4. Review 文件与 REVIEWS.md 索引

存在：

- `.research/reviews/EXP-001/method-review.md`
- `.research/reviews/EXP-001/result-review.md`
- `REVIEWS.md` Index 指向上述两文件

**Reviewer identity：** 两份均写 `MOCK independent reviewer (Codex subagent)`，日期 `2025-09-02`（与代码 commit 的 2026-09-02 差一年）。身份有记录，但不是「不同模型家族 / fresh 外部 Reviewer」的证据；与 Worker F 执行侧同属 Codex 声明。

**§12 / prompts 要求的五段标题：**

| 要求标题 | method-review | result-review |
|----------|---------------|---------------|
| strongest evidence | 无（只有 Strengths） | 无 |
| main weakness | 无（只有 Concerns） | 无（只有 Anomalies / Caveats） |
| alternative explanation | **无** | **无** |
| story impact | **无** | **无**（Verdict 间接提到 Story Evidence） |
| recommended next move | 无独立标题（Verdict 末句有 follow-up） | 无独立标题（Verdict 提到 EXP-002） |

实际标题为 Scope / Strengths / Concerns / Verdict 与 Results Summary / Assessment / Anomalies / Verdict。规范 prompt（`.agents/prompts/method-review.md`、`result-review.md`）未被遵守。

**REVIEWS.md：** HEAD 摘要把 result-review 写成「接受为 Story Evidence」；v0.1 摘要曾是 `ATTENTION_REQUIRED — toy 结果不可外推`。索引能找到文件，但摘要与冻结点叙事已改写且未反映五段结构缺失。

### 5. DISCOVERY / STORY / STATE 是否互相复制

**DISCOVERY（HEAD）**

- 有对应条目：「Three flow statistics suffice on MOCK subset (EXP-001)」
- **没有**规范字段 `Evidence: EXP-001`（模板与 §7.4 要求）
- Evidence 写成 commit hash + `results/EXP-001/metrics.json`
- 正文复制 EXPERIMENTS 数字：F1=1.0 / 0.5455
- 文末写「Test F/G/J 可在 Phase 11 追加」→ 等于承认负结果/推翻 Core Idea **尚未做**

**v0.1 DISCOVERY** 反而有 `Evidence: EXP-001`，并保留负向发现（IF 0.6154 不构成强对照）。HEAD 把 Negative Discoveries 清空成「待后续实验」，与「闭环后学到了弱基线」这一科学内容相比是倒退。

**STORY（HEAD）**

- 六段齐全
- Evidence 写入 **具体数字与 commit**：`F1=1.0000` / `0.5455` / `gap −45.45 pp` / `commit b0621e2`
- 横幅仍写「不含具体性能数字」——自相矛盾
- 违反 §7.2
- Open Gaps #1 仍在；EXPERIMENTS Story Impact 却写「Open Gap #1 在 MOCK 层面临时闭合」——两文件对 Gap 是否闭合说法不一致
- **v0.1 STORY Evidence 没有数字**（质量更好）；HEAD 的 `a2e948b` 把数字加进去了

**STATE（HEAD）**

- 指向下一步：设计 EXP-002；可选真实 CICIDS 复验
- Active = none；Story Status = `IN_PROGRESS`
- Recently Completed 再次抄 F1 与完整 commit hash（STATE 应变小，不应成为第二份实验日志）
- 日期行：`Worker F, 2025-09-02`

**三者是否互相复制：** 是。HEAD 上 STORY / STATE / DISCOVERY / EXPERIMENTS / REVIEWS 摘要都写了同一组 F1。§1 明确禁止三层互相复制。

### 6. §22 Test A / B / E / H

#### Test A — Cold Start：**PASS（带 MINOR 污染）**

本 Tester 只读 `AGENTS.md` + `PROJECT.md` + `STORY.md` + `STATE.md`，四问可答：

1. **项目：** MOCK「Lightweight Flow-Feature Anomaly Detection」——用少量流统计做轻量检测，主数据 CICIDS2017 子集。
2. **Story：** 三统计量（duration / packet-ratio / byte-asymmetry）+ 轻量分类器即可捕获多数攻击信号。
3. **位置：** EXP-001 MOCK 已 closed；Story `IN_PROGRESS`；无活跃实验。
4. **下一步：** 设计 EXP-002 攻击族 ablation；可选真实子集复跑。

未必须再读十几个文件。STATE 已写出 Recommended Next Action。**瑕疵：** 冷启动会从 STORY/STATE 读到 F1 数字（本不该在这两层），但不阻碍理解项目/Story/位置/下一步。

#### Test B — Cross Session / 接着做 EXP-002：**PASS（带 MAJOR 路标噪声）**

新 Agent 从 STATE 能看到「设计 EXP-002：攻击族 ablation」，EXPERIMENTS Next 同向，Skills（`experiment-design` 等）存在。

**路标噪声：** 若该 Agent 再读 tag `v0.1` 的 EXPERIMENTS，会以为该复验 IF 基线（commit `superseded-initial`）；读 HEAD 则去 ablation。Worker I 审计还宣称「V0.1 ready to freeze」。不阻塞启动 EXP-002，但会让「下一步」出现两个版本。

#### Test E — External Code Repo：**PASS（仅对 HEAD）；v0.1 冻结链 FAIL**

按 RESOURCES + EXPERIMENTS（HEAD）：

```text
flow-detector → ../story-research-code → commit b0621e2
→ experiments/EXP-001/run_config.yaml
→ results/EXP-001/metrics.json
```

本机全部命中，metrics 与 HEAD EXPERIMENTS 一致。

按 **tag v0.1 的 EXPERIMENTS**：commit `superseded-initial` 存在且该 commit 内 metrics 为 0.6154/n_test=120，但同 tag 的 `result-review.md` 与磁盘当前 metrics 是另一套数。冻结链 **不能** 从 EXPERIMENTS 走到与 Review 一致的 Results。

T2 对「新 Agent 打开当前工作区」打 **PASS**；对「声称 Gate 3 / v0.1 冻结链」打 **FAIL**。

#### Test H — Reviewer Trace：**PASS（结构）/ FAIL（规范正文）**

任意看到 EXP-001 都能找到 method-review 与 result-review（EXPERIMENTS Review 字段 + REVIEWS.md 索引 + 目录）。结构追溯 **PASS**。

规范要求的五段标题缺失 → 作为「独立 Reviewer 产物是否合格」**FAIL**；Test H 字面是「能快速找到两份文件」，故 Test H 记 **PASS**，把五段缺失放到 R3 MAJOR。

### 7. 全链路是否可理解

科学问题 → 代码 → Git → Runs → Results → Interpretation → Reviewer → Discovery → Story Impact：

**一个新 Agent 沿 HEAD 文件可以走完这条链**，并理解 MOCK 结论（三特征在玩具数据上可分、IF 玩具基线弱、下一步 ablation）。

**不能**声称「Gate 3 已通过」或「v0.1 冻结点可复现同一条链」：冻结点 EXPERIMENTS 与 reviews/results 指向两次不同 run。HEAD 为对齐数字付出了违反 Story/Discovery 分层的代价。`.research/work/` 没有 literature-scout / experiment-agent / result-analyst 产物，Phase 8「实际派三个 subagent」**无文件证据**。

---

## R3 审计

分级：CRITICAL 必须修才能冻结；MAJOR 应在 v0.1 前修；MINOR 可记债。每条含路径、位置、建议修法。

### CRITICAL

#### C1. `v0.1` 冻结快照内部自相矛盾（Gate 3 未过）

- **路径：** `git tag v0.1` → commit `12c132b`（message: Wave 5 adapters README）；其中 Gate 3 内容来自 `5540420`
- **位置：** `12c132b:.research/EXPERIMENTS.md` Git/Results 段 vs `12c132b:.research/reviews/EXP-001/result-review.md` Results Summary
- **事实：** EXPERIMENTS 记 `superseded-initial` / IF **0.6154** / n_test=**120**；result-review 记 commit **`b0621e2`** / IF **0.5455** / n_test=**10**。`git show 5540420` 自己写「Links … superseded-initial … reviews」，但同 commit 加入的 reviews 已经是另一套数。
- **建议：** 选定 **一个** 有效 run（建议代码 HEAD `b0621e2` + 当前 `metrics.json`）。把 EXPERIMENTS / DISCOVERY / REVIEWS / reviews 正文 / STATE 全部改到同一 commit 与同一组 metrics。然后 **移动或重打 tag**（仅在用户明确要求时由 Lead 执行）。在此之前不得宣称 Gate 3 通过。

#### C2. 声称「已冻结 v0.1」但 HEAD 与 index 都已离开 tag

- **路径：** workspace git
- **位置：** `v0.1^{commit}` = `12c132b`；`HEAD` = `a2e948b`（`chore: track all framework files after excludesfile fix`，改写了 6 个 `.research` 文件）；index 另有 15 个已暂存未提交路径（`.claude/skills/*`、`adapters/README.md`、`adapters/cursor.md`、三份 work 文档）
- **事实：** 开发声称 tag 即 V0.1。实际：冻结点之后还有 commit；Wave 5 薄 adapter / Claude symlink / 维护审计 **尚未进入任何 commit**。`origin/master` 已与 `HEAD` 对齐（`a2e948b` 已推送），tag 仍停在 `12c132b`。
- **建议：** 先修 C1 与下方 MAJOR；再一次性 commit Wave 5；干净工作区后由 Lead 重打 `v0.1`。禁止在 dirty index 上宣布冻结。

#### C3. framework-maintenance 审计结论与文件事实相反

- **路径：** `.research/work/v0.1-maintenance-audit.md`（仅在 **index**，未进 HEAD/tag）
- **位置：** Checklist「STORY as experiment log: PASS — numbers stay in EXPERIMENTS」；「DISCOVERY vs EXPERIMENTS overlap: PASS」；「§24 A–G 全 Yes」；「Verdict: V0.1 ready to freeze」
- **事实：** 同工作区 `STORY.md` L20 含 `F1=1.0000` / `0.5455`；`STATE.md` L15 再抄一遍；`DISCOVERY.md` L13 再抄一遍。审计未核对或核对后仍标 PASS。
- **建议：** 作废该 Verdict。按真实文件重跑 `framework-maintenance`；STORY 无数字之前不得写 PASS。

#### C4. STORY 写入具体性能数字（§7.2 / §1）

- **路径：** `.research/STORY.md` L1–3 横幅 vs L20 Evidence
- **位置：** Evidence：「commit `b0621e2` … F1=**1.0000** … F1=**0.5455**（gap −45.45 pp）」
- **事实：** 模板 `.agents/templates/STORY.template.md` 写明「不含具体性能数字」「引用 EXP-xxx，不抄数字」。`a2e948b` 相对 v0.1 的唯一 STORY 改动就是把无数字的 Evidence 换成带数字的 Evidence（`git show a2e948b -- .research/STORY.md`）。
- **建议：** Evidence 只保留「EXP-001 在 MOCK 合成流上给出正向内部证据；IF 玩具基线偏弱；真实 CICIDS 仍开放」。数字只留在 EXPERIMENTS + reviews。

### MAJOR

#### M1. EXPERIMENTS 丢掉 Index 与 Status

- **路径：** `.research/EXPERIMENTS.md`；规范 `.agents/references/experiment-record.md` L7–47
- **位置：** HEAD 文件在标题后直接 `## EXP-001`，无 Index 表、无 `**Status:**`
- **事实：** v0.1 有 Index 且 Status=`completed`。`a2e948b` 删掉它们。模板 `EXPERIMENTS.template.md` 也缺 Index/Status，与 reference 不一致。
- **建议：** 恢复 Index 表 + `Status: completed`。同步改模板，使其与 `experiment-record.md` 一致。

#### M2. DISCOVERY 未用 `Evidence: EXP-001`，并与 EXPERIMENTS 复制数字

- **路径：** `.research/DISCOVERY.md` L11–16；模板 `DISCOVERY.template.md` L11
- **位置：** Evidence 字段是 commit hash，不是 `Evidence: EXP-001`
- **建议：** 改回 `- Evidence: EXP-001`。Finding 用机制语言（「三特征在合成可分分布上线性可分；当前 IF 实现不构成强对照」），F1 只指向 EXPERIMENTS。恢复 v0.1 中关于弱 IF 对照的 Negative / Open Contradiction，不要清空成「待实验」。

#### M3. Review 正文缺少规定五段；替代解释缺席

- **路径：** `.research/reviews/EXP-001/method-review.md`、`result-review.md`；对照 `.agents/prompts/method-review.md` L40–58、`result-review.md` L40–57、`.agents/subagents/reviewer.md`
- **位置：** 现有 `## Strengths/Concerns/Verdict` 等，无 `## strongest evidence` 等五标题；两文件均无 alternative explanation
- **建议：** 按 prompt 重写（可保留现有事实）。result-review 必须讨论：完美 F1 可能来自合成可分性而非「三特征充分」；IF 弱可能来自 toy 实现。Reviewer 行建议标明模型/session，避免「Codex subagent」与执行方无法区分。日期改为 2026-09-02。

#### M4. Adapter 复制了科研 ground truth；「verified」无独立证据

- **路径：** `adapters/README.md`（**暂存区**，比 HEAD/tag 多 115 行）
- **位置：** L31–39「Expected answers」写入 F1=1.0 / IF 0.5455 / commit `b0621e2`；L41–51 矩阵全部 **verified**；L84–92 CLI smoke notes
- **事实：** §19 adapter **不得**回答 How research should work / 实验如何评价。把 MOCK 指标写进 adapter 是复制 canonical 科研状态。工作区 **没有** Worker G 的 CLI 原始输出/日志；`command -v` 只能证明二进制在 PATH：
  - 已安装：`codex`、`claude`、`dsh`、`cursor-agent`
  - 未安装：`opencode`
  - 「实测通过」：本 R3 **未**重跑四条 CLI（避免与并发 Agent 争用）；也 **未**在 `.research/work/` 找到可复核的 stdout。矩阵用语应为「已安装 / 未安装 / 声称实测（无产物）」，不能写 verified 当作 §24 F 已满足。
- **建议：** 从 README 删除 F1/commit ground truth。矩阵三列改为：安装状态 / 本机是否跑过 smoke / 产物路径。smoke 记录放到 `.research/work/framework-dev/` 而不是 adapter。

#### M5. Claude Code 限制记录自相矛盾；薄 adapter 未进冻结点

- **路径：** `adapters/claude-code.md` L12；暂存 `adapters/README.md` L53–58 vs L97；磁盘 `.claude/skills/<name> -> ../../.agents/skills/<name>`
- **位置：**
  - `claude-code.md`：「no project `.claude/skills` mirror required for V0.1」
  - 暂存 README 技能根表：「`.agents/skills/` (no `.claude/` mirror in repo)」
  - 同文件 Limitations：「`.claude/skills/` → `.agents/skills/` symlinks **added in Phase 10**」
  - 磁盘：10 个目录 symlink **已存在**，但只在 **index**，不在 `v0.1` / `HEAD`
- **事实：** 计划 Phase 9 预告：Claude Code 只读 `CLAUDE.md` 与 `.claude/skills/`，不读 `AGENTS.md` 与 `.agents/skills/`。`CLAUDE.md` 作为 boot 指针存在（合格薄入口）。目录 symlink 是正确薄方案，但：(1) 未冻结；(2) 三处文档互相否定；(3) `claude-code.md` 未写清「宿主不自动读 AGENTS/.agents」这一限制。
- **建议：** `claude-code.md` Limitations 明确写 Phase 9 那句限制 + symlink 方案。README 技能根改为「canonical `.agents/skills/`；Claude 另经 `.claude/skills/` symlink」。提交 symlink。删除「no mirror」表述。

#### M6. Test F / G / J 未执行，被写成已覆盖

- **路径：** `.research/work/mock-test-scenarios.md`；`v0.1-maintenance-audit.md` L19–25；`DISCOVERY.md` L41
- **位置：** mock-test-scenarios 自陈「Instruction-only fixtures… Not real experiments」；DISCOVERY 说「可在 Phase 11 追加」
- **事实：** 没有 EXP-NEG / 推翻 Core Idea 的真实 MOCK 记录，也没有「大量历史 EXP」。Phase 11 / T3 工作未做。审计把「pattern 文档」当成 Test F/G/J 覆盖。
- **建议：** 要么按 §22 真正写入一条失败 EXP + 一条推翻 Core Idea 的 MOCK（可极短），要么在完成定义里把 F/G/J 标为 v0.1 未做、不宣称 Phase 11 终审通过。本 R3 任务不含代做 T3。

#### M7. STATE 膨胀并复制实验数字

- **路径：** `.research/STATE.md` L13–16、L45；`wc -l` = 45（§20「尽量几十行」行数过关）
- **位置：** Recently Completed 含 F1 与 40 位 hash；页脚 2025-09-02
- **建议：** Recently Completed 写成「EXP-001 MOCK 闭环（见 EXPERIMENTS）」；下一步保留 EXP-002。日期用 2026-09-02。

#### M8. `.research/work/` 与 Phase 8 并行 subagent 声称不符

- **路径：** `.research/work/`
- **位置：** 仅 `.gitkeep` +（暂存）framework-dev 审核稿、mock-test-scenarios、v0.1-maintenance-audit。无 `literature-scout` / `experiment-agent` / `result-analyst` 工作稿
- **事实：** reviews 目录符合「正式 review」。work 被用来放框架开发审核，尚可；但 Phase 8「实际派三个 subagent」没有 work 产物。codex adapter 写 work artifacts 应在 `.research/work/<role>/`
- **建议：** 若狗食时派过 subagent，补保存 handoff 或声明「未落盘、仅会话内」。不要让 work/ 只剩框架开发文件却声称科研 subagent 已跑。

### MINOR

#### m1. 日期年号 2025 vs 2026

- **路径：** EXPERIMENTS Runs、reviews Date、STATE 页脚、暂存 adapters/README Worker G 日期 vs git author date 2026-09-02
- **建议：** 统一 2026-09-02。

#### m2. RESOURCES 绝对路径增多

- **路径：** `.research/RESOURCES.md` L15、L33、L51、L70
- **建议：** 主定位用相对路径；绝对路径只留在 Last known local。

#### m3. adapters/README 复制冷启动读序

- **路径：** 暂存 README「Canonical read order」= AGENTS → PROJECT → STORY → STATE
- **建议：** 改为「见 AGENTS.md Start Here」，adapter 只保留 CLI 怎么调。

#### m4. EXPERIMENTS Story Impact vs STORY Open Gaps 不一致

- **路径：** EXPERIMENTS L46 vs STORY L31
- **建议：** Gap #1 在 STORY 保留「真实子集未验证」；EXPERIMENTS 不要写「临时闭合」，或改成「MOCK 层有证据、真实层仍开放」。

#### m5. 模板与实例/reference 结构不完全一致

- **路径：** `.agents/templates/EXPERIMENTS.template.md` 无 Index/Status；`experiment-record.md` 有
- **建议：** 模板补 Index + Status，避免下一轮 MOCK 再丢。

#### m6. MOCK 主题一致性（本项通过）

- **路径：** `.research/*`
- **事实：** `rg PTC|CLINC|温度校准` 无命中。全部为流特征检测 MOCK。无早期草稿混入。
- **无需修。**

#### m7. 框架内无 `.py/.sh/.js/.ts`（本项通过）

- **命令：** `find workspace -type f \( -name '*.py' -o -name '*.sh' -o -name '*.js' -o -name '*.ts' \) -not -path '*/.git/*'` → 空
- **代码仓库** `run.py` 等属实验代码，允许。

#### m8. 全局 `~/.gitignore` 含 `/*`

- **路径：** `/Users/herxanadu/.gitignore` 第 1 行 `/*`
- **事实：** 开发注释属实。HEAD 已跟踪 51 个文件（`git ls-files` HEAD=51）。当前 **find -P -type f** = 54（+3 份未提交 work 文档）；index=64（+10 个 `.claude` symlink 条目 + 上述 3 个 work + adapter 仍算 modified）。不一致来自 **未提交的 symlink 与 work 文件**，不是「文件没被跟踪」的全面失败。
- **未跟踪/仅暂存未提交：** 见 C2 列表。无未暂存 unstaged diff。

#### m9. 各 adapter 文件是否只答 §19 六个问题

`adapters/{codex,claude-code,cursor,opencode,deepseek-harness}.md` 结构基本是 Entry / Skills / Subagents / Reviewer / MCP / Limitations（+ Cold start 命令）。**没有**复制 experiment-record / story-loop 正文。Cold start 命令可视为宿主调用，可接受。主要越界在 **README 矩阵的 MOCK 答案**（M4），不在五个分文件的科研逻辑。

---

## §24 完成定义对照

| 条 | 判定 | 证据 |
|----|------|------|
| **A. Workspace 自包含** | **部分满足** | `.research/` + Skills 可复制走；Mode B 代码在仓库外是规格允许的，RESOURCES 有相对路径。扣分：无 code remote；HEAD 绝对路径；tag/HEAD/index 三套内容；复制 workspace **不会**得到 tag 所声称的那条 EXP 链。 |
| **B. Story 可恢复** | **部分满足** | Test A 能讲清项目/Story/薄弱点（真实 CICIDS、归一化、攻击族）。扣分：STORY 被写成带 F1 的实验日志，违反「约一页且非实验日志」。 |
| **C. History 可恢复** | **部分满足** | DISCOVERY / EXPERIMENTS / REVIEWS 都在，能看出「先有三特征 Story，再跑 EXP-001」。扣分：v0.1 与 HEAD 两套 metrics；DISCOVERY 丢掉弱基线负向发现；三文件互相抄数字。 |
| **D. Experiment 可追溯** | **部分满足** | HEAD：Codebase `flow-detector` + commit `b0621e2` + R1 + `results/EXP-001/metrics.json` + reviews 路径均存在且数字一致。扣分：v0.1 冻结链 EXPERIMENTS≠reviews；Entry 只写 yaml（run.py 存在但未作为主 Entry）；Status 缺失。 |
| **E. Research Loop 可运行** | **满足（MOCK 文件层）** | 磁盘上存在 Story → EXP-001 → Discovery → Story/State 更新的一轮。重点是闭环痕迹，不是结果质量。扣分不升格为不满足：subagent work 产物缺失、Review 格式不合格。 |
| **F. 可迁移** | **部分满足（不夸大）** | 本机 `command -v`：Codex / Claude Code / Cursor / DSH **已安装**；OpenCode **未安装**（与计划 §26.5 及 README 一致）。**没有**可独立复核的四平台 cold-start 原始输出。暂存 README 自称四平台 verified + OpenCode documented。R3 只承认：安装状态属实；OpenCode 未测；「verified」证据不足。DSH 按规格不阻塞；OpenCode 未装则 §24 F「至少 Codex/Claude/Cursor/**OpenCode**」**未满**。 |
| **G. 易维护** | **部分满足** | 10 个 Skill 仍只在 `.agents/skills/`；Claude symlink 指向 canonical，未复制内容（方向正确）。扣分：adapter README 复制科研数字与读序；`claude-code.md` 与 README/磁盘 不一致；同一行为（冷启动读什么）出现在 AGENTS + CLAUDE + adapters README。 |

Worker I 审计表把 A–G 全标 Yes —— **驳回**。

---

## 结论

### 是否可冻结 v0.1

**不可以。** tag `v0.1` 确实指向 `12c132b`，但该快照的 EXP-001 链在 EXPERIMENTS 与 reviews/results 之间已经断裂；其后 `a2e948b` 对齐了数字却破坏了 Story/Discovery 分层，且 Wave 5（`.claude/skills`、更新后的 harness 矩阵、维护审计）仍停在 index。开发侧「Gate 3 通过 + 审计通过 + 已冻结」三句话都不成立。

T2 对**当前 HEAD 工作区**的 Test A/B/E/H：**A PASS，B PASS，E PASS，H PASS（找得到文件）**。对**声称的冻结点 Gate 3 链**：E **FAIL**。

### 必须修复项（冻结前）

1. **统一 SoT：** EXPERIMENTS / reviews / `metrics.json` / RESOURCES / DISCOVERY 只绑定 **一个** commit（建议 `b0621e2`）和 **一组** metrics；废除 v0.1 上 superseded-initial↔b0621e2 混用。
2. **STORY 去掉一切 F1/gap/commit 数字**；STATE 去掉 F1 与长 hash。
3. **恢复 EXPERIMENTS Index + Status=completed**；DISCOVERY 使用 `Evidence: EXP-001`，保留弱 IF 对照的负向/矛盾项，不与 EXPERIMENTS 互抄表格。
4. **按 prompt 五段重写** `method-review.md` / `result-review.md`（必须含 alternative explanation 与 story impact）。
5. **Adapter：** 删除 MOCK 指标 ground truth；矩阵改为 已安装/未安装/有无 smoke 产物；写清 Claude「只读 CLAUDE.md + `.claude/skills/`」并提交 symlink；修正 `claude-code.md` 与 README 的互相否定。
6. **Git：** 修完后单一 commit；工作区干净；**再**打 `v0.1`（现有 tag 不能代表可发布快照）。不要在 dirty index 上宣布冻结。
7. **作废** `v0.1-maintenance-audit.md` 中「ready to freeze」与 STORY PASS；Test F/G/J 要么真做要么明确标未做，禁止用 pattern 文档冒充覆盖。

### 建议但不阻塞（若只想最小冻结）

日期年号、RESOURCES 绝对路径、Story Impact 与 Open Gaps 措辞、EXPERIMENTS 模板补 Index、Phase 8 subagent 产物补档或书面承认未落盘。
