# Experiment Record Reference

V0.1 规范：`EXPERIMENTS.md` 的结构、索引表、Status 值、Outcome 值与每个 Experiment section 的字段。

## 文件结构

`EXPERIMENTS.md` 由两部分组成：

1. **顶部索引表** — 快速扫描全部实验
2. **按 EXP-ID 排列的完整 section** — 每个 Experiment 永久保留，老实验可写得更紧凑

所有 Experiment 记录在同一个文件中；不拆成独立数据库或 per-EXP 文件。

## 索引表格式

放在 `EXPERIMENTS.md` 最顶部，按 EXP-ID 升序：

```markdown
# Experiments

## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-001 | Baseline validation | completed | supports | Open Gaps: mechanism unclear | 2026-09-02 |
| EXP-002 | Ablation study | running | not-assessed | Open Gaps: feature contribution | 2026-09-03 |
| EXP-003 | Control comparison | planned | not-assessed | Boundary: dataset scope | 2026-09-03 |
```

列说明：

| 列 | 内容 |
|----|------|
| **EXP-ID** | `EXP-NNN`，与 section 标题一致 |
| **Title** | 短标题；可与 section 副标题相同 |
| **Status** | 见 §Status 值；新增或变更实验时同步更新 |
| **Outcome** | 见 §Outcome 值；每个 Experiment 必有；新建固定 `not-assessed` |
| **Story Gap** | 指向 STORY 中 `Open Gaps` 或 `Boundary` 的具体条目（短语即可） |
| **Updated** | 该实验记录最后实质性更新的日期（ISO `YYYY-MM-DD`） |

规则：

- 每新建或关闭一个 Experiment，先更新索引表，再写/更新对应 section。
- 索引表只放摘要；细节只在 section 中维护，避免双份维护长文本。
- 老实验 Status 变为 `superseded` 或 `abandoned` 时保留行，不删历史。
- 索引表 Outcome 列与 section 字段必须同步；不要只改一处。

## Status 值

V0.1 冻结以下 Status；索引表与 section 内 `Status:` 字段保持一致。

| Status | 含义 | 典型时机 |
|--------|------|----------|
| `planned` | 已设计，尚未执行 | experiment-design 完成，未开始跑 |
| `running` | 正在执行或分析中 | 有活跃 Run 或结果待解读 |
| `completed` | 已有可分析结果并记录 | Main Findings 已写入 |
| `failed` | 技术失败，无法得到可用结果 | 环境/代码/数据错误导致实验作废 |
| `abandoned` | 主动放弃，未得到有信息量的结果 | 判断低价值或方向错误后停止 |
| `superseded` | 被后续实验取代 | 新 EXP 明确覆盖同一科学问题 |

转换建议（非强制状态机）：

```text
planned → running → completed
planned → running → failed | abandoned
completed | failed | abandoned → superseded（当新 EXP 接管同一问题时）
```

`completed` 不表示"假设被证实"；只表示实验执行完毕且结果已记录。

## Outcome 值

每个 Experiment **必有** Outcome；新建时固定为 `not-assessed`。索引表与 section 内 `Outcome:` 字段保持一致。

**Status** = 工作生命周期；**Outcome** = 科学结果语义。二者独立，不要互相推导。technical failure ≠ negative scientific finding：`Status=failed` 时 Outcome 保持 `not-assessed`，不把它当成负向科学发现。

完整枚举（只在本文件定义；其它文件引用本节，不要再抄这张表）：

| Outcome | 精确定义 |
|---------|----------|
| `not-assessed` | 尚未形成科学判断，包括 planned/running/技术失败 |
| `supports` | 有效证据支持该 Experiment 所测试的主要假设/预测 |
| `contradicts` | 有效证据明确与主要假设/预测相反 |
| `null` | 实验有效完成，目标效应/差异基本未观察到，本身是有信息量的零结果 |
| `inconclusive` | 数据、方差、控制、样本量等不足以判断支持还是反对 |
| `invalid` | 运行可能完成，但由于泄漏、实现错误、不公平比较等，不可用于科学推断 |

科学结论如何进入 DISCOVERY 由 `result-analysis` 决定；本表只定义词义。
Execution success / finite output only establishes artifact usability.
`supports` only when the EXP's predeclared scientific prediction is
satisfied.

## ID 规则

```text
EXP-001
EXP-002
EXP-031 — Candidate Ambiguity Screening   # 可选附名
```

不使用复杂编码。老 Experiment 可写得更紧凑，但 section 永久保留。

## What deserves a new EXP-ID?

New EXP-ID is for a scientific question that can change a scientific decision.

一个任务只有同时满足下面条件时，默认才创建新的 Experiment：

1. **必须有 Scientific Question** — 例如「source deletion stability 是否真正带来独立于普通 SSL-kNN 的候选质量提升？」而不是「实现 parser / 修 UUID / 补 schema / 同步代码 / 检查目录」。
2. **至少存在两个可能结果，并会导致不同科研判断** — 例如 `Full > matched sham` → source mechanism remains plausible；`Full ≈ matched sham` → source mechanism lacks independent value。若可能结果只是 `tests pass / tests fail` 或 `file exists / file missing`，通常不是独立科学 Experiment。
3. **结果能够改变至少一个东西：** method design、mechanism belief、Story Boundary、Open Gap、route choice、scientific comparison。

判断标准不是名字（admission / qualification / audit），而是：**结果是否改变我们对研究对象或方法有效性的科学判断？** 必要的数据有效性、共线、held-out、provenance、control 泄漏问题可以成为科学 Experiment。

### Support-task rule

以下工作默认作为**当前科学 EXP 的 support work**，不自动创建新 EXP-ID，不自动产生新的 Story Impact，不自动产生新的 Reviewer cycle：

```text
路径修复 · Python/version compatibility · runner bug · 日志修复
文件同步 · schema 实现 · serialization · parser · CLI 参数
GPU 调用 bug · 代码重构 · 单元测试补齐 · 结果文件格式
普通数据转换 · 重复下载 · 远端传输
```

记录在 current EXP Run notes、`.research/work/<current-exp>-support-*.md`、或 Git commit，然后返回当前科学 Experiment。

A purely mechanical smoke/support task does not receive a new science
EXP-ID; record its pass/fail under the parent EXP support/run notes.
If a support repair would change the scientific contract (split,
candidate labels, train/test grouping, sample selector, or similar):
stop compact support and return to `experiment-design`. Independent
review only when the redesign is high-stakes under `experiment-review`
or previous evidence may be invalidated. Method-component change ≠
automatic Reviewer.

### 不回写历史

已有 EXP（例如 EXP-677 / EXP-678 / …）即使今天看来更像工程/资格任务：不要删除、不要重新编号、不要批量补 class、不要改写历史 Outcome。新规则只适用于以后。历史保持可追溯。

## Section 建议字段

每个 Experiment section 标题格式：`## EXP-NNN` 或 `## EXP-NNN — Title`。

建议包含：

| 字段 | 说明 |
|------|------|
| **Status** | 与索引表一致 |
| **Outcome** | 与索引表一致；见 §Outcome 值 |
| **Question** | 这个实验要回答的科学问题 |
| **Motivation** | 与当前 Story gap 的关系 |
| **Method** | 方法概要（非完整论文方法节） |
| **Comparisons** | 对照/基线 |
| **Data / Setup** | 数据集、划分、关键超参 |
| **Runs** | 每次执行（seed、retry 等），无全局 Run ID |
| **Code** | Codebase ID（对应 RESOURCES） |
| **Git** | repository + commit（可多 commit，见 `git-linking.md`） |
| **Results** | 结果位置（路径或 URI） |
| **Main Findings** | 客观发生了什么 |
| **Interpretation** | 对 Story 的含义 |
| **Discovery Impact** | 应写入 DISCOVERY 的要点 |
| **Story Impact** | `Level 0|1|2 — {{对 STORY 六段的影响}}`（见 `story-loop.md` §Story Impact Level；不是 Outcome） |
| **Review** | 是否/如何 Review；链接 REVIEWS |
| **Next** | 建议后续实验 |

## Experiment vs Run

```text
Experiment = 一个科学问题
Run        = 该问题的一次具体执行
```

Run **不**建立全局编号（无 `RUN-042`）。只在 Experiment section 内用局部标签。

示例：

```markdown
## EXP-031 — Feature Ablation

Status: completed
Outcome: supports

Runs:
- R1: seed 601, completed
- R2: seed 602, completed
- R3: seed 602 retry after bugfix (commit def456)
```

同一 Experiment 可对应多个 Git commit（例如先发现 bug 再修复后重跑）。有效 Run 与 commit 的对应关系写在 Runs 与 Git 字段中。

## Git 绑定（正式实验必填）

正式实验的 Git 字段与路径恢复见 `git-linking.md`。本节不重复五元组定义。

## 与 Review 的链接

```text
EXPERIMENTS.md → EXP-031 section
.research/reviews/EXP-031/method-review-r<N>.md
.research/reviews/EXP-031/result-review-r<N>.md
REVIEWS.md → EXP-031 摘要
```

命名与 Verdict 契约见 `reviewer.md`。

## 写作原则

- **不要把运行成功等同于科学成功** — Status 记工作生命周期，Outcome 记科学判断（§Outcome 值）。Execution success / finite output only establishes artifact usability. `Outcome=supports` only when the EXP's predeclared scientific prediction is satisfied.
- **负结果与 null 结果保留** — 有效完成的科学结论写入 Main Findings；技术失败保持 `not-assessed`，不当作负向科学发现。
- **数字留在 EXPERIMENTS** — STORY 不抄性能数字。
- **紧凑但不删历史** — 老实验可缩短正文，不删除 section 或索引行。
