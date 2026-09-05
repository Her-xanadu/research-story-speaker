# Story Loop Reference

Story 驱动的科研循环：两层 Workflow、gap 优先级、反重复与停滞处理。

Workflow Position 的唯一定义处：`state-files.md` §STATE.md。编排器：`research-loop` skill。

## 关键修正

**不要把每一次 Experiment 都机械地走 `W1 → W2 → W3 → W4 → W1`。**

对几十轮实验优化，把 **W2–W3–W4 组成快速内循环**，只有当 Story、机制或研究方向真的发生变化时才回到 W1。

```text
实验 → 结果 → 调整方法 → 再实验 → Story 小改 → 再实验
→ 方法失败 → 换机制 → Story 大改 → 再实验
```

Workflow 是**认知阶段**，不是 Agent 调用次数。一个很小的实验可以在**同一个 turn** 走完 `W2 → W3 → W4 → W2`。STATE 只记**下一休息点**（通常 `W2 TEST` + 下一 EXP），不记「刚刚经过 W3/W4」——历史在 EXPERIMENTS / DISCOVERY。

Skills 是 strong guidance，不是 mandatory state machine。

## Method-First Principle

```text
科研循环的默认推进单位是“科学判断的改变”，
不是“完成了一个工程任务”。

每一轮优先问：

当前方法最值得验证或修改的是什么？

然后设计能够改变这个判断的最小实验。

工程、数据、环境、资格和审查任务
只在直接阻塞当前科学实验时执行，
完成后立即返回当前方法实验。
```

## 两层 Workflow

### 最终转移

```text
W0 SETUP
   ↓
W1 FRAME
   ↓
W2 TEST
   ↓
W3 LEARN
   ↓
W4 DECIDE
   ├── same route, next test clear ─────────→ W2
   ├── Story / mechanism needs reframing ──→ W1
   ├── evidence needs another look ────────→ W3
   └── Story complete ─────────────────────→ W5
```

### 大循环（低频）

```text
W1 → W2 → W3 → W4 → W1
```

负责：**换问题、换 Story、换机制、换研究路线。**

### 内循环（默认高频）

```text
W2 → W3 → W4 → W2
```

负责：**同一 Story 下不断实验优化。** 这是未来最常使用的循环。

## Method-First Inner Loop

不是新的 Workflow Stage，也不是 STATE 字段。它只是 `W2–W3–W4` 的认知主线：

```text
Current Method Hypothesis
        ↓
Weakest Mechanism Assumption
        ↓
Minimum Discriminating Experiment
        ↓
Observed Result
        ↓
Mechanism Diagnosis
        ↓
Method Consequence
   ├─ keep
   ├─ simplify
   ├─ modify
   ├─ replace component
   └─ abandon mechanism
        ↓
Next Discriminating Experiment
        ↓
W2 TEST
```

不要写成 `M1` / `M2` / `M3` 进 STATE。

见上文 Method-First Principle。

工程、数据、环境、资格和审查任务只在直接阻塞当前科学实验时执行；完成后立即返回当前方法实验。**support work does not advance the scientific loop by itself.**

长周期默认思考：

```text
当前 Story 的核心方法是什么？
↓
当前方法最脆弱的机制假设是什么？
↓
什么实验最能区分它和最强替代解释？
↓
运行 → 机制诊断 → 方法更新 → 下一判别实验
```

不要默认先问：还有什么文件没审、schema 没补、资格门没检查、对象可以 Audit。

### 长周期主链

```text
W0 SETUP
   ↓
W1 FRAME
   ↓
╔══════════════════════════════════════╗
║              INNER LOOP              ║
║    W2 TEST → W3 LEARN → W4 DECIDE   ║
║       ↑                    │          ║
║       └──── Story stable ──┘          ║
╚══════════════════════════════════════╝
                          │
                    Story/route changes
                          ↓
                         W1

W4 + completion criteria satisfied
                         ↓
                     W5 HANDOFF
```

### 阶段职责

| 阶段 | 职责 | 典型 owner |
|------|------|------------|
| W0 SETUP | 算力、代码 Git | `workspace-setup` |
| W1 FRAME | 当前科学问题；文献；换机制/路线；大改 Story | `research-loop` + `literature-research` / `idea-evaluation` / `story-maintenance` |
| W2 TEST | 设计 / 实现 / 跑实验 | `experiment-design` → `experiment-execution` |
| W3 LEARN | 结果 → Outcome / Discovery | `result-analysis` |
| W4 DECIDE | 科研换挡器（五问，见下） | Level 0/1 且 Next 清楚：`result-analysis`；Level 2 / 停滞 / 完成：`research-loop` |
| W5 HANDOFF | Story 完成可写 | `Story Status: READY_FOR_WRITING` |

文献**不是**独立 W 阶段：属于 W1，或 W4 判定知识缺口后回 W1。

`scout` / `focus` / `confirm` 是 `research-loop` 内部选路透镜，**禁止**写入 STATE（避免两套阶段词）。

### Agent 连续性口诀

```text
先问方法，不先问流程。
每个新 EXP 都要改变一个科学判断。
结果出来先诊断机制，再决定下一实验。
同一方法还能被有效检验，就留在 W2–W3–W4。
核心机制需要重构，才回 W1。
工程问题只做最小修复，修完立刻回实验。
历史按需查，STATE 只保留现在。
Story 随证据演化，不随任务日志膨胀。
```

W 阶段口诀（同上，不另建状态机）：

```text
W2 → 当前路线还没测完，继续
W3 → 结果出来了，先诊断机制，别先想新 Idea
W4 → 结果对方法意味着什么？Story 要不要变？默认回 W2
W1 → 只有真正需要换问题时才重新 frame
```

## 工作示例（几十轮内循环）

Story：`candidate ambiguity 可以提升 noisy-label detector`

Gap：提升来自 ambiguity 还是 source-group balancing？

W1 FRAME 后问题清楚：

```text
EXP-021  W2 → W3 → W4   Full ≈ ambiguity-off
```

**不要重新跑完整 W1。** 问题仍清楚 → `W4 → W2` → EXP-022 → … → EXP-023 mechanism isolation → 可连续十轮 `W2 → W3 → W4 → W2`。

即使 50 个 Experiment、20 次 Story 小更新、5 次 Core Idea 大改、3 次 pivot，仍只维护：

```text
一个 Story · 一个 Workflow Position · 一个 STATE 游标
· 一个 EXPERIMENTS ledger · 一个累积 DISCOVERY
```

## 何时回到 W1（仅三类）

Level 1 小改且下一实验清楚 → **仍回 W2，不是 W1。**

### A. Core Idea 被推翻

连续实验表明原机制不成立 → `W4 → W1`，重新 frame 真正值得解释的现象（Story Impact Level 2）。

### B. 当前路线无信息增益

连续 EXP 仅改 lr / weight / threshold / seed，Story 与 Discovery 无变化 → `W4 → W1`，重新找 bottleneck（见 §停滞处理）。

### C. 出现需重新 frame 的矛盾

如 dataset A 支持、B 相反 → 不是再跑一个 seed → `W4 → W1`，问机制在什么 regime 下成立。

## W4 换挡器

每轮五个问题：

```text
1. 这次结果是否可靠？
2. 它是否改变了我们对 Story 的相信？
3. 下一步还是同一个科学问题吗？
4. What does this result imply for the method?
   keep? simplify? delete component? change mechanism?
   change control? abandon mechanism?
5. 下一阶段是什么？
```

长周期默认出口是 **W2 TEST**。若 Story 没有 Level 2 改变、当前科学问题仍成立、还有明确可区分的下一实验 → `W4 → W2`，而不是默认回 W1。

下一动作优先是判别实验（mechanism-off、simpler replacement、rival hypothesis、remove component、matched sham）。低优先：more seeds / epochs / thresholds / 次要参数，除非 variance 本身就是当前科学问题。

| W4 判断 | 下一 Workflow Position |
|---------|-------------------------|
| Story 稳定，下一实验清楚 | `W2 TEST`（默认） |
| 结果还没解释清 | `W3 LEARN` |
| Core Idea / Gap / route 需重构（A/B/C） | `W1 FRAME` |
| Story 完成 | `W5 HANDOFF` |

谁执行 W4：Level 0 且 Next 清楚 → `result-analysis` 直接写 STATE `W2`；Level 1 → `story-maintenance` 后 `W2`；Level 2 / A/B/C / Next 不清 → `research-loop`。

### Method Complexity Rule

实验不支持当前方法时，默认顺序：

```text
1. identify failed prediction
2. remove unsupported component
3. test simpler explanation
4. only then consider adding a mechanism
```

禁止默认「结果差 → 加组件 / 加 loss / 加 gate / 加 expert」。每次新增组件必须回答：`Which failed prediction requires this component?` 若答案只是 `might improve performance`，默认不加。

优先 deletion experiment：`Full` vs `Full - Component A`，而不是 `Full + B + C + D`。

### Method Check（每 3–5 个有意义科学 EXP）

不是 Reviewer，不是新 Workflow，不要求单独 artifact。Main 在 W4 简单问：

```text
最近这些实验是否改变了机制理解？
当前方法是否比 5 个 EXP 前更复杂？
是否有组件已经失去证据支持？
当前 focal gap 还是最重要的吗？
```

有新理解 → 继续 W2。方法越来越复杂但无新理解 → W1 reframe。

### 方法内循环与 Story

很多正常 Experiment 只需更新 `EXPERIMENTS` + `DISCOVERY` + `STATE`。Story 可以连续 3 / 5 / 10 个 EXP 不修改——这不是遗漏，是正确分层。长周期方法演化写在 DISCOVERY（机制为什么变化），等证据成熟后再改 Story Core Idea。

文献不在同一机制的 `W2 → W3 → W4 → W2` 里重做。只有 W1 reframe、novelty threat、新机制、新 baseline 必须加入、或用户明确要求 freshness 时才做新 literature work。

## Story Impact Level

写在 EXPERIMENTS 的 **Story Impact** 一行：`Level 0|1|2 — {{对六段的影响}}`。不是 Outcome 或 Verdict。

| Level | STORY 改动 | W4 后典型 Position |
|-------|------------|-------------------|
| **0** | 不动（seed、sanity、小 ablation、inconclusive、工程失败） | `W2 TEST` |
| **1** | 仅 Evidence / Boundary / Open Gaps（小改） | `W2 TEST`（下一实验清楚时） |
| **2** | Problem / Key Observation / Core Idea（大改） | `W1 FRAME`；建议 Reviewer |

小改 Story 后**不**重启大循环；大改才回 W1。

## 三层记忆

```text
EXPERIMENTS = 所有实验事实（EXP-001 …）
DISCOVERY   = 十几轮以后学到了什么
STORY       = 今天最终还相信什么（约一页六段）
```

STORY **不**写逐条 EXP 流水账。历史为何变成今天这个 Story → DISCOVERY；每一步 → EXPERIMENTS。

默认记忆更新链：`state-files.md` §更新顺序。

```text
EXPERIMENTS → DISCOVERY → STORY（如需要）→ STATE
```

## Gap 优先级

在 **W1 FRAME** 或 `research-loop` 需要重新 frame 时使用。`STATE` 已是 `W2 TEST` 且 Recommended Next Action 已点名普通 EXP 时，**不要**重跑「最大 gap」。

每轮先读 `STORY.md` 六段，重点 **Open Gaps** 与 **Boundary**：

1. **能改变核心判断的 gap** — 会显著改变 Problem、Core Idea 或是否继续该路线。
2. **阻塞 Story 完成的 gap** — 与 PROJECT 完成条件或 Boundary 直接相关。
3. **可被现有资源快速验证的 gap** — RESOURCES 可达下可设计实验或文献。
4. **低成本、高信息量探索** — 优先于大规模 parameter sweep。

| 情形 | 优先动作 |
|------|----------|
| 外部知识不明、新颖性存疑 | Literature（W1） |
| 已有假设需实证检验 | Experiment（W2 内循环） |
| 高成本、新核心方法、异常、Story 核心将大改 | Review |
| 证据已足但 Story 表述混乱 | story-maintenance |

> 优先设计能**最大程度改变当前科研判断**的实验，而不是 parameter sweep。

## 反重复

开始新工作前查：

```text
STORY（Open Gaps）· STATE（Workflow Position、Active Experiment、Next）
EXPERIMENTS 索引 + 相关 section · DISCOVERY（Negative / Invalidated）
```

- 同一科学问题不拆多个 EXP-ID；新假设新 Experiment，旧 EXP 标 `superseded`。
- 已否定路线不在无新机制理由时重做。
- EXPERIMENTS 与 DISCOVERY 不重复长文。
- 合并并行 Agent 重复工作。
- 文件膨胀 → `research-memory`，**不删**有价值负结果。

## 停滞处理

不建立严格状态机。停滞信号 → 通常 `W4 → W1`（B 类：无信息增益）。

### 识别停滞

- 连续多轮未改变 Problem / Core Idea / 主要 Open Gap。
- 反复相似超参，Interpretation 无新信息。
- STATE 长期 `running` 无 Discovery。
- Open Contradictions 增加但无针对实验。
- **方法没有学到东西：** 连续实验只改变数字（lr / threshold / seed / weight / epochs），但不改变任何机制判断（Interpretation / Discovery / Core Idea 都不变）→ 就是停滞 → `W4 → W1`。

### 推荐动作（非强制）

1. **重新评估 gap** — 是否仍为最大 gap？→ 常需 `W1 FRAME`。
2. **放弃低价值路线** — EXP `abandoned`，DISCOVERY 记原因。
3. **文献补课** — 知识缺口 → `literature-research`（W1）。
4. **独立视角** — `research-lead`；关键 EXP review。
5. **Story 大改前 Review** — Level 2 前建议 Reviewer。
6. **维护整理** — `research-memory`。

### 自主性底线

```text
重要实验要能找到代码和结果
重要 Git 实验要能定位 commit
重要历史不能静默删除
Story 大改最好 Review
并行 Agent 不要互相覆盖同一状态文件
```

## STATE 游标规则

`STATE.md` 必须很小。在 `Current Focus` 前有 **Workflow Position** 封闭枚举：

`W0 SETUP` | `W1 FRAME` | `W2 TEST` | `W3 LEARN` | `W4 DECIDE` | `W5 HANDOFF`

| 结束时情况 | 写入 Position |
|------------|----------------|
| 内循环结束、下一实验清楚 | `W2 TEST` |
| 需重构问题/机制/路线 | `W1 FRAME` |
| Story 完成 | `W5 HANDOFF` |
| 结果还没解释清 | `W3 LEARN` |
| 等人或 Reviewer | `W4 DECIDE` |
| UNINITIALIZED / 未 setup | `W0 SETUP` |

长周期示例见 `state-files.md` §STATE.md。并行实验：`EXP-041 (primary); EXP-042/043 parallel`，仍**一个** Position。

限制的是 **one focal scientific question**，不是「只能有一个 science EXP」。同一焦点下可并行 mechanism-off / matched sham / second dataset / 多 seed。进入 W2 focus 后，不允许多路线无边界并行（Route A/B/C/D 全部深入），除非 W1 明确处于 scout。Support tasks 可并行，但不能成为主轴。

**禁止**作为正式 STATE 字段：`Research Round`、`Story Revision`、`Iteration`、每条路线一套 Workflow。一个 Project 只有 **one Workflow Position**；候选 route 在 DISCOVERY / `.research/work/` / idea-evaluation。

W 名、Level、scout/focus/confirm **不是** Outcome 或 Verdict。

## 启动与恢复

```text
1. AGENTS.md
2. PROJECT → STORY → STATE（先读 Workflow Position）
3. STATE 指向的当前 EXP section + 该 EXP 指向的最新 artifact
```

默认到此停止。不要全读 EXPERIMENTS / REVIEWS / 全部 DISCOVERY，不要扫描 `work/`。细节：`state-files.md` §Scientific Working Set。

`workspace-resume` 应回答：Workflow Position、Current Gap、Active Experiment、Recommended Next Action，然后**继续执行**。

- Position `W2 TEST` 且 Next 已点名普通 EXP → compact 内循环，**不要** `research-loop` 重 frame。
- Position `W1 FRAME` 或 Next 不清 → `research-loop` 或 resume 后进入 FRAME。

## 相关 reference

- 状态文件职责：`state-files.md`
- 实验记录：`experiment-record.md`
- Git 追溯：`git-linking.md`
