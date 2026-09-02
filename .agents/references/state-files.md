# State Files Reference

V0.1 规范：八个 `.research/` 状态文件的职责、边界与反重复规则。

## 认知分层

循环见 `story-loop.md` §核心循环。

```text
PROJECT      = 我们最终想解决什么
STORY        = 我们现在相信什么
STATE        = 我们现在做到哪里
DISCOVERY    = 我们一路学到了什么
EXPERIMENTS  = 我们实际上做过什么
LITERATURE   = 外部研究告诉了我们什么
REVIEWS      = 独立 Reviewer 如何评价关键实验
RESOURCES    = 代码、数据、算力等外部资源在哪里
```

核心关系：

```text
EXPERIMENTS  → What happened?
DISCOVERY    → What did we learn?
STORY        → What do we currently believe?
```

Chat history 不是科研事实来源；workspace 文件才是。

## 文件职责

### PROJECT.md

长期稳定的科研定义。极少修改。

包含：research goal、primary scientific problem、target performance direction、key datasets、evaluation principles、persistent constraints、Story 完成条件。

完成条件中的阈值是预设目标，实测数字只写 EXPERIMENTS。

不包含：当前 Story 细节、实验数字、临时 blocker、Discovery 历史。

### STORY.md

**系统核心**。固定六段：

```text
Problem → Key Observation → Core Idea → Evidence → Boundary → Open Gaps
```

要求：单一当前 Story；约一页；不记录具体性能数字；是"论文摘要的科研版"。

小改（Evidence / Boundary / Open Gaps / 表述）Agent 可自主完成。大改（Problem / Key Observation / Core Idea）建议针对触发实验调用 Reviewer。

### STATE.md

**必须很小**（几十行）。只回答：

- 现在主要在解决什么？
- 当前活跃实验是什么？
- 最近完成了什么？
- 下一步最值得做什么？
- 有哪些 blocker？
- 最相关文件在哪里？

目标：陌生 Agent 读 `PROJECT + STORY + STATE` 后几分钟内理解项目位置。

### DISCOVERY.md

科学经验积累，解释"为什么形成当前 Story"。

结构：

```text
Current Scientific Understanding
Positive Discoveries
Negative Discoveries
Null / Inconclusive Findings
Invalidated Findings
Open Contradictions
Research Evolution
```

重要 Discovery 标注 `Evidence: EXP-xxx`。普通 debug（路径错误、语法错误）不进入。

### EXPERIMENTS.md

**所有实验的唯一完整总账**。单文件，每个 Experiment 永久保留自己的 section。

ID 格式：`EXP-001`、`EXP-002` … 可附名称 `EXP-031 — Candidate Ambiguity Screening`。

字段、Status、索引表格式见 `experiment-record.md`。Experiment ≠ Run。

### LITERATURE.md

已进入当前科研认知的外部知识。每篇重要论文：Reference、Research Problem、Core Method、Important Finding、Relation to Our Story、Relation to Experiments、Possible Inspiration、Source。

不要求完整文献数据库；Zotero/Web/PDF 只是获取工具。

### REVIEWS.md

Reviewer 与 Experiment 一一关联的总览。完整 Review 在 `.research/reviews/EXP-xxx/`；摘要写入 `REVIEWS.md`。

并非每个 Experiment 都必须 Review。优先：高成本实验前、新核心方法、异常结果、关闭重要路线、Story 核心机制大改、认定 Story 完成。

### RESOURCES.md

连接 workspace 与外部环境。记录**资源身份 + 定位提示**（Codebases、Datasets、Compute、External Capabilities）。

路径失效时 Agent 应重新定位并更新，而非认为项目失效。

## 反重复规则

### 禁止复制的内容

| 来源 | 禁止写入 |
|------|----------|
| EXPERIMENTS | STORY（完整实验叙述、性能数字） |
| EXPERIMENTS | DISCOVERY（逐条复制 Main Findings） |
| DISCOVERY | EXPERIMENTS（长篇解释代替实验记录） |
| DISCOVERY | STORY（整段 Discovery 粘贴） |
| LITERATURE | STORY（文献综述式堆砌） |
| REVIEWS | EXPERIMENTS（代替 Main Findings） |
| 任意文件 | STATE（历史流水账） |
| Chat / session | 任意状态文件 |

### 允许的最小交叉引用

- DISCOVERY 中写 `Evidence: EXP-xxx`，不复制实验全文。
- STORY Evidence 段写机制性结论，不抄数字；数字留在 EXPERIMENTS。
- STATE 用 `EXP-xxx`、文件名指针，不展开实验细节。
- REVIEWS.md 写结论摘要，完整 Review 只在 `reviews/EXP-xxx/`。

### 单一事实来源（§20）

| 主题 | 唯一定义处 |
|------|-----------|
| Story 语义与六段结构 | `story-maintenance` skill + 本文件 STORY 段 |
| Experiment 字段与 Status | `experiment-record.md` |
| Git / Code 绑定 | `git-linking.md` |
| 科研循环与 gap 优先级 | `story-loop.md` |
| 宿主调用方式 | `adapters/` |

禁止把同一规则复制到多个 Skill 或多个状态文件。

## 更新顺序

重要科研进展后默认：

```text
EXPERIMENTS → DISCOVERY → STORY（如需要）→ STATE
```

LITERATURE 与 REVIEWS 按任务独立更新，不插入上述链的中间替代 DISCOVERY。

## 尺寸建议

| 文件 | 目标 |
|------|------|
| AGENTS.md | 尽量 < 150 行 |
| STATE.md | 几十行 |
| STORY.md | 约一页 |
| EXPERIMENTS.md | 可长；顶部索引表 + 各 section；老实验可压缩正文 |

新 Agent 应快速理解，不应为恢复上下文而加载大量无关规则。

## Story 完成条件

同时满足：

1. 达到项目预设主要性能目标
2. 形成完整、可发表的机制 Story
3. 独立 Reviewer 批准

完成后 `STATE` 可标记 `READY_FOR_WRITING`；workspace 仍可继续补实验与整理。

## 相关 reference

- 实验字段：`experiment-record.md`
- Git 绑定：`git-linking.md`
- 科研循环：`story-loop.md`
