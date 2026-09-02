# Round 1+3 Fix Report

Fixer: Gate 1 + Gate 3（文件 / prompt 层）  
Date: 2026-09-02  
未改：`.agents/skills/`、`.agents/subagents/`、`.agents/prompts/`、`.claude/`、`story-research-code`。未执行任何 git 写操作。

---

## R1

| ID | 文件 | 做了什么 |
|----|------|----------|
| **C1** | `.research/STORY.md` | Evidence 删除 F1 / gap / n / commit；改为「EXP-001 在 MOCK 合成流上给出正向内部证据；IF 玩具基线偏弱；真实 CICIDS 子集结论仍开放」。六段保留。 |
| **C2** | `STORY.md` / `DISCOVERY.md` / `STATE.md` / `EXPERIMENTS.md` / `REVIEWS.md` | 数字只留 EXPERIMENTS（+ reviews 正文）。DISCOVERY/STORY/STATE/REVIEWS 摘要改为机制结论或指针。 |
| **M1** | `.research/STATE.md` | Recently Completed 改为 `EXP-001 completed (MOCK) — 见 EXPERIMENTS.md`；Active 与 EXPERIMENTS 对齐为 `completed`；Story Status 仅 `IN_PROGRESS`；删除自造词与完整 hash。页脚 2026-09-02。 |
| **M2** | `.research/EXPERIMENTS.md`、`.agents/templates/EXPERIMENTS.template.md` | 顶部补 `## Index`；section 补 `**Status:** completed`（模板为枚举占位）。Status 定义不抄进模板，值见 `experiment-record.md`。 |
| **M3** | `.research/RESOURCES.md` | 只保留 Codebases / Datasets / Compute / External Capabilities。删除 Results Artifacts 与 `python3` 命令。Git：`local toy repo, branch master, no remote`。入口与 EXPERIMENTS 一致（`run.py` + `run_config.yaml`）。主定位相对路径；绝对路径仅 Last known local location。 |
| **M4** | `AGENTS.md` | 改为七段：Workspace Identity / Start Here / Autonomy / Research Memory / Skill Routing / Subagents / Maintenance。Code & Git、Story 六段、底线各压成一句收入对应段。更新链改为 `state-files.md` §更新顺序。并行写规则保留。10 个 Skill 名未变。66 行。 |
| **M5** | `README.md` | 删除 freeze / git tag 表述。「当前状态」改为：Gate 1–3 审核已完成并进入修复；smoke 产物待重跑；Test F/G/J 未执行。循环与分层压缩为链接概览。 |
| **M6** | `.research/STATE.md` | Current Focus / Next 按 STORY Open Gaps 顺序，并写明「本轮按 Open Gaps 顺序：优先 Gap #1」。Status 词表对齐。 |
| **N1** | `CLAUDE.md` | 保持极薄 boot 指针；保留 `adapters/claude-code.md` 指针；未复制 AGENTS 正文。 |
| **N2** | `AGENTS.md` | Start Here：代码仓库由 RESOURCES 定位，可为 workspace 内 / 并列 / 远程。 |
| **N3** | `.research/DISCOVERY.md` | 改为模板句式 + `Evidence: EXP-001`。恢复 Negative（弱 IF 对照）。Evolution 只写「EXP-001 后 Core Idea 获得 MOCK 内部证据」。删除 Phase 11 / Test F/G/J 预告。 |
| **N4** | `.research/REVIEWS.md` | 摘要去掉数字：method 可接受且合成局限已标注；result 接受为 MOCK-tier Story Evidence，不可外推。日期 2026-09-02。 |
| **N5** | `state-files.md`、`experiment-record.md` | 认知分层循环图改为一句「循环见 story-loop.md §核心循环」。Git 绑定段改为「见 git-linking.md」+ 一句，不再重复五元组。锚点标题未改名。 |
| **N6** | `STATE.md`、`DISCOVERY.md`、`PROJECT.md`、`LITERATURE.md` | 日期统一 2026-09-02；MOCK 横幅保留；DISCOVERY 去掉开发计划文字。 |
| **N7** | `state-files.md` `### PROJECT.md` | 加「完成条件中的阈值是预设目标，实测数字只写 EXPERIMENTS」。PROJECT 完成条件阈值保留。 |
| **N9** | EXPERIMENTS 模板+实例、RESOURCES、DISCOVERY | 与 reference/模板结构对齐（Index/Status；RESOURCES 不再多 Results Artifacts；DISCOVERY 不再四字段加厚）。 |

R1 N8（无框架脚本）无需改文件。

---

## R3

| ID | 文件 | 做了什么 |
|----|------|----------|
| **C1** | EXPERIMENTS / reviews / DISCOVERY / STATE / RESOURCES | 唯一绑定代码 HEAD commit（EXPERIMENTS Git 段 + 两份 review 元信息）。旧初始 commit 短哈希已从 `.research/` 去掉。**未**移动 git tag（Lead）。 |
| **C2** | — | **未处理：** HEAD/index/tag 对齐与提交由 Lead 统一执行。 |
| **C3** | `.research/work/v0.1-maintenance-audit.md` | 作废原 Verdict 与 STORY/DISCOVERY PASS；按八项检查表重跑；§24 A–G 诚实判定；推荐 **fix-first**。 |
| **C4** | `.research/STORY.md` | 同 R1 C1。 |
| **M1** | `EXPERIMENTS.md` + 模板 | 同 R1 M2。Status=`completed`。Entry=`experiments/EXP-001/run.py`，Config 子字段 `run_config.yaml`。Runs 日期 2026-09-02。Story Impact：「MOCK 层有证据、真实层仍开放」。 |
| **M2** | `.research/DISCOVERY.md` | 同 R1 N3。 |
| **M3** | `.research/reviews/EXP-001/method-review.md`、`result-review.md` | 重写为五个小写标题。元信息：Grok 4.6 / different-model-family, fresh-context / 2026-09-02 / 指定 commit。result-review 的 alternative explanation 讨论合成线性可分性 vs「三特征充分」，以及 IF 弱来自玩具实现/参数。 |
| **M4** | `adapters/README.md` | 删除 F1/commit ground truth。矩阵改为已安装 / smoke 落盘 / 产物路径；全部「smoke 产物：无（待修复轮后重跑，存放 .research/work/framework-dev/harness-smoke/）」；OpenCode 未安装。保留 CLI。日期 2026-09-02。 |
| **M5** | `adapters/claude-code.md`、`adapters/README.md` | Limitations 写清 Claude Code 只自动读 CLAUDE.md 与 `.claude/skills/`。Skill 根表写 canonical `.agents/skills/` + Claude 目录 symlink `../../.agents/skills/<name>`。删除所有 no-mirror 表述。`.claude/` 本身未改（禁止）。 |
| **M6** | `DISCOVERY.md`、`mock-test-scenarios.md`、维护审计 | V0.1 明确 Test F/G/J **未执行**。场景文档加「不构成执行证据」。未代做 T3。 |
| **M7** | `.research/STATE.md` | 同 R1 M1。 |
| **M8** | 维护审计 | 书面承认：狗食阶段 subagent 输出仅在会话内，未落盘。 |
| **m1** | EXPERIMENTS / reviews / STATE / adapters README | 日期统一 2026-09-02。 |
| **m2** | `RESOURCES.md` | 主定位相对路径；绝对路径只留 Last known local。 |
| **m3** | `adapters/README.md` | Canonical read order 改为「见 AGENTS.md Start Here」。 |
| **m4** | `EXPERIMENTS.md` Story Impact、`STORY.md` Open Gaps #1 | Gap #1 仍为真实子集未验证；EXPERIMENTS 不写「临时闭合」。 |
| **m5** | `EXPERIMENTS.template.md` | 补 Index + Status 占位。 |
| **m6–m8** | — | 审核已通过；无改。 |
| **m9** | `adapters/{codex,cursor,opencode,deepseek-harness}.md` | 核对：无科研数字、无冷启动读序复述。仅改 README + claude-code。 |

---

## 锚点标题核对（`rg -n "^## "`）

与清单一致（未改名、未删除）：

- `state-files.md`：认知分层、文件职责、反重复规则、更新顺序、尺寸建议、Story 完成条件（另有既有「相关 reference」）
- `story-loop.md`：核心循环、Gap 优先级、反重复、停滞处理、启动与恢复（另有既有「相关 reference」）
- `git-linking.md`：每个正式 Experiment 的最小绑定、路径恢复五步法、完整追溯链（另有既有：原则、多 commit 场景、从 workspace 定位代码仓库、推荐代码布局、与 Runs 的关系）
- `experiment-record.md`：`## Index`（索引表示例围栏内）、Status 值、Section 建议字段、Git 绑定（正式实验必填）（另有既有：文件结构、索引表格式、ID 规则、Experiment vs Run、与 Review 的链接、写作原则；围栏内示例 `## EXP-031`）

---

## 自检摘要

在 `/Users/herxanadu/Documents/story-research-workspace` 执行。

| 命令 | 结果 |
|------|------|
| leak rg（STORY/STATE/DISCOVERY/REVIEWS/RESOURCES/AGENTS/README/CLAUDE/adapters） | **0 命中** |
| 非 HEAD 初始 commit 短哈希 in `.research/` | **0 命中**（`round-3-review.md` 中该历史短哈希已替换为 `superseded-initial`，以满足冻结决策） |
| 错误年号 rg（排除 `round-*-review.md`） | **0 命中** |
| `^## Index` / `**Status:**` in EXPERIMENTS + 模板 | 均有命中 |
| `^## ` AGENTS.md | 恰好 7 个，顺序正确；`wc -l` = **66** |
| reviews 五标题 | 两份各恰好 5 个规定标题 |
| `verified\|freeze\|v0.1 tag\|git tag` in README.md + adapters/ | **0 命中**（否定表述「不可冻结」只出现在维护审计 work 文档，不在该 rg 范围内） |
| `git cat-file -e` 指定 commit | ok |
| `ls-tree` 三路径 | `experiments/EXP-001/run.py`、`experiments/EXP-001/run_config.yaml`、`results/EXP-001/metrics.json` 均存在 |
| `git status --short` | 本轮允许路径均出现。同时出现并行修复者对 `.agents/skills/`、`.agents/prompts/`、`.agents/subagents/` 的修改，以及既有未跟踪的 `round-2-fix.md` / `round-3-review.md`。本会话新建 `round-1-3-fix.md`。未 commit。 |

---

## 未处理项及原因

1. **git add / commit / tag / push** — 禁止；Lead 统一提交。因此 R3 C2（tag/HEAD/index 对齐、重打 tag）未做。
2. **`.agents/skills/`、`.agents/subagents/`、`.agents/prompts/`、`.claude/`** — 禁止；并行修复者负责。维护审计对 Skill overlap 标 NOTE。
3. **`story-research-code`** — 只读。
4. **Test F/G/J 真做** — V0.1 明确未执行；本轮只标注，不代做。
5. **Harness smoke 重跑** — 无落盘产物；矩阵已诚实改写，未伪造 verified。
6. **`CLAUDE.md` 映射表** — 按 E15 保持极薄，未再扩写 symlink 细节（细节在 `adapters/claude-code.md`）。
7. **`round-*-review.md` 中的错误年号** — 自检 glob 排除；未改审核原文日期引用。仅去掉 `.research/` 内被禁的旧 commit 短哈希。
