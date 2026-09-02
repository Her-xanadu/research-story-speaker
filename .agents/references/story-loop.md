# Story Loop Reference

V0.1 规范：Story 驱动的科研循环、gap 优先级、反重复与停滞处理。

## 核心循环

整个框架围绕**唯一的当前 `STORY.md`** 运转：

```text
STORY
  ↓
识别当前 Story 最薄弱的环节（最大 gap）
  ↓
提出最有价值的科学问题
  ↓
Literature / Experiment / Review
  ↓
获得新证据
  ↓
更新 EXPERIMENTS
  ↓
提炼 DISCOVERY
  ↓
必要时修改 STORY
  ↓
更新 STATE
  ↓
进入下一轮
```

默认记忆更新链的唯一定义见 `state-files.md` §更新顺序。

## Gap 优先级

每轮先读 `STORY.md` 六段，重点看 **Open Gaps** 与 **Boundary**。

判断"当前最大 gap"时，按以下优先级（高→低）：

1. **能改变核心判断的 gap** — 若解决/否定该问题，会显著改变 Problem、Core Idea 或是否继续该路线。
2. **阻塞 Story 完成的 gap** — 与 PROJECT 中完成条件或 STORY Boundary 直接相关。
3. **可被现有资源快速验证的 gap** — 在 RESOURCES 可达的代码/数据/算力下可设计实验或文献检索。
4. **低成本、高信息量的探索** — 优先于大规模 parameter sweep。

选择 Literature / Experiment / Review：

| 情形 | 优先动作 |
|------|----------|
| 外部知识不明、新颖性存疑 | Literature |
| 已有假设需实证检验 | Experiment |
| 高成本、新核心方法、异常结果、Story 核心将大改 | Review（method 或 result） |
| 证据已足但 Story 表述混乱 | story-maintenance + STATE |

实验设计原则（与 `experiment-design` skill 一致）：

> 优先设计能够**最大程度改变当前科研判断**的实验，而不是大量 parameter sweep。

## 反重复

开始新工作前，先查：

```text
STORY（Open Gaps）
STATE（活跃实验与 Next）
EXPERIMENTS 索引表 + 相关 section
DISCOVERY（尤其 Negative / Invalidated）
```

规则：

- **同一科学问题**不拆成多个 EXP-ID；新假设用新 Experiment，旧 EXP 标 `superseded`。
- **已否定路线**（DISCOVERY Invalidated / Negative）不在无新机制理由时重做。
- **EXPERIMENTS 与 DISCOVERY 不重复长文** — 事实在 EXPERIMENTS，提炼在 DISCOVERY；见 `state-files.md`。
- **合并重复工作** — 发现并行 Agent 或历史 session 重复同一实验时，合并记录并更新索引表。
- 若状态文件膨胀、DISCOVERY 与 EXPERIMENTS 大量重复、新 Agent 难以理解 → 调用 `research-memory` skill 整理，**不删除有价值的负结果**。

## 停滞处理

V0.1 **不**建立严格科研状态机；Skills 是 strong guidance，不是 mandatory workflow。停滞时 Agent 可自主调整路线，但应遵守以下底线。

### 识别停滞

出现以下信号时视为停滞，应主动换策略：

- 连续多轮实验未改变 STORY 的 Problem / Core Idea / 主要 Open Gap。
- 反复跑相似配置（仅改超参）而 Interpretation 无新信息。
- STATE 长期指向同一 `running` 实验而无 Discovery 产出。
- Open Contradictions 增加但无新实验针对矛盾本身。

### 推荐动作（按顺序考虑，非强制）

1. **重新评估 gap** — 当前做的是否仍是"最大 gap"？读 DISCOVERY 与 STORY，必要时缩小或重写 Open Gaps。
2. **放弃低价值路线** — 将 EXP 标为 `abandoned`，在 DISCOVERY 记录原因；转向更高信息量实验。
3. **文献补课** — 若停滞来自知识缺口而非证据缺口，派 literature-scout 或 literature-research。
4. **独立视角** — 派 research-lead 分析 bottleneck；对关键 EXP 做 method/result review。
5. **Story 大改前 Review** — 若拟改 Problem / Key Observation / Core Idea，建议 Reviewer 介入后再改 STORY。
6. **维护整理** — 运行 research-memory 压缩 STATE、理顺 DISCOVERY，避免在冗余历史中"空转"。

### 自主性底线（§17）

```text
重要实验要能找到代码和结果
重要 Git 实验要能定位 commit
重要历史不能静默删除
Story 大改最好 Review
并行 Agent 不要互相覆盖同一状态文件
```

除此之外：可创建/停止实验、调整顺序、换工具、用 MCP、派 Subagent、跳过不适用 Skill 步骤。

## 启动与恢复

陌生 Agent 或新 session：

```text
1. AGENTS.md
2. PROJECT → STORY → STATE
3. 按任务 + DISCOVERY / EXPERIMENTS / LITERATURE / REVIEWS / RESOURCES
```

`workspace-resume` skill 应回答：Current Story、Current Gap、Latest Relevant Evidence、Active Experiment、Recommended Next Action，然后**继续执行**，不只汇报。

## 相关 reference

- 状态文件职责与反重复：`state-files.md`
- 实验记录与 Status：`experiment-record.md`
- Git 与结果追溯：`git-linking.md`
