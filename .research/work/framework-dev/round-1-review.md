# Gate 1 Round 1 — Reviewer R1 / Tester T1

审核对象：Story-Driven Research Workspace V0.1 认知层（Phase 0–3）
审核日期：2026-09-02
范围：`AGENTS.md`、`CLAUDE.md`、`README.md`、`.agents/references/` 四文件、`.agents/templates/` 八模板、`.research/` 八个 MOCK 实例
规格：计划 §1 / §6 / §7.1–7.7 / §8 / §9 / §10 / §20 / §21 Phase 3
未审：skills、adapters、subagents、prompts、toy 代码仓库（并行开发，忽略）

---

## T1 冷启动结果

身份：无聊天历史的新 Agent。只按 `AGENTS.md` → `PROJECT.md` → `STORY.md` → `STATE.md` 顺序读取。未打开 DISCOVERY / EXPERIMENTS / LITERATURE / REVIEWS / RESOURCES / references / templates。

### 1. 这个项目是什么？

长期目标是证明：基于流级统计特征的轻量检测器，在标准入侵检测基准上可接近深度包检测/全特征强基线的检出能力，同时显著降低计算与部署成本。核心科学问题是：异常检测是否必须依赖包级或高维流特征上的复杂模型，还是少量可解释的流统计量已足够。评估主数据为 CICIDS2017 子集（可选 UNSW-NB15）。当前 `.research/` 明确标注为 **MOCK** 测试实例，不是真实科研项目。

来源：`PROJECT.md`（Research Goal / Primary Scientific Problem / Key Datasets）+ `AGENTS.md` 工作区身份句。

### 2. 当前 Story 是什么？

当前相信：多数常见攻击在流持续时间、包数比率、字节不对称性三个统计量上与正常流量可分；用仅含这三特征的轻量分类器（逻辑回归或浅层树）加 per-flow 归一化即可捕获主要信号，复杂特征主要带来边际收益。边界是只做流级聚合、尚未上真实 CICIDS、当前 Isolation Forest toy 基线可能不够强。最大缺口是真实子集上的差距、归一化策略、以及哪些攻击族依赖额外特征。

来源：`STORY.md` 六段（Problem → Open Gaps）。

### 3. 现在做到哪里？

EXP-001 已完成（STATE 写为 closed / MOCK tier），无活跃实验。Story Status 为 `IN_PROGRESS`。MOCK 合成数据上已有正向内部证据，method/result review 已做。尚未在真实 CICIDS 子集上复验，未宣称 Story 完成。

来源：`STATE.md`（Current Focus / Active Experiment / Recently Completed / Story Status）。

### 4. 下一步应该看什么/做什么？

按 `STATE.md` Recommended Next Action：

1. 设计 EXP-002（攻击族 ablation：哪些攻击依赖额外特征）
2. 可选：把 `mock_flows.csv` 换成真实 CICIDS 子集，复跑 EXP-001 协议
3. 若复验仍支持 Core Idea，再考虑推进 Story Status

继续执行前，按 `AGENTS.md` 按需打开 `.research/EXPERIMENTS.md`（EXP-001 细节）和 `.research/DISCOVERY.md`（学到了什么）；需要跑代码时再读 `RESOURCES.md`。

### 读取文件清单

| # | 文件 | 是否必要 |
|---|------|----------|
| 1 | `AGENTS.md` | 是（入口） |
| 2 | `.research/PROJECT.md` | 是（Q1） |
| 3 | `.research/STORY.md` | 是（Q2） |
| 4 | `.research/STATE.md` | 是（Q3、Q4） |

共 **4** 个文件。四问均可回答，没有卡点。未读其余状态文件、references、templates。

说明：`STATE.md` Key Files 表列出了 EXPERIMENTS / DISCOVERY / RESOURCES / reviews，机械跟随该表会多读 2–4 个文件，但仍远少于「十几个」。四问本身不需要那些文件。

### T1 判定：**PASS**

入口设计达到 Phase 3 预期：陌生 Agent 只读 `AGENTS.md + PROJECT + STORY + STATE` 即可定位项目、Story、进度与下一步。未出现必须读十几个文件才能回答的失败模式。

---

## R1 审计

行数实测（`wc -l`，审核时点）：

| 文件 | 行数 | §20 建议 | 本项 |
|------|------|----------|------|
| `AGENTS.md` | 87 | < 150 | 达标 |
| `STATE.md` | 45 | 几十行 | 行数达标，内容密度越界（见下） |
| `STORY.md` | 37 | 约一页 | 篇幅达标，但 Evidence 混入数字 |
| `CLAUDE.md` | 30 | 极薄 | 达标 |
| `README.md` | 149 | 简洁架构说明 | 偏长 |

`.py` / `.sh` / `.js` / `.ts`：workspace 内 **0** 个。

---

### CRITICAL

#### C1. STORY Evidence 写入具体性能数字与 commit，违反 §7.2 / §1

- **文件：** `.research/STORY.md` Evidence 段（约 L17–20）
- **事实：** 标题横幅写「不含具体性能数字」，正文却写 `commit b0621e2`、`50 行流`、`F1=1.0000`、`IF F1=0.5455`、`gap −45.45 pp`。
- **规格：** §7.2「不记录具体性能数字」；`state-files.md`「STORY Evidence 段写机制性结论，不抄数字；数字留在 EXPERIMENTS」。
- **建议：** Evidence 只保留机制句 + `EXP-001` 指针，例如：「EXP-001 在 MOCK 合成流上给出正向内部证据；真实子集结论仍开放」。数字、n、commit 全部留在 `EXPERIMENTS.md`。

#### C2. EXPERIMENTS / DISCOVERY / STORY / STATE 互相复制同一组数字，认知分层失效

- **文件与位置：**
  - `.research/EXPERIMENTS.md` Main Findings（L36–40）：`F1=1.0000` / `0.5455` / `−45.45 pp` / `n_train=40, n_test=10`（此处是合法 SoT）
  - `.research/DISCOVERY.md` Positive Discoveries #1（L11–16）：逐条复制同一组 F1
  - `.research/STORY.md` Evidence（L20）：再抄一遍
  - `.research/STATE.md` Recently Completed（L15）：再抄 `F1=1.0 vs 0.5455`，并展开完整 git hash（L16）
  - `.research/REVIEWS.md` result-review 摘要（L16）：同样数字（摘要级，见 M 项）
- **规格：** §1「EXPERIMENTS = What happened / DISCOVERY = What did we learn / STORY = What do we currently believe；不要让三个文件互相复制」；`state-files.md` 反重复表明确禁止 EXPERIMENTS→DISCOVERY 逐条复制 Main Findings、禁止 EXPERIMENTS→STORY 抄数字、禁止任意文件把 STATE 写成流水账。
- **建议：**
  - EXPERIMENTS：保留完整数字、runs、git（唯一事实源）
  - DISCOVERY：改成「三特征在 MOCK 合成分布上可分，且强于本实验的 full-feature IF toy 基线。Evidence: EXP-001。置信度为 MOCK-tier，外推需谨慎。」不要贴 F1
  - STORY：见 C1
  - STATE：见 M1，只留 `EXP-001 completed (MOCK)` 指针

此条不修则 Gate 1「每个文件只承担自己的职责、避免重复」不能冻结。

---

### MAJOR

#### M1. STATE 展开实验细节，超出 §7.3「只回答六个问题」

- **文件：** `.research/STATE.md` Recently Completed（L13–18）、Recommended Next Action 第 3 条（L24）
- **事实：** 写入 F1 对比、完整 commit hash、review 完成声明、四文件「已同步」。Story Status 还发明 `SUPPORTED_MOCK`（L24），不在模板枚举 `IN_PROGRESS | READY_FOR_WRITING`、也不在 §8 的 `READY_FOR_WRITING` 中。Active Experiment 写 `closed`，而 `experiment-record.md` 冻结词是 `completed` / `failed` / `abandoned` / `superseded`，无 `closed`。
- **建议：** Recently Completed 改为子弹指针（`EXP-001 completed (MOCK)`）。Next Action 不要发明新 Story Status 词。Active Experiment 与 EXPERIMENTS Status 对齐为 `completed`。完整 hash 从 STATE 删除。

#### M2. EXPERIMENTS 实例与模板都缺少 reference 规定的 Index 表与 Status 字段

- **文件：**
  - `.agents/references/experiment-record.md` L6–48、L79–88（要求顶部 Index + section 内 `Status:`，并冻结 6 个 Status 值）
  - `.agents/templates/EXPERIMENTS.template.md` 全文（无 `## Index`，无 `Status`）
  - `.research/EXPERIMENTS.md` 全文（同样缺失）
- **事实：** 模板只声明「字段规范见 experiment-record.md」，但示例 section 未体现 Index/Status，MOCK 实例跟着模板走，规范与落地两套。
- **是否「同一规则写两版」：** 字段/Status/索引表的完整定义只在 `experiment-record.md`（符合 SoT）。问题是模板与实例**没有实现**该定义，而不是双份定义。`state-files.md` L87 只做指针，这一处 SoT 是对的。
- **建议：** 在 `EXPERIMENTS.template.md` 顶部加入 Index 表示例，每个 section 加 `**Status:** {{planned|running|completed|...}}`。MOCK `EXPERIMENTS.md` 同步补 Index 一行 + `Status: completed`。不要把 Status 表再抄进模板或 AGENTS.md。

#### M3. RESOURCES 超出「资源身份 + 定位提示」，并与 EXPERIMENTS 抢 Git/结果 SoT

- **文件：** `.research/RESOURCES.md`
  - L11：`Git:` 写成实验 commit `b0621e2…`（§6 / `git-linking.md`：commit 绑定在 EXPERIMENTS，RESOURCES 的 Git 应是 remote/身份）
  - L21–25：`Entry points` 含 `python3 …/run.py` 执行命令
  - L67–70：`## Results Artifacts` 再用绝对路径重复 EXP-001 metrics（与 EXPERIMENTS Results 重复）
- **对比模板：** `.agents/templates/RESOURCES.template.md` 无 Entry points、无 Results Artifacts。
- **另：** EXPERIMENTS Entry 是 `experiments/EXP-001/run_config.yaml`（`.research/EXPERIMENTS.md` L32），RESOURCES 指向 `run.py`（L24）。陌生 Agent 无法判断入口是配置还是脚本。
- **凭据：** 未发现 password / token / key。此项不是泄密问题。
- **建议：** RESOURCES 只保留 Codebases / Datasets / Compute / External Capabilities。Git 改 remote 或 `local toy repo`（无 commit）。删 Results Artifacts。Entry 与 EXPERIMENTS 对齐为同一相对路径（建议 `experiments/EXP-001/run.py` + 配置作 Notes）。绝对路径只留在 Last known local location。

#### M4. AGENTS.md 未按 §9 七段组织，并把协议碎片塞进入口

- **文件：** `AGENTS.md`
- **§9 要求七段：** Workspace Identity、Start Here、Autonomy、Research Memory、Skill Routing、Subagents、Maintenance。
- **实际标题：** 无 `## Workspace Identity`；多出 `## Code & Git`（L69–70）、`## Story 结构`（L72–74）、`## 底线`（L82–87）。Maintenance（L76–80）只列模板/adapter/MOCK，§9 要求的更新链写在 Research Memory（L33–37）。
- **并行写权限：** 有（L87「并行 Agent 避免同时写同一状态文件」），位置在额外的「底线」节，合格但未收进七段。
- **是否把完整流程塞进去：** 未贴整段科研循环图，87 行也未超 §20。但 Skill 全表 + 六段 Story + Git 追溯 + 四条底线，已超过「只做路由与入口」。
- **建议：** 第一段改名为 `## Workspace Identity`。删除或各压缩为 Start Here/Maintenance 下一句指针：`Code & Git` → `见 RESOURCES.md 与 git-linking.md`；`Story 结构` → 留给 `state-files.md` / `story-maintenance`；底线四条并入 Autonomy 或 Maintenance 各一行。更新链放到 Maintenance。不要在 AGENTS.md 再写六段定义。

#### M5. README「当前状态」把 Gate 1 写成 V0.1 已 freeze

- **文件：** `README.md` L147–149
- **事实：** 「V0.1 freeze: 10 canonical Skills、…、5 harness adapters… 见 git tag `v0.1`。」计划 §21 / §26：Gate 1 停在 Phase 0–3，tag `v0.1` 在 Phase 11。认知层 README 不应宣称 Skills/adapters 已冻结。
- **另：** README L26–75 几乎整节复制计划 §1 循环与认知分层，作为架构说明可接受，但与 AGENTS/state-files 三处同图，偏重。
- **建议：** 「当前状态」改为「Gate 1 认知层：入口 + 8 模板 + MOCK 实例；Skills/adapters 尚未验收」。循环图保留一处即可。

#### M6. STORY Open Gaps 与 STATE Next Action 可恢复，但入口字段互相打架

- **文件：** `STORY.md` Open Gaps #1（真实子集，L31）vs `STATE.md` Next 优先 EXP-002 ablation（L20–22）；`STATE.md` L11 `closed` vs EXPERIMENTS 无 Status。
- **规格：** §7.3 目标是陌生 Agent 几分钟内理解位置。T1 能回答，但「最大 gap」按 `story-loop.md` 应优先能改变核心判断/阻塞完成条件的缺口（真实数据复验更接近 PROJECT 完成条件），STATE 却把 Gap 3 放在第一。
- **建议：** STATE Current Focus / Next 与 STORY Open Gaps 排序对齐，或显式写「本轮选择 Gap 3 的理由（一句）」。统一 Status 词表。

---

### MINOR

#### N1. CLAUDE.md 未复制 AGENTS.md 正文（合规），但有超前引用

- **文件：** `CLAUDE.md` 全文（30 行）
- **事实：** 只做 boot 指针（读 AGENTS、`.agents/skills/`、`.research/`），符合 §10。L30 指向 `adapters/claude-code.md`（Phase 10 产物）。映射表（L13–20）是宿主适配，不算复制科研逻辑。
- **建议：** Gate 1 可删「See adapters/…」或改为「跨 Harness 测试后再补」。

#### N2. AGENTS.md「独立 Git 仓库」略窄于 §2 三种布局

- **文件：** `AGENTS.md` L70
- **事实：** 写死「实验代码在独立 Git 仓库」。§2 允许模式 A（代码在 workspace 内）。
- **建议：** 改为「代码仓库由 RESOURCES 定位，可为 workspace 内 / 并列 / 远程」。

#### N3. DISCOVERY 结构相对模板加厚，且 Evolution 复述 Story 叙事

- **文件：** `.research/DISCOVERY.md` L11–16（Finding/Evidence/Confidence/Story link 四字段）；L34–37 Research Evolution
- **对比：** `.agents/templates/DISCOVERY.template.md` 为正负发现一行 + `Evidence: EXP-xxx`
- **建议：** MOCK 收成模板句式。Evolution 只写「EXP-001 后 Core Idea 获得 MOCK 内部证据」，不要再讲一遍三特征故事。

#### N4. REVIEWS 总览摘要抄了 F1

- **文件：** `.research/REVIEWS.md` L16
- **规格：** 允许一行结论摘要；完整数字仍属 EXPERIMENTS。
- **建议：** 「MOCK 结果接受为 Story Evidence；合成数据局限已标注」即可。

#### N5. 循环图 / Git 五元组在 references 间轻度重复

- **文件：** `state-files.md` L7–24 与 `story-loop.md` L7–35 几乎同一张循环图；`experiment-record.md` L128–140 与 `git-linking.md` L12–43 重复「Codebase+repo+commit+entry+results」
- **规格：** §20 SoT。主定义应各留一处，另一处只链。
- **建议：** 循环图只在 `story-loop.md` 展开，`state-files.md` 一行链接。Git 五元组只在 `git-linking.md` 展开，`experiment-record.md` 保持现有「详见 git-linking.md」。

#### N6. MOCK 标注总体清晰，个别日期/语气不一致

- **清晰：** 八个 `.research/*.md` 均有 `> **MOCK**` 横幅；`AGENTS.md` L80、`PROJECT.md` L3、`STORY.md` L3 均声明可替换。模板本身不标 MOCK（正确）。
- **问题：** `STATE.md` L45 日期 `2025-09-02`（今天是 2026-09-02）；`DISCOVERY.md` L41 提到 Phase 11 Test F/G/J，把开发计划写进科研记忆。
- **建议：** 日期改为 2026-09-02。删掉 Phase 11 预告。

#### N7. PROJECT 完成条件含目标阈值，与「数字只在 EXPERIMENTS」边界需写清

- **文件：** `.research/PROJECT.md` L37（F1 差距 < 5%、延迟降低 > 50%）
- **判断：** §7.1 / §8 允许 PROJECT 承载完成条件；这是目标不是实验结果。不算违规。
- **建议：** 可在 PROJECT 或 state-files 加一句「完成条件中的阈值是预设目标，实测数字只写 EXPERIMENTS」。

#### N8. 无框架脚本依赖（合规）

- workspace 内无 `.py/.sh/.js/.ts`。RESOURCES 中的 `python3 …/run.py` 指向 workspace **外** 的 toy 代码，不是框架运行时。删掉该命令后更干净（见 M3）。

#### N9. 模板 ↔ 实例结构总体一致，例外已在 MAJOR 列出

一致：PROJECT / STORY / STATE / LITERATURE / REVIEWS 的标题层级与模板对齐。  
例外：EXPERIMENTS 缺 Index/Status（M2）；RESOURCES 多 Entry points 与 Results Artifacts（M3）；DISCOVERY 子字段加厚（N3）。

---

### 分项对照（审计清单 1–10）

| # | 检查项 | 结论 |
|---|--------|------|
| 1 | 8 文件职责重叠 / 互抄 | **CRITICAL**（C1、C2）；STATE vs Open Gaps 指针级重叠可接受，数字互抄不可接受 |
| 2 | 尺寸 | AGENTS 87、STATE 45、STORY 37 行均达标；STATE/STORY 是内容越界不是篇幅越界 |
| 3 | AGENTS 七段、不塞流程、并行写 | 七段不齐（M4）；未塞完整流程；并行写有 |
| 4 | CLAUDE 是否复制 AGENTS | **否**，合规（N1） |
| 5 | templates ↔ 实例；MOCK 标注 | 大体一致；EXPERIMENTS/RESOURCES 结构漂移；MOCK 横幅清晰 |
| 6 | references vs templates 的 EXP 字段/Status/Index | 规范只在 reference（SoT 对）；模板与实例未落地（M2）。未出现「两处各写一版 Status 表」 |
| 7 | STORY 六段式、无数字/实验日志 | 六段齐全；**有数字**（C1） |
| 8 | RESOURCES 身份+定位、无凭据 | 无凭据；含 commit/结果/运行命令（M3） |
| 9 | 框架脚本依赖 | **零** |
| 10 | Agent 无法可靠恢复 | 入口四问可恢复（T1 PASS）；EXP 入口 run.py vs run_config.yaml、Status 词表、Story Status 自造词会造成执行期迷路（M3、M1、M6） |

---

## 结论

### T1：PASS

冷启动四问用 4 个文件即可回答，入口符合 §20 Progressive Disclosure 与 §21 Phase 3。

### Gate 1：**不通过**（须修复 CRITICAL + 下列 MAJOR 后再冻结）

认知层目录、模板骨架、AGENTS 短入口、CLAUDE 极薄、无脚本依赖、MOCK 横幅，这些是合格的。阻塞冻结的是：**同一组实验数字写进了 STORY / DISCOVERY / STATE**，EXPERIMENTS 作为唯一总账的分层没有落地。

### 必须修复（owner 本轮）

1. **STORY.md Evidence**：删除 F1、gap、n、commit；只留机制结论 + `EXP-001`。（C1）
2. **DISCOVERY.md Positive #1**：删除与 Main Findings 重复的数字；改为学习结论 + `Evidence: EXP-001`。（C2）
3. **STATE.md**：删除 F1 与完整 hash；Status 词与 `experiment-record.md` 对齐；不要发明 `SUPPORTED_MOCK`。（M1）
4. **EXPERIMENTS.template.md + EXPERIMENTS.md**：补 Index 表与 `Status` 字段，值为 `completed`。（M2）
5. **RESOURCES.md**：去掉 Results Artifacts 与实验 commit；Entry 与 EXPERIMENTS 对齐；Git 只保留仓库身份。（M3）
6. **AGENTS.md**：补 `Workspace Identity` 标题；把 Code & Git / Story 结构 / 底线收进七段；并行写规则保留一句。（M4）
7. **README.md**：删除「V0.1 freeze / git tag v0.1」表述。（M5）

MINOR（N1–N7、N9）建议同轮顺手改，不阻塞复审理解，但 N5 的循环图双份会在 Gate 2 被 R2 再打。

### 复审关注点

修完后应能证明：打开 STORY 看不到任何 F1；打开 STATE 看不到 commit hash；打开 DISCOVERY 看不到 `1.0000`；打开 EXPERIMENTS 看得到 Index + Status + 数字 SoT。T1 四问应仍然只用四个文件就能回答。
