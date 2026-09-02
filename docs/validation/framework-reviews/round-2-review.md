# Gate 2 Round 2 — Reviewer R2

审核对象：Story-Driven Research Workspace V0.1 Skills / Subagents / Prompts（Phase 4–6）
审核日期：2026-09-02
身份：独立 Reviewer R2（fresh context，与开发者不同模型）
范围：仅 `.agents/skills/*/SKILL.md`（10）、`.agents/subagents/*.md`（5）、`.agents/prompts/*.md`（4）
只读参考：`.agents/references/*.md`、`AGENTS.md`
规格：计划 §11.1–11.10、§12、§13、§17、§20、§21 Phase 5 / Phase 11、§9 Subagents
未改：除本文件外未修改任何 workspace / 计划文件

---

## 清单核对

### 数量

| 类别 | 规格 | 实际 | 结果 |
|------|------|------|------|
| Skills | 10 | 10：`research-loop`、`workspace-resume`、`story-maintenance`、`literature-research`、`experiment-design`、`experiment-execution`、`result-analysis`、`experiment-review`、`research-memory`、`framework-maintenance` | 通过 |
| Subagents | 5 | 5：`research-lead`、`literature-scout`、`experiment-agent`、`result-analyst`、`reviewer` | 通过 |
| Prompts | 4 | 4：`method-review`、`result-review`、`experiment-review`、`subagent-handoff` | 通过 |
| 脚本依赖 | 0 | `.agents/skills|subagents|prompts` 下无 `.py/.sh/.js/.ts`；Skill 正文无运行时脚本调用 | 通过 |

`AGENTS.md` Skill Routing 表 10 个名字与目录一一对应，无幽灵 Skill 名。

### 命名与 frontmatter

每个 `SKILL.md` 的 YAML `name` **等于**目录名。`description` 均含触发场景（`Use when` / 中英触发词如「查文献」「设计实验」「接着做」）。

五个 subagent 的 `name` 与文件名一致。四个 prompt 无 frontmatter（规格未要求），文件名与 §13 一致。

### 行数

指引约 120 行；§20 明确不强制卡行，禁止的是 500–700 行单文件。

| 文件 | 行数 | 相对 ~120 | 备注 |
|------|------|-----------|------|
| `.agents/skills/experiment-design/SKILL.md` | 104 | 内 | |
| `.agents/skills/experiment-execution/SKILL.md` | 98 | 内 | |
| `.agents/skills/experiment-review/SKILL.md` | 119 | 内 | |
| `.agents/skills/framework-maintenance/SKILL.md` | 135 | 略超 | Phase 11 八项检查表有必要；另含可下沉的 §19/§20 复述（见 MAJOR-7） |
| `.agents/skills/literature-research/SKILL.md` | 106 | 内 | |
| `.agents/skills/research-loop/SKILL.md` | 132 | 略超 | 调度器复述了 gap 优先级/反重复（见 MAJOR-2） |
| `.agents/skills/research-memory/SKILL.md` | 116 | 内 | |
| `.agents/skills/result-analysis/SKILL.md` | 105 | 内 | |
| `.agents/skills/story-maintenance/SKILL.md` | 112 | 内 | |
| `.agents/skills/workspace-resume/SKILL.md` | 112 | 内 | |
| `.agents/subagents/research-lead.md` | 79 | 短 | 符合 §12 / §20 |
| `.agents/subagents/literature-scout.md` | 92 | 短 | |
| `.agents/subagents/experiment-agent.md` | 93 | 短 | |
| `.agents/subagents/result-analyst.md` | 92 | 短 | |
| `.agents/subagents/reviewer.md` | 95 | 短 | |
| `.agents/prompts/method-review.md` | 67 | — | |
| `.agents/prompts/result-review.md` | 66 | — | |
| `.agents/prompts/experiment-review.md` | 72 | — | |
| `.agents/prompts/subagent-handoff.md` | 66 | — | |
| `AGENTS.md`（参考） | 87 | <150 | 未塞进完整科研流程 |

无 Skill 接近 500 行。尺寸本身不构成 Gate 失败。

### Phase 5 六要素

十个 Skill **均含**：什么时候用 / 主要目标 / 默认流程 / 需要读取什么 / 完成后更新什么 / 允许合理偏离。标题统一为 `When to use`、`Goal`、`Default flow`、`Reads`、`Updates`、`Deviation allowed`。

未发现强制审批锁、禁止偏离、或「必须逐步询问用户」的硬状态机。`story-maintenance` 写明 Reviewer 是 strong guidance, not a hard lock。`research-loop` 明确反对 fixed state machine（措辞有语病，见 MINOR-1）。编号步骤是默认流程，不是引擎。

### 职责边界速查（对照 §11）

| Skill | 规格要点 | 本轮判断 |
|-------|----------|----------|
| research-loop | 只调度，不实现文献/实验/review | 通过（有 SSOT 复述，见 MAJOR-2） |
| workspace-resume | 五问之后**继续执行**，不只汇报 | 通过（Goal + flow §4 + 「Never end with only 已恢复」） |
| story-maintenance vs research-memory | STORY 科学改写 vs 压缩/一致性 | 基本清楚；边缘句见 MINOR-3 |
| experiment-execution | 运行成功 ≠ 科学成功 | 通过（Goal 段原文） |
| result-analysis | §11.7 八问 | 通过（flow 步骤 2 中文八问齐全） |
| experiment-review | method/result 二分；写入 `reviews/EXP-ID/` + `REVIEWS.md` | 通过 |
| research-memory | 不得删除有价值负结果 | 通过（Goal + Updates Never delete） |
| framework-maintenance | Phase 11 八项；不做科研 | 通过（checklist 1–8 + Boundaries） |

### Prompts §13 四字段

四个 prompt 均出现 `EXP-ID`、`Story gap`、`Relevant files`、`Required output`。均要求 Reviewer/Subagent **自己读盘**，禁止把状态文件全文粘进 dispatch。`subagent-handoff.md` 规定 subagent 只写 `.research/work/` 或 `.research/reviews/<EXP-ID>/`，不改八个 canonical 状态文件。

---

## R2 审计

### CRITICAL

**C1. 同一条记忆更新规则被复制进五个 Skill（直接触犯 §20「禁止同样规则复制到五个 Skill」）**

规则原文（canonical 已在 `state-files.md`「更新顺序」与 `story-loop.md`「默认记忆更新链」）：

```text
EXPERIMENTS → DISCOVERY → STORY (if needed) → STATE
```

完整出现位置：

| 文件 | 行 |
|------|----|
| `.agents/skills/research-loop/SKILL.md` | 87–90（括号写成 `STORY (story-maintenance)`） |
| `.agents/skills/workspace-resume/SKILL.md` | 98–102 |
| `.agents/skills/story-maintenance/SKILL.md` | 65–69 |
| `.agents/skills/result-analysis/SKILL.md` | 62–66 |
| `.agents/skills/research-memory/SKILL.md` | 103–106 |

另：`AGENTS.md` 33–37 行、`.agents/prompts/subagent-handoff.md` 57 行也各抄一遍（不在本 Gate 的修改权内，但说明泄漏面更大）。

建议修法：十个 Skill 全部改为链接 `[state-files.md](../../references/state-files.md) §更新顺序`（或 `story-loop.md` 选一处为唯一指针）。需要强调顺序的 Skill（`result-analysis`、`story-maintenance`）只写「按 reference 链更新，本 Skill 负责其中 STORY/DISCOVERY 哪一环」，不要再贴四段箭头。`research-loop` / `workspace-resume` / `research-memory` 删掉代码块。

---

### MAJOR

**M1. `research-loop` 把 `story-loop.md` 的核心规则又写了一遍，调度器膨胀成第二份循环规范**

位置：`.agents/skills/research-loop/SKILL.md`

- 59–63 行：gap 优先级四条（改变 Problem/Core Idea → 阻塞完成 → RESOURCES 可验证 → 低成本高信息），与 `story-loop.md` 37–46 行同构。
- 51–54 行：反重复扫描清单（Open Gaps、STATE Next、EXPERIMENTS、DISCOVERY Negative/Invalidated），与 `story-loop.md` 61–78 行同构。
- 99–100 行：停滞处理缩写（尚有「see story-loop reference」，好于前两处）。

§11.1 要求它只做调度。现在一个陌生 Agent 读完 loop Skill 就「够用」，reference 变成可选——这正是 SSOT 失效的症状。

建议修法：删除四条优先级与反重复清单的正文；改成「按 `story-loop.md` Gap 优先级 / 反重复 判断，本 Skill 只输出路由表并 invoke」。停滞段保持「信号 → 链到 reference」即可。目标把该文件压回 ~100 行。

**M2. Experiment 路由未点名三个 Skill，陌生 Agent 无法从 loop 一跳走到设计/执行/分析**

位置：`.agents/skills/research-loop/SKILL.md` 69–73 行路由表。

| Gap nature | Route | Delegate |
| --- | --- | --- |
| … Literature | `literature-research` / scout | 已点名 |
| Untested mechanism… | Experiment | **`design → execution → analysis`**（未点名） |
| … Review | `experiment-review` / reviewer | 已点名 |
| … Story | `story-maintenance` | 已点名 |

`AGENTS.md` 路由表有三个全名，但 `research-loop` 是循环中的调度入口。对照检查清单第 10 条：拿到 Story gap 后应能沿 **research-loop → 对应 Skill → subagent prompt** 走通。当前 Experiment 支路要猜目录名。

建议修法：Delegate 列改为 `` `experiment-design` → `experiment-execution` → `result-analysis` ``（可加「并行时 `experiment-agent` / `result-analyst`，handoff 用 `subagent-handoff.md`」一句）。不要在 loop 里展开三 Skill 的流程。

**M3. Reviewer 独立性只写「优先不同模型」，没有「拿不到不同模型时怎么办」**

规格 §12 列出：不同模型 / 不同模型家族 / fresh context / 外部 Reviewer MCP。检查清单第 6 条明确要求降级策略。

| 文件 | 现有表述 | 缺口 |
|------|----------|------|
| `.agents/subagents/reviewer.md` 19 行 | Prefer different model, fresh context, or external Reviewer MCP | 未写模型家族；无降级 |
| `.agents/skills/experiment-review/SKILL.md` 47–48 行 | Prefer different model, different model family, fresh context, or `reviewer` subagent | 无降级；未说明同模型是否仍可审 |

建议修法（只在 **一处** 完整定义，推荐 `reviewer.md`，Skill 链过去）：

1. 优先：不同模型家族 → 不同模型 → 外部 Reviewer MCP。
2. 最低独立：同一模型也可，但必须 **fresh context**（无 executor 聊天历史），且在 review 文首记录 `independence: same-model-fresh-context`（或等价字段）。
3. 禁止：与执行/分析同一会话、同一上下文里「自己审自己」。
4. 拿不到 subagent 机制时：Main Agent 仍可用 method/result prompt 开新会话执行，不得跳过记录独立性限制。

**M4. Reviewer 五段输出在四处完整定义**

规格要求 reviewer 不只 approve/reject，并给出五段。这是对的，但完整 heading 块应只在 **prompt + reviewer 角色** 之一为主，Skill 只链接。

| 位置 | 内容 |
|------|------|
| `.agents/subagents/reviewer.md` 69–86 行 | 五段 heading 全文 |
| `.agents/prompts/method-review.md` 41–58 行 | 同五段（method 视角填空） |
| `.agents/prompts/result-review.md` 40–57 行 | 同五段（result 视角填空） |
| `.agents/skills/experiment-review/SKILL.md` 62–70 行 | 再列一次 Strongest evidence / … / Recommended next move |

`method-review` 与 `result-review` 保留各自五段是合理的（§13 = 交给该 agent 的 exact task；填空不同）。重复源是 **Skill 再抄一份**。

建议修法：Skill 改为「输出结构以 active prompt 为准，最低五段见 `reviewer.md`」。删除 Skill 内 62–70 行列表。若希望 SSOT 更干净：五段 heading 只留在 `reviewer.md`，两个 prompt 写「使用 `reviewer.md` Required output headings，并按下列 method/result 问题填空」。

**M5. `experiment-review` prompt 与 Skill / 两个 task prompt 职责重叠**

`.agents/prompts/experiment-review.md` 定位是 Main Agent **编排** prompt，这与 Skill「How to do a workflow」撞车。

重叠点：

- 「When to review」26–34 行 ≈ Skill 17–24 行 ≈ `state-files.md` REVIEWS 优先情形（高成本前、新核心方法、异常结果、关路线、Story 大改、认定完成）。
- 「Orchestration steps」36–45 行 ≈ Skill Default flow 步骤 2–6。
- Task fields 四元组与 method/result 两个 prompt 的 Task fields 同构（§13 要求每个 prompt 都有四字段，这一块允许相似；不要再复制 When-to-review）。

未重叠、应保留的独特内容：Review decision matrix（47–54 行）、`REVIEWS.md` synthesis template（56–66 行）、「subagent 不写 REVIEWS.md」（45 行）。

建议修法：prompt 删除 When to review 与 Orchestration 长步骤，改为一句「何时审、如何派，见 `experiment-review` Skill；本文件只提供决策矩阵与 REVIEWS 摘要模板」。Skill 的 Reads 已链到该 prompt，保留即可。避免陌生 Agent 不知道先打开 Skill 还是 prompt。

**M6. `literature-research` 把 `LITERATURE.md` 八字段整表抄进 Skill**

位置：`.agents/skills/literature-research/SKILL.md` 62–65 行：

> Reference, Research Problem, Core Method, Important Finding, Relation to Our Story, Relation to Experiments, Possible Inspiration, Source

Canonical：`state-files.md` LITERATURE 段（约 91 行）+ `.agents/templates/LITERATURE.template.md`。§11.4 要求本 Skill 回答的是五透镜（Known / Conflicts / Supports / Suggests / Novelty）——Goal 表已经做对了。字段名单是状态格式，不应在 Skill 再列一遍。

建议修法：改为「按 template / `state-files.md` §LITERATURE.md 追加一节；本 Skill 只保证五透镜能映射到 Relation 字段」。删掉八字段 enumeration。

**M7. `framework-maintenance` 把 §19 adapter 规则、§20 归属表与尺寸建议整段搬进 Skill**

位置：`.agents/skills/framework-maintenance/SKILL.md`

- 43–50 行：Canonical owner 表（Story → `story-maintenance`；Experiment record → `experiment-record.md`；…）≈ `state-files.md` 129–137 行。
- 65–69 行：adapter 只回答六问、不定义科研 ≈ 计划 §19。
- 133–134 行：AGENTS <150 / STATE 几十行 / STORY 一页 / Skills ~100–130 ≈ `state-files.md` 151–158 行与 §20。

Phase 11 八项检查表（75–84 行）**属于本 Skill 职责，应保留**。被复制的是「规则正文」而不是「检查是否发生了这些漂移」。

建议修法：owner 表、adapter 六问、尺寸数字全部改成链接；检查表用「对照 `state-files.md` §20 / adapter 文件是否复制了科研逻辑」这种检查语言，不要在 Skill 里再定义一遍规则。这也能把 135 行收回到指引附近。

**M8. Git 绑定「最小五要素 + 路径恢复五步」在执行 Skill 内展开，而 canonical 已在 `git-linking.md`**

位置：`.agents/skills/experiment-execution/SKILL.md`

- 38–40 行：`remote → relative → search → ask user → update RESOURCES`（`git-linking.md` 路径恢复步骤的压缩版）。
- 50 行：`Codebase ID, Git repository, Git commit, Entry, Results root`（`git-linking.md` 12–23 行最小绑定，缺 Experiment ID 一项）。

§11.6 要求执行 Skill **保存** code location / git commit / result location / runs——点名「要记哪些事实」可以；把恢复算法与字段全集再写一遍不行。目前只有一个 Skill 展开，尚未到「五个 Skill」，故定为 MAJOR 而非 CRITICAL。

建议修法：改为「绑定字段与失效恢复只遵循 `git-linking.md`；本 Skill 负责在跑之前冻结 commit、跑之后把路径写进 EXPERIMENTS」。删除恢复五步枚举与字段逗号列表。

---

### MINOR

**N1. `research-loop` 底线句语法错误**

`.agents/skills/research-loop/SKILL.md` 131 行：`Do **not** fixed state machine` → 应为 `Do not use a fixed state machine`（或 `Do not turn this into a fixed state machine`）。

**N2. 循环默认流程读起来像状态机，尽管已有 Deviation 段**

同文件 43–100 行：步骤 1–6 + 「Default: loop to step 1」。§17 允许编号默认流程，但「loop to step 1」对陌生 Agent 偏强制。建议改成「若仍自主推进，回到 gap 判断；允许跳步」。

**N3. `story-maintenance` 与 `research-memory` 对 STORY 的写权限边界还可再钉一句**

- `story-maintenance`：科学信念改写；不改 EXPERIMENTS/DISCOVERY/LITERATURE。
- `research-memory` 92–100 行：可改全部七个状态文件；机制性改写 defer 给 `story-maintenance`。
- `experiment-execution` 81 行：允许对 STORY 做 `trivial factual corrections`——这会越过 story-maintenance。

建议：`research-memory` Updates 加一句「压缩/搬移细节，不改 Problem/Core Idea 的科学主张」。`experiment-execution` 删除「except trivial factual corrections」，STORY 一律不碰。

**N4. `result-analysis` 与 `research-loop` 再次复述「运行成功 ≠ 科学成功」**

Canonical：`experiment-record.md`（约 153 行）+ 执行 Skill Goal。又出现在：

- `research-loop` 92 行
- `result-analysis` 100 行

分析 Skill 提一次作为分析纪律可以接受；loop 不应重复。建议 loop 删除该句。

**N5. Story 六段名称在多个入口点名**

`story-maintenance` 作为 §20 指定的 Story 语义 owner，列出六段是正当的。额外出现：`research-loop` 47 行、`workspace-resume` 64 行、`research-memory` 57 行、`AGENTS.md` 74 行。建议非 owner Skill 只写「六段，见 `STORY.md` / story-maintenance」，不要再画 `Problem → … → Open Gaps` 箭头链。

**N6. `experiment-review` Skill 的 Traceability 代码块复制 §16 链路**

`.agents/skills/experiment-review/SKILL.md` 94–102 行。`git-linking.md` / `state-files.md` 已能走通 EXPERIMENTS → reviews → REVIEWS。建议改为一句链接。

**N7. `workspace-resume` 复述 AGENTS.md 的「chat history 不是记忆」**

`.agents/skills/workspace-resume/SKILL.md` 41 行。canonical 在 `AGENTS.md` Research Memory 与 `state-files.md` 26 行。改为「见 `AGENTS.md` / `state-files.md`」即可。

**N8. `experiment-agent` 与执行 Skill 对「谁写 EXPERIMENTS」的用词略混**

- `experiment-agent.md` 11、49 行：subagent 不写 canonical，Main Agent 更新。
- `experiment-execution/SKILL.md` 88 行：`Delegate … to experiment-agent; executor still owns workspace file updates.`

「executor」可能被读成 subagent。建议改成 `Main Agent (this skill) still owns EXPERIMENTS/STATE updates after the subagent returns`。

**N9. `research-loop` 未点名 `research-lead`**

`story-loop.md` 停滞处理第 4 步会派 `research-lead`。loop Skill 只说「Subagent per AGENTS.md」。建议在 Deviation 或 invoke 段加半句：独立判断下一步时派 `research-lead`（读 STORY/STATE/DISCOVERY，写 `.research/work/`），不要在 loop 进程内再写一份 bottleneck 分析规范。

**N10. 略超 120 行的两个 Skill**

`research-loop` 132、`framework-maintenance` 135。内容必要度取决于是否砍掉 MAJOR-1/M7 的复述。修完 SSOT 后应自然回落到指引附近；不必为行数而拆文件。

**N11. 两个 review task prompt 的「Do not paste / 五段 heading / 不改 canonical」骨架相似**

这是 §13 四字段 + 共用 reviewer 角色的预期成本，不是缺陷。只要 M4/M5 把 Skill/编排 prompt 上的第三、第四份拷贝拿掉即可。不要把 `method-review.md` 与 `result-review.md` 合并——二者问题清单不同，合并会损害 exact task。

---

### 对照检查清单的通过项（不写入必须修复）

以下项本轮**通过**，避免 owner 误修：

1. 10/5/4 齐全；`name` = 目录名；`description` 有触发场景。
2. 十个 Skill 均有 Phase 5 六要素；无审批锁/禁止偏离。
4. `workspace-resume` 继续执行；`experiment-execution` 强调运行≠科学成功；`result-analysis` 覆盖 §11.7 八问；`experiment-review` 区分 method/result 并写入 `reviews/EXP-ID/` + `REVIEWS.md`；`research-memory` 明确不得删负结果；`framework-maintenance` 含八项检查且不做科研；`research-loop` 未把文献检索步骤或跑实验命令写进自身。
6. 五个 subagent 均短；均有读取范围与返回格式，与 §12 字段对齐。`reviewer.md` 要求五段而非 approve/reject。`literature-scout.md` 11、45 行明确不改 Story（也不改 LITERATURE canonical）。
7. 四 prompt 均含四字段；均要求读盘不粘贴全文；handoff 禁止 subagent 改 canonical。
8. `AGENTS.md` 未指向不存在的 Skill。Skill 未复制 AGENTS 的整张路由表（仅有 chat-history 一句，见 N7）。
9. 零脚本依赖。
10. Literature / Review 支路可走通；Experiment 支路被 M2 挡住，修 M2 后可走通。

`literature-scout` 不改 Story：通过。Subagent 写 `.research/work/` + reviewer 写 `.research/reviews/EXP-ID/`：通过。

---

## 结论

**Gate 2 当前不可通过。**

文件齐、结构齐、职责主线齐、无硬状态机、无脚本——Wave 2 的骨架是对的。卡住验收的是 §20 Single Source of Truth：同一条更新链进了五个 Skill，循环/文献字段/Review 五段/何时审/Git 绑定又在 Skill↔reference↔prompt 之间复述。再叠加 Reviewer 无「同模型降级」说明、以及 loop 对 Experiment 三 Skill 未点名，陌生 Agent 不能稳定走完「gap → Skill → subagent prompt」。

### 必须修复（阻塞 Gate 2）

1. **C1** — 删除五个 Skill 中的 `EXPERIMENTS → DISCOVERY → STORY → STATE` 代码块，只保留对 `state-files.md`（或 `story-loop.md`，二选一）的链接。
2. **M1** — `research-loop` 删除 gap 优先级与反重复正文，改为链接 `story-loop.md`。
3. **M2** — `research-loop` 路由表 Experiment 行点名 `experiment-design` → `experiment-execution` → `result-analysis`。
4. **M3** — 在 `reviewer.md`（唯一完整定义）写明：无不同模型时 → fresh context + 文首记录独立性限制；禁止同会话自审。Skill 只链接。
5. **M4** — `experiment-review` Skill 删除五段输出列表，改为链接 `reviewer.md` / 两个 task prompt。
6. **M5** — `prompts/experiment-review.md` 删除与 Skill 重复的 When-to-review / Orchestration；只留决策矩阵与 `REVIEWS.md` 摘要模板。
7. **M6** — `literature-research` 删除 LITERATURE 八字段枚举，改为链接 template / `state-files.md`。
8. **M7** — `framework-maintenance` 删除 §19/§20 规则正文与尺寸数字，检查表改为对照 reference 的检查语言。
9. **M8** — `experiment-execution` 删除 Git 恢复五步与绑定字段逗号列表，改为链接 `git-linking.md`。

### 建议一并改（不单独阻塞，但与 SSOT/走通相关）

- N1 语法；N3 去掉 execution 对 STORY 的 trivial fix；N8 澄清「executor = Main Agent」；N9 点名 `research-lead`。

### 修复后的走通预期（检查清单第 10 条）

陌生 Agent 只读 `AGENTS.md`：

1. 新 session → `workspace-resume` → 五问 → 同轮继续。
2. 路由未定 → `research-loop` → 读 STORY gap。
3. Literature → `literature-research`；量大则 `literature-scout` + `subagent-handoff`（只写 `.research/work/`，不改 Story）。
4. Experiment → `experiment-design` → `experiment-execution`（或 `experiment-agent`）→ `result-analysis`（或 `result-analyst`）。
5. Review → `experiment-review` Skill → 按矩阵选用 `method-review.md` / `result-review.md` → `reviewer` 写入 `.research/reviews/EXP-ID/` → Main Agent 写 `REVIEWS.md`。

完成上述必须修复后，建议 R2 只复审 C1 与 M1–M8 是否变为「链接而非正文」；不必重开全量 Gate。
