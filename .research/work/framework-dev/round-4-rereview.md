# Gate 1–3 Round 4 — Reviewer R4 复审

- **角色：** 独立 Reviewer R4（fresh context；非开发者、非修复者）
- **审核时点：** 2026-09-02
- **对象：** workspace `/Users/herxanadu/Documents/story-research-workspace` HEAD `2abd6d0`；toy 代码 `/Users/herxanadu/Documents/story-research-code` HEAD `b0621e2`
- **规格：** `/Users/herxanadu/.cursor/plans/story_workspace_多agent开发_7d4d7dfb.plan.md` §1 / §7 / §9 / §11–13 / §19–22 / §24
- **对照声称：** `.research/work/framework-dev/round-2-fix.md`、`round-1-3-fix.md`（声称，非证据）
- **只读：** 除本文件外未改任何路径；未执行 git 写操作
- **明确不审：** `.research/work/framework-dev/harness-smoke/` 与 `adapters/README.md` 支持矩阵三列的 smoke 结论（Lead 另行核对）

判定用语：已销项 / 部分 / 未销项。证据均为当前磁盘路径与行号。

---

## 1. T1 冷启动四问

身份：无聊天历史的新 Agent。只按 `AGENTS.md` → `.research/PROJECT.md` → `.research/STORY.md` → `.research/STATE.md` 顺序读取。未打开 DISCOVERY / EXPERIMENTS / LITERATURE / REVIEWS / RESOURCES / references / templates / Skills。

### 1. 这个项目是什么？

长期目标是证明：基于流级统计特征的轻量检测器，在标准入侵检测基准上可接近深度包检测的检出能力，同时显著降低计算与部署成本。核心科学问题是：异常检测是否必须依赖包级或高维流特征上的复杂模型，还是少量可解释的流统计量已足够。主数据 CICIDS2017 子集（可选 UNSW-NB15）。当前 `.research/` 为 **MOCK** 实例，可替换。

来源：`AGENTS.md` L5–7；`.research/PROJECT.md` L5–20。

### 2. 当前 Story 是什么？

当前相信：多数常见攻击在流持续时间、包数比率、字节不对称性三个统计量上与正常流量可分；仅含这三特征的轻量分类器（逻辑回归或浅层树）加 per-flow 归一化即可捕获主要信号，复杂特征主要带来边际收益。边界：只做流级聚合；MOCK 合成数据已跑通；真实 CICIDS 尚未验证；当前 IF toy 基线可能不够强。Open Gaps：真实子集差距、归一化策略、哪些攻击族依赖额外特征。

来源：`.research/STORY.md` 六段（L5–33）。正文无 F1 / gap 百分点 / n / commit。

### 3. 现在做到哪里？

EXP-001 已 `completed`（MOCK）；无活跃实验。Story Status = `IN_PROGRESS`。Current Focus 是 Open Gap #1（真实 CICIDS 子集尚未验证）。method/result review 已指向 `REVIEWS.md`。未宣称 Story 完成。

来源：`.research/STATE.md` L5–43。

### 4. 下一步应该看什么/做什么？

按 `STATE.md` Recommended Next Action（L18–24），本轮按 Open Gaps 顺序，**优先 Gap #1**：

1. 将合成流替换为真实 CICIDS 子集，复跑 EXP-001 协议
2. 设计 EXP-002：攻击族 ablation（Open Gap #3）
3. 视复验结果再评估归一化（Open Gap #2）

继续执行前，按 `AGENTS.md` L17 按需打开 EXPERIMENTS / DISCOVERY；需要跑代码时再读 RESOURCES。

### 读取文件清单

| # | 文件 | 是否必要 |
|---|------|----------|
| 1 | `AGENTS.md` | 是（入口） |
| 2 | `.research/PROJECT.md` | 是（Q1） |
| 3 | `.research/STORY.md` | 是（Q2） |
| 4 | `.research/STATE.md` | 是（Q3、Q4） |

共 **4** 个文件。四问均可回答，没有卡点。

### T1 判定：**PASS**

---

## 2. 销项表

### 2.1 R1（C1、C2、M1–M6 + N1–N7、N9）

| 编号 | 判定 | 证据 | 若未销项，最小修法 |
|------|------|------|-------------------|
| **C1** | 已销项 | `.research/STORY.md` Evidence L17–20 仅机制句 + `EXP-001` 指针；全文 `rg` 无 `F1` / `1.0000` / `0.5455` / `b0621e2`。横幅 L3 仍写「不含具体性能数字」，与正文一致。 | — |
| **C2** | 已销项 | 数字仅在 `.research/EXPERIMENTS.md` L45–49（合法 SoT）与两份 review 正文。`.research/DISCOVERY.md` L11 为机制句 + `Evidence: EXP-001`，无 `1.0000`。`.research/STATE.md` 无 F1、无 hash。`.research/REVIEWS.md` L15–16 无数字。 | — |
| **M1** | 已销项 | `.research/STATE.md` Recently Completed L15：`EXP-001 completed (MOCK) — 见 EXPERIMENTS.md`。Active L11：`none — EXP-001 completed (MOCK)`，与 `experiment-record.md` L52–57 冻结词 `completed` 对齐，无 `closed`。Story Status L43：`IN_PROGRESS`，无 `SUPPORTED_MOCK`。无完整 hash。页脚 L45：`2026-09-02`。 | — |
| **M2** | 已销项 | `.research/EXPERIMENTS.md` L5–9 `## Index`；L15 `**Status:** completed`。`.agents/templates/EXPERIMENTS.template.md` L5–9 `## Index`；L15 `**Status:**` 占位。Status 定义仍只在 `experiment-record.md` L46–57。 | — |
| **M3** | 已销项 | `.research/RESOURCES.md` 仅四节：Codebases / Datasets / Compute / External Capabilities（L5、L23、L35、L47）。Git L11：`local toy repo, branch master, no remote`（无 commit）。入口 L19 与 EXPERIMENTS L40–41 一致：`experiments/EXP-001/run.py` + `run_config.yaml`。无 Results Artifacts、无 `python3` 命令。绝对路径仅 L15 Last known local location。 | — |
| **M4** | 已销项 | `AGENTS.md` 恰好七个 `##`：Workspace Identity L5 / Start Here L9 / Autonomy L21 / Research Memory L27 / Skill Routing L33 / Subagents L50 / Maintenance L64。`wc -l` = **68**（≤150）。并行写 L31；Subagent 写权限 L62。Skill Routing 十名（L37–46）与 `.agents/skills/` 十个目录一一对应。 | — |
| **M5** | 已销项 | `README.md` L102–104「当前状态」写审核已完成并进入修复、smoke 待重跑、Test F/G/J 未执行。全文无 `V0.1 freeze` / `git tag v0.1`。 | — |
| **M6** | 已销项 | `STORY.md` Open Gaps L31–33：#1 真实子集、#2 归一化、#3 攻击族。`STATE.md` L20–24 显式「本轮按 STORY Open Gaps 顺序：优先 Gap #1」，随后 Gap #3 / Gap #2。Status 词与 EXPERIMENTS `completed`、Story `IN_PROGRESS` 对齐。 | — |
| **N1** | 已销项 | `CLAUDE.md` 30 行，未复制 AGENTS 正文。L30 指向 `adapters/claude-code.md`；该文件现已存在，不再是超前空引用。 | — |
| **N2** | 已销项 | `AGENTS.md` L19：代码仓库由 RESOURCES 定位，可为 workspace 内 / 并列 / 远程。 | — |
| **N3** | 已销项 | `.research/DISCOVERY.md` Positive/Negative 均为「机制句 — Evidence: EXP-001」（L11、L15）。Evolution L31 仅「EXP-001 后 Core Idea 获得 MOCK 内部证据」。无四字段加厚、无 Phase 11 预告。结构与 `DISCOVERY.template.md` 对齐。 | — |
| **N4** | 已销项 | `.research/REVIEWS.md` L15–16：method「方法可接受，合成数据局限已标注」；result「MOCK-tier Story Evidence，不可外推」。无 F1。 | — |
| **N5** | 已销项 | 循环图正文在 `.agents/references/story-loop.md` L9–29；`state-files.md` L7 一行「循环见 story-loop.md §核心循环」。Git 五元组：`experiment-record.md` L128–130「见 git-linking.md」，不重复字段表。 | — |
| **N6** | 已销项 | 八个 `.research/*.md` 均保留 `> **MOCK**` 横幅（各 L3）。`STATE.md` L45 日期 `2026-09-02`。`DISCOVERY.md` 无 Phase 11 / Test F/G/J 预告。`rg 2025-09` 仅命中 `round-*-review.md` 历史引用。 | — |
| **N7** | 已销项 | `.agents/references/state-files.md` L38：「完成条件中的阈值是预设目标，实测数字只写 EXPERIMENTS」。`PROJECT.md` L37 阈值保留（允许）。 | — |
| **N9** | 已销项 | EXPERIMENTS 实例+模板有 Index/Status；RESOURCES 不再多 Results Artifacts；DISCOVERY 不再四字段加厚。PROJECT / STORY / STATE / LITERATURE / REVIEWS 标题层级与对应模板一致。 | — |

### 2.2 R2（C1、M1–M8 + N1–N9）

`rg -n --hidden "EXPERIMENTS.*(→|->).*DISCOVERY.*(→|->).*STORY" .agents AGENTS.md README.md` 在 Skills / Subagents / Prompts / AGENTS / README **0 命中**。完整四段箭头仍出现在：

- `.agents/references/state-files.md` L150（`## 更新顺序`）— 规格允许的 SoT
- `.agents/references/story-loop.md` L34（`## 核心循环` 下「默认记忆更新链」）— **仍有一份**，见 §3

十个 Skill 均含六要素标题；`name` = 目录名；行数 102–120，均在 ~100–130。无审批锁 / 禁止偏离。未发现硬状态机（`research-loop` L117：`Do not use a fixed state machine`）。

| 编号 | 判定 | 证据 | 若未销项，最小修法 |
|------|------|------|-------------------|
| **C1** | 已销项 | 五个 Skill + `subagent-handoff.md` 不再贴四段箭头。`result-analysis/SKILL.md` L62–63、`story-maintenance/SKILL.md` L65–66 只链 `state-files.md` §更新顺序并声明本 Skill 负责哪一环。`subagent-handoff.md` L56–57 同样只链。 | — |
| **M1** | 已销项 | `research-loop/SKILL.md` L49–57：反重复 / Gap 优先级改为链 `story-loop.md` §反重复 / §Gap 优先级，无四条正文。停滞 L89–90 链 §停滞处理。118 行。 | — |
| **M2** | 已销项 | `research-loop/SKILL.md` L64：Experiment Delegate = `` `experiment-design` → `experiment-execution` → `result-analysis` ``。L68–69 点名 `experiment-agent` / `result-analyst` + `subagent-handoff.md`。未展开三 Skill 流程。 | — |
| **M3** | 已销项 | `.agents/subagents/reviewer.md` L19–46 `## Independence policy` 为唯一定义：模型家族 → 不同模型 → 外部 MCP；最低 `independence: same-model-fresh-context`；禁同会话自审；无 subagent 时新会话仍须记录限制。`experiment-review/SKILL.md` L48–49、L102–103 只链接、不复述。 | — |
| **M4** | 已销项 | 五段 heading 完整块只在 `reviewer.md` L96–113。`experiment-review/SKILL.md` L58–59 只写「最低五段见 reviewer.md Required output headings」。`method-review.md` L43–50、`result-review.md` L44–51 使用同一 headings，仅保留 method/result 填空。 | — |
| **M5** | 已销项 | `.agents/prompts/experiment-review.md` L3–5 一句「何时审、如何派见 Skill」；保留 decision matrix L30–37、synthesis template L39–49、「subagent 不写 REVIEWS.md」L51。无 When-to-review / Orchestration 长步骤。 | — |
| **M6** | 已销项 | `literature-research/SKILL.md` 无八字段枚举。L62–66 链 `LITERATURE.template.md` 与 `state-files.md` §LITERATURE.md；保留五透镜 Goal 表 L36–44 与「五透镜映射到 Relation 字段」一句。 | — |
| **M7** | 已销项 | `framework-maintenance/SKILL.md` 无 owner 表、无 adapter 六问正文、无尺寸数字。L34–36、L62–66 检查表用「对照 state-files.md §单一事实来源（§20）/ §尺寸建议」。Phase 11 八项检查表 L53–67 保留为检查语言。120 行。 | — |
| **M8** | 已销项 | `experiment-execution/SKILL.md` 无恢复五步枚举、无绑定字段逗号列表。L33–35、L41、L53 链 `git-linking.md` §路径恢复五步法 / §每个正式 Experiment 的最小绑定；本 Skill 负责跑前冻结 commit、跑后写 EXPERIMENTS。 | — |
| **N1** | 已销项 | `research-loop/SKILL.md` L117：`Do not use a fixed state machine`。 | — |
| **N2** | 已销项 | 同文件 L85–86：回到 gap 判断；允许跳步。 | — |
| **N3** | 已销项 | `experiment-execution/SKILL.md` L84：`STORY is never touched here`。`research-memory/SKILL.md` L96：压缩/搬移细节，不改 Problem/Core Idea 的科学主张。 | — |
| **N4** | 已销项 | `research-loop` 全文无「运行成功 ≠ 科学成功」。该句留在 `experiment-execution/SKILL.md` L31–33 与 `result-analysis/SKILL.md` L97。 | — |
| **N5** | 已销项 | 六段箭头链仅 `story-maintenance/SKILL.md` L36–38（owner）。`research-loop` L46、`workspace-resume` L65、`research-memory` L57 写「六段见 STORY.md / story-maintenance」。`AGENTS.md` L29 同样只指针。 | — |
| **N6** | 已销项 | `experiment-review/SKILL.md` L87–90 Traceability 改为一句链 `git-linking.md` §完整追溯链。 | — |
| **N7** | 已销项 | `workspace-resume/SKILL.md` L41–42：chat history vs 文件记忆见 `AGENTS.md` Research Memory 与 `state-files.md`。 | — |
| **N8** | 已销项 | `experiment-execution/SKILL.md` L91–92：`Main Agent (this skill) still owns EXPERIMENTS/STATE updates after the subagent returns`。 | — |
| **N9** | 已销项 | `research-loop/SKILL.md` L76–77、L113–114 点名 `research-lead`（读 STORY/STATE/DISCOVERY，写 `.research/work/`）。 | — |

#### 相对链接抽查（12 条）

全部指向真实文件；小节标题除一条缩写外均存在。

| # | 源 | 目标 | 结果 |
|---|-----|------|------|
| 1 | `research-loop/SKILL.md` L51 | `story-loop.md` §反重复 | 存在（`story-loop.md` L61 `## 反重复`） |
| 2 | `research-loop/SKILL.md` L56 | `story-loop.md` §Gap 优先级 | 存在（L37） |
| 3 | `research-loop/SKILL.md` L81 | `state-files.md` §更新顺序 | 存在（L145） |
| 4 | `research-loop/SKILL.md` L90 | `story-loop.md` §停滞处理 | 存在（L80） |
| 5 | `research-loop/SKILL.md` L69 | `../../prompts/subagent-handoff.md` | 文件存在 |
| 6 | `experiment-review/SKILL.md` L49 | `reviewer.md` §Independence policy | 存在（L19） |
| 7 | `experiment-review/SKILL.md` L90 | `git-linking.md` §完整追溯链 | 存在（L100） |
| 8 | `experiment-review/SKILL.md` L61 | `../../templates/REVIEWS.template.md` | 文件存在 |
| 9 | `literature-research/SKILL.md` L63–64 | `LITERATURE.template.md` + `state-files.md` §LITERATURE.md | 模板存在；`state-files.md` L93 `### LITERATURE.md` |
| 10 | `experiment-execution/SKILL.md` L41 | `git-linking.md` §路径恢复五步法 | 存在（L71） |
| 11 | `experiment-execution/SKILL.md` L50 | `git-linking.md` §推荐代码布局（§15） | 存在（L83） |
| 12 | `framework-maintenance/SKILL.md` L14 vs L35 | `state-files.md` §20 / §单一事实来源（§20） | 文件存在。L14 写 `§20` 是缩写；规范标题是 L133 `### 单一事实来源（§20）`。L35/L66 已用全称，不阻塞。 |

#### 去重是否过度

否。抽读 `research-loop`、`experiment-review`、`literature-research`、`framework-maintenance`、`experiment-execution`、`workspace-resume`、`story-maintenance`、`result-analysis`、`research-memory`：均保留 When / Goal / Default flow / Reads / Updates / Deviation，陌生 Agent 读完知道该做什么、细节去哪看。无「只剩链接的空壳」。`experiment-design` 本轮未改，仍有完整流程。

### 2.3 R3（必修项；第 6 项 git tag / harness smoke 不在本次范围）

| 编号 | 判定 | 证据 | 若未销项，最小修法 |
|------|------|------|-------------------|
| **C1**（单一 commit SoT） | 已销项 | `.research/EXPERIMENTS.md` L39 唯一绑定 `b0621e2ed266cc26020fac5b3295a588469bb495`。两份 review 元信息同 hash（`method-review.md` L6、`result-review.md` L6）。STORY / STATE / DISCOVERY / RESOURCES / REVIEWS 摘要不含其它代码 commit。`git -C story-research-code cat-file -e <commit>` OK。`ls-tree` 含 `experiments/EXP-001/run.py`、`experiments/EXP-001/run_config.yaml`、`results/EXP-001/metrics.json`。磁盘 `metrics.json`：`three_feature_lr_f1: 1.0`、`full_feature_if_f1: 0.5455`、`f1_gap_percentage_points: -45.45`、`n_train: 40`、`n_test: 10`，与 EXPERIMENTS Main Findings L47–49 一致。 | — |
| **C2**（tag / HEAD / index） | 未销项（**不在本次范围**） | workspace HEAD = `2abd6d0`；`v0.1^{commit}` 仍 = `12c132b`。工作区在本审核时点 `git status --porcelain` 为空（除随后写入的本报告）。重打 tag 按任务排除。 | Lead 在干净 HEAD 上重打 `v0.1`（用户明确要求时）。 |
| **C3**（维护审计作废旧 Verdict） | 已销项 | `.research/work/v0.1-maintenance-audit.md` L6 作废原「ready to freeze」与 STORY/DISCOVERY 假 PASS。L48–50 / L64 现 Verdict = **fix-first / 不可冻结**。L12–13 Test F/G/J **未执行**；L13 subagent 产物未落盘。§24 表 L54–62 与当前文件大体相符（见本报告 §4）。Checklist #1「更新链只在 state-files.md」略过满，见 §3。 | — |
| **C4** | 已销项 | 同 R1 C1。 | — |
| **M1** | 已销项 | 同 R1 M2。Entry L40 = `run.py`，Config L41 = `run_config.yaml`。Runs L32 日期 `2026-09-02`。Story Impact L55：「MOCK 层有证据、真实层仍开放」。 | — |
| **M2** | 已销项 | 同 R1 N3。Negative L15 恢复弱 IF 对照。 | — |
| **M3** | 已销项 | `method-review.md` 与 `result-review.md` 均为五个小写标题（各 L9 / L13 / L17 / L21 / L25）。文首 independence：`different-model-family, fresh-context`（各 L4）。`result-review.md` L17–19 alternative explanation 有实质：合成可分性 vs「三特征充分」；IF 0.5455 可能来自玩具实现/参数。日期 2026-09-02。 | — |
| **M4** | 已销项（smoke 列不评判） | `adapters/README.md` 无 F1 / commit GT / `verified`。L7 写「不回答科研问题、不提供实验数字」。L6 冷启动读序改为「见 AGENTS.md Start Here」。矩阵三列形式已改为已安装 / 有无落盘产物 / 路径；**不对 smoke 列内容下结论**。 | — |
| **M5** | 已销项 | 三处一致：(1) `adapters/claude-code.md` L12、L42：只自动读 `CLAUDE.md` + `.claude/skills/`；symlink `../../.agents/skills/<name>`。(2) `adapters/README.md` L33、L67 同表述。(3) `ls -la .claude/skills/`：十个目录 symlink，目标均为 `../../.agents/skills/<同名>`。无「no mirror」表述。 | — |
| **M6** | 已销项 | 维护审计 L12、`mock-test-scenarios.md` L3「不构成执行证据」。DISCOVERY 已删 Phase 11 预告。未代做 T3。 | — |
| **M7** | 已销项 | 同 R1 M1。 | — |
| **M8** | 已销项 | 维护审计 L13：狗食阶段 subagent 输出仅会话内、未落盘。`.research/work/` 无 `literature-scout` / `experiment-agent` / `result-analyst` 产物（仅 `.gitkeep`、`framework-dev/`、`mock-test-scenarios.md`、`v0.1-maintenance-audit.md`）。 | — |
| **m1** 日期 | 已销项 | `rg --hidden "2025-09" . --glob '!.git'` 仅命中 `round-1-review.md` / `round-3-review.md` 历史引用。EXPERIMENTS / reviews / STATE / adapters README / 维护审计均为 2026-09-02。 | — |
| **m2** | 已销项 | RESOURCES 绝对路径仅 L15 Last known local location。 | — |
| **m3** | 已销项 | `adapters/README.md` L6：冷启动读序见 AGENTS.md Start Here。 | — |
| **m4** | 已销项 | EXPERIMENTS Story Impact L55 不写「临时闭合」；STORY Open Gap #1 L31 仍为真实子集未验证。 | — |
| **m5** | 已销项 | 同 R1 M2 模板。 | — |

---

## 3. 新引入问题

未发现阻塞性新矛盾。下列为修复后仍存在、或不精确、但不构成 Gate 阻塞的事实：

1. **更新链双份（references，非 Skill）** — `story-loop.md` L31–35 仍有完整 `EXPERIMENTS → DISCOVERY → STORY（如需要）→ STATE`。Skills/AGENTS/README 已清。维护审计 Checklist #1（`v0.1-maintenance-audit.md` L22）写「更新链只在 state-files.md」因此略过满。最小修法：`story-loop.md` 该代码块改为一句「默认记忆更新链见 state-files.md §更新顺序」。不阻塞 Gate 2（原 C1 针对五个 Skill，已销）。
2. **STATE Next 与 EXPERIMENTS Next 可并存** — STATE L20–24 优先 Gap #1（真实 CICIDS 复跑），其次 Gap #3 = EXP-002 ablation。EXPERIMENTS L59 同一顺序写「真实 CICIDS 复跑；攻击族 ablation（EXP-002 候选）」。关系已说明，不是互斥。
3. **MOCK 横幅** — 八个 `.research/*.md` 均保留 `> **MOCK**`（各 L3）。未发现被删。
4. **框架脚本** — workspace 内 `find` 无 `.py/.sh/.js/.ts`。
5. **git porcelain** — 本审核开始时 `git status --porcelain` 为空。除本报告外未见并行测试员越权改动。`adapters/README.md` 矩阵三列与 `harness-smoke/` **不在核验范围**，不作结论。
6. **维护审计行数** — 审计写 `AGENTS.md` = 66 行；当前 `wc -l` = 68。仍 < 150，不影响 M4。
7. **`framework-maintenance` L14 `§20` 缩写** — 规范标题是 `### 单一事实来源（§20）`。同文件后文已用全称。不阻塞。

未发现：为去重复而把必要信息删没；Skill 变成空壳；链接指向不存在文件。

---

## 4. §24 A–G 独立判定

| 条 | 判定 | 一句证据 |
|----|------|----------|
| **A. Workspace 自包含** | 满足（Mode B 允许） | `.research/` + `.agents/` 可复制走；RESOURCES L13 相对路径 `../story-research-code`。无 code remote 是 MOCK 并列布局的已知限制，规格允许模式 B。 |
| **B. Story 可恢复** | 满足 | T1 四问 4 个文件可答；STORY 六段、约一页、无性能数字；薄弱点在 Open Gaps #1–#3。 |
| **C. History 可恢复** | 部分 | EXPERIMENTS / DISCOVERY / REVIEWS 能看出「先有三特征 Story，再跑 EXP-001，弱 IF 对照入 Negative」。扣分：Test F/G/J 未执行，没有失败实验 / 推翻 Core Idea / 大量历史压缩的真实记录（已如实标注）。 |
| **D. Experiment 可追溯** | 满足 | `flow-detector` → `../story-research-code` → `b0621e2ed266cc26020fac5b3295a588469bb495` → Entry `run.py` + Config `run_config.yaml` → `results/EXP-001/metrics.json`（数值与 Main Findings 一致）→ `.research/reviews/EXP-001/`。Status=`completed`。 |
| **E. Research Loop 可运行** | 满足（MOCK 文件层） | 磁盘上存在 Story → EXP-001 → Discovery → Story/State 一轮。subagent work 未落盘已书面承认，不把 Phase 8 写成已有产物。 |
| **F. 可迁移** | 部分（**smoke 产物由 Lead 另行核对**） | OpenCode 未安装则「至少 Codex/Claude/Cursor/OpenCode」未满。本审核不对矩阵 smoke 列下结论。 |
| **G. 易维护** | 满足（当前文件层） | 10 个 Skill 只在 `.agents/skills/`；Claude 经 `.claude/skills/<name>` symlink，未复制内容。Adapter 无科研 GT、无冷启动读序复制。canonical 规则改一处即可。 |

相对修复前审计：原 A–G 全 Yes 已被作废。本判定与重写后的维护审计大体同向；本审核把 **G 升为满足**（Skill SSOT 修复已进盘），把审计 Checklist #1 的「更新链唯一」视为过满（见 §3.1）。

---

## 5. 结论

### Gate 1：**通过**

T1 PASS。R1 CRITICAL（C1、C2）与 MAJOR（M1–M6）全部已销项。STORY 无 F1；STATE 无完整 hash；DISCOVERY 无 `1.0000`；EXPERIMENTS 有 Index + Status + 数字 SoT。AGENTS 七段、≤150 行。README 不再宣称 freeze / v0.1 tag。

阻塞项：无。

### Gate 2：**通过**

R2 CRITICAL C1 与 MAJOR M1–M8 全部已销项。十个 Skill 六要素仍在、非空壳、无硬状态机。Reviewer 独立性与五段输出以 `reviewer.md` 为唯一完整定义。Experiment 路由点名三 Skill + `research-lead`。抽查 12 条相对链接均可达。

阻塞项：无。`story-loop.md` 仍有一份更新链副本，属 references 级残留，不升格为 Gate 2 阻塞。

### Gate 3：**通过**（文件链；tag / smoke 不在本次范围）

Test A：PASS（本报告 T1）。  
Test B：PASS — STATE 给出可执行下一步（Gap #1 复跑，其后 EXP-002）。  
Test E：PASS — 当前 HEAD 文件沿 EXPERIMENTS → RESOURCES → commit `b0621e2` → Entry/Config → metrics → reviews 可重建，且 metrics 与 Main Findings 一致。  
Test H：PASS — 两份 review 可从 EXPERIMENTS Review 字段、`REVIEWS.md` Index、目录三处找到；五段标题与 independence 声明齐全；alternative explanation 有实质内容。

R3 文件层必修项（C1 单一 commit、C3 审计作废、C4/M1–M8 除 tag/smoke）已销项。

**不在本次范围、不列入 Gate 3 阻塞：** git tag `v0.1` 仍指向 `12c132b`（与 HEAD `2abd6d0` 分离）；Harness smoke 产物由 Lead 核对。宣布「V0.1 已冻结」仍须 Lead 在干净工作区重打 tag，并完成 smoke 核对——那是冻结声明，不是 Gate 3 文件链本身。

阻塞项：无。
