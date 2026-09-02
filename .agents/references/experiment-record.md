# Experiment Record Reference

V0.1 规范：`EXPERIMENTS.md` 的结构、索引表、Status 值与每个 Experiment section 的字段。

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

| EXP-ID | Title | Status | Story Gap | Updated |
|--------|-------|--------|-----------|---------|
| EXP-001 | Baseline validation | completed | Open Gaps: mechanism unclear | 2026-09-02 |
| EXP-002 | Ablation study | running | Open Gaps: feature contribution | 2026-09-03 |
| EXP-003 | Control comparison | planned | Boundary: dataset scope | 2026-09-03 |
```

列说明：

| 列 | 内容 |
|----|------|
| **EXP-ID** | `EXP-NNN`，与 section 标题一致 |
| **Title** | 短标题；可与 section 副标题相同 |
| **Status** | 见下表；新增或变更实验时同步更新 |
| **Story Gap** | 指向 STORY 中 `Open Gaps` 或 `Boundary` 的具体条目（短语即可） |
| **Updated** | 该实验记录最后实质性更新的日期（ISO `YYYY-MM-DD`） |

规则：

- 每新建或关闭一个 Experiment，先更新索引表，再写/更新对应 section。
- 索引表只放摘要；细节只在 section 中维护，避免双份维护长文本。
- 老实验 Status 变为 `superseded` 或 `abandoned` 时保留行，不删历史。

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

## ID 规则

```text
EXP-001
EXP-002
EXP-031 — Candidate Ambiguity Screening   # 可选附名
```

不使用复杂编码。老 Experiment 可写得更紧凑，但 section 永久保留。

## Section 建议字段

每个 Experiment section 标题格式：`## EXP-NNN` 或 `## EXP-NNN — Title`。

建议包含：

| 字段 | 说明 |
|------|------|
| **Status** | 与索引表一致 |
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
| **Story Impact** | 对 STORY 六段的影响 |
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

Runs:
- R1: seed 601, completed
- R2: seed 602, completed
- R3: seed 602 retry after bugfix (commit def456)
```

同一 Experiment 可对应多个 Git commit（例如先发现 bug 再修复后重跑）。有效 Run 与 commit 的对应关系写在 Runs 与 Git 字段中。

## Git 绑定（正式实验必填）

每个正式 Experiment 至少能定位：

```text
Codebase ID     → RESOURCES.md
Git repository  → remote 或本地路径
Git commit      → 冻结的代码版本
Entry           → 实验入口脚本/配置
Results         → 结果目录
```

详见 `git-linking.md`。

## 与 Review 的链接

```text
EXPERIMENTS.md → EXP-031 section
.research/reviews/EXP-031/method-review.md
.research/reviews/EXP-031/result-review.md
REVIEWS.md → EXP-031 摘要
```

## 写作原则

- **不要把运行成功等同于科学成功** — 只记录事实。
- **负结果与 null 结果保留** — 写入 Main Findings，提炼到 DISCOVERY。
- **数字留在 EXPERIMENTS** — STORY 不抄性能数字。
- **紧凑但不删历史** — 老实验可缩短正文，不删除 section 或索引行。
