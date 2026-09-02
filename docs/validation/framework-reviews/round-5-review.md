# Gate A Round 5 — Reviewer R5 复审（V0.1.1 Wave A/B）

- **角色：** 独立 Reviewer R5（fresh context；same-model / fresh-context；非开发者、非修复者）
- **审核时点：** 2026-09-03
- **对象：** workspace `/Users/herxanadu/Documents/story-research-workspace` 当前磁盘。HEAD = `c8d672f`（Wave A）；Wave B 协议修订在工作树未提交（`git status --porcelain` 可见 `.agents/`、`AGENTS.md`、`README.md`、根 `.research/STATE.md`）。本审核以磁盘为准，不要求本轮 commit。
- **规格：** 计划冻结点 P0-1~P0-5 + 7/8（`v0.1.1_清洁模板修订`）；Gate A 机械检查。
- **只读：** 除本文件外未改任何路径；未 git commit。
- **明确不审：** Wave C（C1 UNINITIALIZED smoke、C2–C4 初始化/交接、C3 Claude 深链）。不因「还没做 C1」判 Gate A 失败。

判定用语：已销项 / 部分 / 未销项。证据为当前磁盘路径与行号。

---

## 1. 根 `.research/` 空状态（亲自读盘）

| 文件 | 冻结要求 | 磁盘事实 |
|------|----------|----------|
| `PROJECT.md` | `Project Status: UNINITIALIZED` | L3 `**Project Status:** UNINITIALIZED`。无 Story/实验编造。 |
| `STORY.md` | 六段均为 `_Not established yet._`；零 EXP 引用 | L5–26 六段皆占位；全文无 `EXP-`。 |
| `STATE.md` | Focus=初始化；Active=`none`；Next=workspace-resume init；`NOT_INITIALIZED` | L5 / L9 / L17 / L33 对齐。Blockers L21 指向 UNINITIALIZED。 |
| `EXPERIMENTS.md` | 标题 + 空 Index + `_No experiments yet._`；零 `## EXP-` 段 | L1–8：Index 含 Outcome 列、无数据行、无 section。 |
| `REVIEWS.md` | `## Index` + `_No reviews yet._` | L1–5。 |
| `DISCOVERY.md` | 各节 `_None yet._`；零 EXP 引用 | L3–29；无 `EXP-`。 |
| `LITERATURE.md` | 零文献 | L3 `_No literature has been incorporated yet._` |
| `RESOURCES.md` | 四空节；无本机路径 | L5–19 仅 Codebases/Datasets/Compute/External Capabilities；`rg` 无 `/Users/` 或绝对路径。 |
| `reviews/` / `work/` | 空目录可保留 gitkeep | 各仅 `.gitkeep`。 |

根八文件不是带 `{{placeholder}}` 的模板副本。与「合法空状态」冻结句一致。

`STATE.md` 工作树相对 Wave A commit 只删了 Key Files 中 `Example MOCK` 一行（原 L30），不破坏空状态。

---

## 2. 销项表

| 编号 | 判定 | 证据 | 若未销项，最小修法 |
|------|------|------|-------------------|
| **P0-1 目录三分** | 已销项 | **框架层：** `AGENTS.md`、`CLAUDE.md`、`.agents/`、`.claude/`、`adapters/`。**项目层：** 根 `.research/` 为空状态（§1）。**示例：** `examples/mock-flow-detection/`（`README.md` + 八文件 + `reviews/EXP-001/` r1/r2）。**验证：** `docs/validation/{framework-reviews,harness-smoke,handoff-tests}/` + `test-scenarios.md`。旧 `.research/work/framework-dev` 不存在。`handoff-tests/` 空目录属 Wave C，不扣 Gate A。未新增 Skill（仍 10 个）或状态文件。 | — |
| **P0-2 workspace-resume 初始化** | 已销项 | `workspace-resume/SKILL.md` L22、L44–45、L49–69：读到 `UNINITIALIZED` 则收集目标/代码数据位置/约束，**materialize 既有八文件**（点名 L55–57），不新建一套；EXPERIMENTS/REVIEWS 保持空 Index；Status → `ACTIVE`，Story Status → `IN_PROGRESS`；信息不足不得写 `ACTIVE`。科学边界 L61–66、L133–135：六段是结构不是必填事实；禁止编造 Key Observation / Core Idea / Evidence。`AGENTS.md` L9–12、L16 同一分支。`PROJECT.template.md` L3 仅 `UNINITIALIZED \| ACTIVE`。`STATE.template.md` L36–38 三值并说明转换。 | — |
| **P0-3 Recovery source / Portability / 冻结句** | 已销项 | `git-linking.md` L6–7 冻结句「能够恢复科研认知，不代表能够重新获取实验代码、数据和结果」+ 英文对照。L84 条件句：无稳定恢复源则只能恢复认知。L127–135 Portability 三值 +「进入 Story Evidence / Reviewer acceptance / 论文级结论的 EXP 必须有稳定恢复源」。`RESOURCES.template.md` L16–18 两字段；L3–5 不存秘密。`state-files.md` L107 同步 Recovery source / Portability。根 `RESOURCES.md` 无 Codebase 条目故无本机路径（符合空状态）。示例 `examples/mock-flow-detection/.research/RESOURCES.md` L13–19 为 `local-only` / `host-dependent` + `../../../story-research-code`。 | — |
| **P0-4 Outcome 必有 + 技术失败≠负发现** | 已销项 | **完整六行定义表只在** `experiment-record.md` L71–86（`not-assessed`/`supports`/`contradicts`/`null`/`inconclusive`/`invalid`）。L37、L73：必有字段，新建固定 `not-assessed`；L75 技术失败保持 `not-assessed`。模板 `EXPERIMENTS.template.md` L9 默认 `not-assessed`，L17 `{{per experiment-record.md}}`，无复述表。消费者使用值但不贴表：`experiment-design` L39/L69/L88；`experiment-execution` L62–63、L89、L98；`result-analysis` L37–38 链 §Outcome 值，L59–62 **`failed`+`not-assessed` 不产生 Negative Discovery**，`completed`+`contradicts\|null` 才进 DISCOVERY，`invalid` 不当 Negative。根 Index 已含 Outcome 列（`.research/EXPERIMENTS.md` L5）。`rg` 框架层无第二份六行定义表。 | 示例账本未跟字段：见 §3.1（不升格为本条未销项）。 |
| **P0-5 Reviewer Verdict / 元数据 / r\<N\> / Main 整合** | 已销项 | **完整枚举只在** `reviewer.md` L132–140：`PROCEED \| REVISE \| REJECT \| INSUFFICIENT_EVIDENCE \| ATTENTION_REQUIRED`。文首元数据 L27–35；Provenance `raw \| synthesis`，synthesis 用 `Source reviews:`（L42–53），禁止 `synthesis(...)`。两维独立性相对被审 Agent（L37–40、L60–72）。命名 L123–130 `*-review-r<N>.md`。Reviewer 禁止写 canonical（L101–102）。Prompts：`method-review.md` L7/L24/L34–35；`result-review.md` L7/L25/L35–36；`experiment-review.md` L7、L23–26、L45–46 `per reviewer.md`，L52–54 **Subagents 不写 REVIEWS.md**。`research-loop/SKILL.md` L86–88 **只引用** `ATTENTION_REQUIRED` + `Use reviewer.md §Verdict`，无其它 Verdict token。`experiment-review` Skill 写 REVIEWS/EXPERIMENTS 属 Main Agent 编排（L65–71、L87–89），与「Reviewer 只写 `reviews/<EXP-ID>/`」不冲突。`AGENTS.md` L69 写权限同句。 | 示例 r2 缺 `## Verdict`、handoff 占位无 `r<N>`：见 §3.2–3.3（非协议缺失）。 |
| **7→8 状态文件** | 已销项 | `rg "seven\|七个" .agents` = **0**。`research-memory/SKILL.md` L4「8 research state files」；L38 列出八文件；L95 `May edit all 8`。`state-files.md` L3「八个」；`workspace-resume` L55–57 点名八文件；`AGENTS.md` L69「八个 canonical」。`framework-maintenance` L68 第 9 项检查「七/八 state-file count」，本身不断言七个。 | — |

### 2.1 Gate A 机械项（计划 §Gate A，非 P0 编号）

| 检查 | 判定 | 证据 |
|------|------|------|
| Outcome SSOT | 通过 | 完整表仅 `experiment-record.md` L77–86；消费者无私有 Outcome token。 |
| Verdict SSOT | 通过 | 完整枚举仅 `reviewer.md` L136；`ATTENTION_REQUIRED` 属于该枚举。 |
| 根空状态 | 通过 | §1。 |
| 相对链接 | 通过 | 扫描 `AGENTS.md` / `CLAUDE.md` / `README.md` / `.agents/` / `adapters/` / `examples/`：124 条相对链接均可解析，0 missing。（Claude 经 symlink 打开 Skill 的深链属 C3，不审。） |
| 四 reference 锚点未改名 | 通过 | 既有 `##` 保留；新增 `experiment-record.md` L71 `## Outcome 值`、`git-linking.md` L127 `## Portability`（计划允许追加）。 |
| Skill 六要素 + frontmatter | 通过 | 十个 `.agents/skills/*/SKILL.md` 均有 `name`/`description` 与 When / Goal / Default flow / Reads / Updates / Deviation。行数 102–135。 |
| 无框架脚本 | 通过 | `find` 无 `.py/.sh/.js/.ts`。 |
| 未新增 Skill/状态文件 | 通过 | Skill 仍 10 个；`.claude/skills/` 十个 symlink → `../../.agents/skills/<同名>`。 |
| AGENTS 七段、<150 行 | 通过 | 七个 `##`：Workspace Identity L7 / Start Here L14 / Autonomy L28 / Research Memory L34 / Skill Routing L40 / Subagents L57 / Maintenance L71。`wc -l` = **76**。 |
| `framework-maintenance` 不在日常 loop | 通过 | `AGENTS.md` L53「仅维护框架、升级 Harness 或发布版本时使用」。`research-loop/SKILL.md` 无该 Skill。该 Skill L23–24 禁止用于科研。 |

---

## 3. 新引入问题

未发现阻塞 Gate A 的协议缺口。下列为 Wave B 后示例/文档漂移，**不构成 Gate A 阻塞**：

1. **示例 EXPERIMENTS 未跟 Outcome（非阻塞）** — `examples/mock-flow-detection/.research/EXPERIMENTS.md` Index L7 仍为 `EXP-ID \| Title \| Status \| Story Gap \| Updated`（无 Outcome 列）；section 有 Status L15、无 `**Outcome:**`。协议已要求必有字段。最小修法：Index 加 Outcome 列，section 补科学判断（MOCK 闭环语义上接近 `supports` 或按 reviewer 口径标注，并由 Index 同步）。不因示例滞后判 P0-4 未销项。
2. **示例 r2 缺 `## Verdict`（非阻塞）** — `reviewer.md` L134 要求正文第一 heading 为 `## Verdict`。`method-review-r2.md` / `result-review-r2.md` 从 `## strongest evidence` 起笔，全文无 `## Verdict`；`REVIEWS.md` L13–14 却写 `r2 — PROCEED`。r1 为恢复的 raw（旧标题 + L32/L36 `## Verdict`），按 Wave A「不润色」保留，合理。最小修法：r2 文首加 `## Verdict`（synthesis 允许整理结构；或注明 example 豁免并改 REVIEWS 摘要）。
3. **`subagent-handoff.md` L16 仍写无轮次文件名（非阻塞）** — `Required output: .research/reviews/<EXP-ID>/<review-type>.md`，与 `reviewer.md` L126–130 `*-review-r<N>.md` 不一致。Role 表 L33 只写目录，正确。最小修法：占位改为 `<method\|result>-review-r<N>.md`。
4. **`REVIEWS.md` 摘要双格式（非阻塞）** — `REVIEWS.template.md` L14–19 为 `**Latest method review:** r2 — <per reviewer.md>` + Files 列表（符合冻结句）。`prompts/experiment-review.md` L42–50 另给 `Method verdict:` / `Result verdict:` 块。Main Agent 可能写出两种摘要。最小修法：prompt 合成模板改与 REVIEWS.template 同一形状。
5. **LITERATURE 字段列表未同步 Identifier/Access（非阻塞）** — `LITERATURE.template.md` L11–13 新增 `Identifier` / `Access`；`state-files.md` L95 仍列旧八字段。Skill 同时链两者（`literature-research/SKILL.md` L63–64）。最小修法：state-files §LITERATURE.md 补两字段名（不要把模板正文再抄一遍）。
6. **`adapters/README.md` 提前写 Wave C（非阻塞 Gate A；Gate B 前必须改）** — L5「UNINITIALIZED re-test in V0.1.1 Wave C」、矩阵 Last tested `2026-09-02` 指向仍描述 MOCK 根项目的 `docs/validation/harness-smoke/*.md`。`README.md` L62–65 已诚实写 pending Wave C1–C4。二者矛盾。本轮**不**因此判 Gate A 失败（C 未跑）。最小修法：Last tested 改回 V0.1 MOCK 证据，或等 C1 完成再改句子。
7. **`docs/validation/test-scenarios.md` Test F（非阻塞）** — L9–12「When an experiment fails」仍写自动加 Negative Discovery。该文件声明不构成执行证据（L3），且 Test F/G/J 属 V0.2。与 P0-4 协议不一致，留作 V0.2 修订即可。
8. **示例 `PROJECT.md` 无 Project Status 字段（非阻塞）** — 根模板已两值；MOCK 示例仍无 `UNINITIALIZED|ACTIVE`。仅在有人把示例拷回根目录时才会模糊 init 分支。

### 3.1 去重是否过度

否。抽读 `workspace-resume`（135 行，含完整 init 分支）、`research-loop`、`experiment-review`、`result-analysis`、`experiment-execution`、`experiment-design`、`research-memory`、`framework-maintenance`、`story-maintenance`、`literature-research`：均保留六要素与可执行步骤；细节用链接，不是空壳。未发现为去重而删掉「该做什么」的必要信息。

### 3.2 新矛盾（STATE IN_PROGRESS vs UNINITIALIZED）

**当前根磁盘无此矛盾。** `PROJECT.md` L3 = `UNINITIALIZED` 与 `STATE.md` L33 = `NOT_INITIALIZED` 成对。`IN_PROGRESS` 只出现在初始化**之后**的指令里：`AGENTS.md` L11、`workspace-resume` L60、`STATE.template.md` L38（「Project Status 变为 ACTIVE 后用 IN_PROGRESS」）。这是时序转换，不是同一快照双值。

未发现其它协议级新矛盾（Outcome/Verdict 私有词表、`synthesis(...)`、Reviewer 越权写 canonical、research-loop 复制完整 Verdict 表）。

---

## 4. 结论

### Gate A：**通过**

P0-1~P0-5 与 7/8 全部 **已销项**。根 `.research/` 为合法空状态；Outcome/Verdict 各一份完整枚举；初始化不编造 Story；git-linking 冻结句与 Portability 已落地；技术失败不自动进入 Negative Discovery；Reviewer 只写 `reviews/<EXP-ID>/`，canonical 由 Main Agent 整合；AGENTS 七段、76 行；`framework-maintenance` 不在日常 loop；无新 Skill/状态文件/脚本。

Wave C 未跑，不列入本 Gate。示例账本/r2 标题与 adapters Last tested 的文档漂移见 §3，属修复建议，不是阻塞。

### 阻塞项

无。

### 未销项行

无。
