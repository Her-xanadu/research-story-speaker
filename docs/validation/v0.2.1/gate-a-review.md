# V0.2.1 Gate A Review — micro-hardening (Waves 0, A–F)

- **角色：** 独立 V0.2.1 Gate A Reviewer。相对 Wave A–F 实施者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无实施聊天历史；只读磁盘 + `/usr/bin/git`）。
- **审核时点：** 2026-09-04 14:05–14:20（UTC+8）。
- **对象：** repo `/Users/herxanadu/research-story-speaker`，分支 `v0.2.1-micro-hardening`，HEAD = `6b44c72f9e455fc43e862973654c46c5f956ff16`（与任务书一致；`origin/v0.2.1-micro-hardening` 同值）。Diff 基线 `v0.2^{}` = `577bb76f4cc7a43ae4c64a1c35456d4d53e746c1`；`git merge-base v0.2 v0.2.1-micro-hardening` = `577bb76`（线性叠加，无重写）。
- **工作树布局：** 主 checkout `/Users/herxanadu/research-story-speaker` 在 `v0.2-research-intelligence`（= `577bb76`），干净。被审分支 checkout 在 worktree `/private/tmp/rss-v021-wave0`（`6b44c72`，`git status --porcelain --untracked-files=all` 为空）。审核读取全部以 `git show 6b44c72:<path>` / 该 worktree 为准。
- **只写：** 本文件（按任务书写到 `docs/validation/v0.2.1/gate-a-review.md`，落在主 checkout；`docs/validation/v0.2.1/` 目录只存在于 v0.2.1 分支，故本文件在主 checkout 为未跟踪文件，需由实施侧移入 `v0.2.1-micro-hardening` 后提交）。未 `git add` / commit / tag / push；未改 Skills、prompts、Layer 2、Protocol、templates、`.research/`、`AGENTS.md`。
- **范围：** 静态审读 `v0.2..6b44c72` 全部 45 文件 diff（+1728 / −257）+ 相关 owner 文件；不跑 harness。
- **Gate 判定词汇：** `APPROVE | TARGETED_REVISION | MAJOR_REVISION` 为本任务书规定的框架层 gate 结论，**不是** `reviewer.md` §Verdict、不是 Idea-gate 四动作、不是 `evidence-and-claim.md` §F 报告标签，不新增任何 Protocol 枚举。

---

## 0. 提交链（磁盘证据）

```text
6ab7ed5  Release sync: record v0.2.1 baseline after master FF      (Wave 0)
80d8180  Wave A: compact exploratory execution path                 → merge 2fa4b85
1934c3a  Wave B: semantic cleanup                                   → merge 04d871c
5c4f56a  Wave C: skill-evolution and fixture hygiene                → merge 5df99a0
6ef093b  Wave D: bounded deep-literature search                     → merge 82758e9
8592f69  Wave F: source and path hygiene                            → merge 607faac
6b44c72  Wave E: meta-rule deduplication                            (直接落在集成分支)
```

---

## 1. 冻结检查

| 项 | 结果 | 证据 |
|---|---|---|
| `v0.1^{}` | **✓** | `8db3b301f5bce878d6c2ee4a61bcb234c9609c3d` |
| `v0.1.1^{}` | **✓** | `762deb4c9db896acb5c00066b8e6dc5a63732cfa` |
| `v0.2^{}` | **✓** | `577bb76f4cc7a43ae4c64a1c35456d4d53e746c1` |
| `origin/master` == `v0.2^{}` | **✓** | `577bb76`（Wave 0 FF；未要求 == v0.2.1） |
| canonical state = 8 | **✓** | `git ls-tree 6b44c72 .research/` → 8 个 `*.md` + `reviews/` + `work/` |
| research-loop = 1 | **✓** | 唯一 `.agents/skills/research-loop/SKILL.md` |
| subagents = 5 | **✓** | experiment-agent / literature-scout / research-lead / result-analyst / reviewer |
| skills = 12 | **✓** | `.agents/skills/*/SKILL.md` = 12；`.claude/skills/` 12 个 symlink `→ ../../.agents/skills/<name>` 全部可解析 |
| RI references = 6 | **✓** | `deep-literature-mode / evidence-and-claim / experiment-thinking / idea-and-mechanism / scientific-reasoning / skill-evolution`（`.gitkeep` 已删，目录非空） |
| framework scripts = 0 | **✓** | 框架层无非 `.md`/`.gitkeep`/symlink 对象；全仓无 `100755`；无 `.sh/.py/.js/.yaml/.json` |
| 根 `.research/` UNINITIALIZED，零实验 | **✓** | `git diff --stat v0.2 6b44c72 -- .research/` 为空；`PROJECT.md` `Project Status: UNINITIALIZED`；`EXPERIMENTS.md` 无 `EXP-\d{3}` |
| Protocol owner byte-identical | **✓** | `state-files.md`、`experiment-record.md`、`story-loop.md`、`git-linking.md` 对 v0.2 diff 为空；`reviewer.md` +1 行（仅 progressive-load cite，§Verdict 段未动） |
| 模板 | **✓** | 仅 `REVIEWS.template.md` +1 行（头部说明“索引 + 当前摘要，非 Reviewer artifact”），字段形状未变；其余 7 模板未动 |
| `adapters/`、`CLAUDE.md`、`.gitignore` | **✓** | 不在 diff 内 |
| 历史证据未重写 | **✓** | `docs/validation/research-intelligence/{cases,prompt-regression,gate-reviews,source-audit}/` 不在 diff 内 |
| 相对链接 | **✓** | 框架层 + live-cases + sources 共 68 文件 382 个相对链接，0 断链 |

---

## 2. SSOT 检查（`rg` 于 `.agents AGENTS.md CLAUDE.md README.md adapters`）

| 规则 | 结果 | 证据 |
|---|---|---|
| Outcome 完整表只在 `experiment-record.md` | **✓** | 单行枚举 `not-assessed…supports…contradicts` 与表格行 `` | `supports` | `` 在 owner 之外 0 命中 |
| Reviewer Verdict 完整表只在 `reviewer.md` | **✓** | `PROCEED` / `INSUFFICIENT_EVIDENCE` 仅 `reviewer.md:157`。Wave B 在 `experiment-review.md`（prompt L82 / Skill L82–83）以 `REVISE / REJECT / ATTENTION_REQUIRED` 作同步触发条件并写明 “per reviewer.md §Verdict; do not recopy the list”——是引用子集不是复表（措辞见 N1） |
| Story 六段不被 RI 分叉 | **✓** | 六段只在 `state-files.md` §STORY + `STORY.template.md`；RI 六件无六段枚举；`story-maintenance` L40 / `result-diagnosis.md` L165 为 v0.2 既有 cite |
| Idea-gate 动作只在 `idea-and-mechanism.md` | **✓** | 四行 glossary 只在 §H（L224–229）；`idea-evaluation` Skill L21 / prompt L6 / `story-audit.md` L160 三处提及均为 v0.2 既有 cite，Wave B 未新增枚举 |
| §F 报告标签只在 `evidence-and-claim.md` | **✓** | 六标签块只在 §F；`does not address` 其余命中为 README 历史证据句、`skill-evolution.md` 禁用短语表、`story-audit.md` 普通英文 |
| `compact` / `full` 未进 Protocol / STATE / EXPERIMENTS / Status / Outcome | **✓** | 四 Protocol owner + `reviewer.md` + 8 模板 + 根 `.research/` 内 `compact` 0 命中；`full` 仅 v0.2 既有无关用法（`reviewer.md` “full definition / full section”、`LITERATURE.template.md` `full-text-checked`）。两 Skill 均显式声明 “Skill-internal modes. Never write them into STATE, EXPERIMENTS, Status, or Outcome” |
| Wave B 引用的字段均已存在 | **✓** | STATE `## Recommended Next Action`（模板 L17）；EXPERIMENTS `Next`（`experiment-record.md` L124）；REVIEWS `Latest method review / Latest result review`（模板）；`Provenance: raw \| synthesis` owner 为 `reviewer.md` L34 |

---

## 3. Wave 逐项

### Wave A — compact 探索路径（`experiment-design`、`result-analysis`、`research-loop`）

**✓ 存在且闭合。** 两 Skill 各新增 `## Compact vs full (Skill-internal)`（Compact 默认 / Full 触发清单 / experiment-design 另有 “Do not auto-upgrade” 六项）与 `### Compact flow` / `### Full … flow`；`result-analysis` 抽出 `### Persist Protocol (both modes)`，DISCOVERY 写入规则（`failed`+`not-assessed` 不产生 Negative；`invalid` 非 Negative；Invalidated Findings 为 full 触发）逐条保留。compact 五项映射到既有 EXP 字段（无新列、无新文件、无 Protocol enum）。`experiment-proposal.md`、`result-diagnosis.md`、`failure-diagnosis.md` 留在磁盘未删，仅改为 full 触发加载。`research-loop` L98 一句 “Stay compact: do not default-load …” 接线，未展开流程。Full 流程步骤（experiment-design 9 步含设计字段表；result-analysis 加载 prompt 后走 Persist）与 v0.2 等价。

### Wave B — 语义清理（landing / REVIEWS / Next 同步）

**✓。** (1) Idea-gate 四动作落地规则写在 `idea-evaluation` Skill “Handoff / Main integration” 与 prompt “Handoff / state impact”，两处一致：ADVANCE → `experiment-design`；REVISE → STATE Recommended Next Action 一行；PARK/ABANDON → DISCOVERY 普通段落 + `Memo:` 指针，明确禁止 `Evidence: work/...`、禁止无 EXP 证据写 Positive/Negative Discoveries。写权限收紧为 “八个 canonical 文件全部不写”（v0.2 只列三个）。(2) `REVIEWS.md` 语义统一为 “索引 + 当前摘要，非 Reviewer artifact，不计独立 Reviewer 数”，`Provenance` 只描述 `.research/reviews/` 文件——Skill、prompt、模板三处一致。(3) Next 同步：Verdict 改变下一步时 Main 同步 EXPERIMENTS `Next` 与 STATE `Recommended Next Action`，“No new fields”。(4) `idea-and-mechanism.md` §C 轴列表改为 6 轴、costume 表与 §E/§G 改用 “claimed §C axis” 措辞；§H glossary 未动。

### Wave C — skill-evolution 与夹具卫生

**✓。** `skill-evolution.md` §C 新增 “Agent-visible input ≠ grader rubric” 与禁用短语表；§D 新增 author / executor / scorer 三角色、`Model relation: same-model` + `Context relation: fresh-context` 记录要求（字段名与 `reviewer.md` L29–30 一致，未复制 Reviewer 契约）；§F Reviewer 清单扩为 10 项；Anti-patterns 增 2 条。`framework-maintenance` 增 checklist #13（scorer independence），“12-row” 措辞改为 “audit checklist”。`live-cases/` 新增 README + case01/02/03/10（`input.md` / `grader.md` / `artifacts/` 分离）。

**禁用短语 `rg -i` 于 `case*/input.md` 与 `case*/artifacts/`：0 命中**（`ADVANCE is wrong` / `must output REVISE` / `expected action` / `PASS condition` / `should not trigger` / `typically does not address`）。扩展扫描（`PASS|FAIL|grader|rubric|cosmetic|costume|weak-baseline|untuned|under-capacity|protection|Idea-gate|Evidence-gate|novelty threat|leak`）在 input 中仅命中任务框架用词（“idea-evaluation-only task”、“Idea-gate the … successor”）与作者推销句（“Please ADVANCE”），属 §C 允许的 author pitch。`case10/input.md` 无 skip 名单、无门 Skill 点名，评分只在 `grader.md`。costume 事实（`lambda_frozen: true`）在 `artifacts/`。历史 `cases/` README 与 `prompt-regression/` 未重写。残余提问者层面提示见 N4。

### Wave D — 有界深文献搜索

**✓。** `deep-literature-mode.md` 新增 `## G. Search budget (soft)` + `### One extension`：Pass 1 / Pass 2 默认上限、RQ actionable 即停、四类阻塞才允许一次延长且须写明改变哪个 revise/park/EXP 决策、三类“不算理由”；反复声明 “not a paper-count Protocol enum, not a LITERATURE field, not a STATE field”，`Pass 1 / Pass 2 / extension` 为工作文件判断注记。v0.2 `## Stop / cost` 两段内容原样并入 §G（heading diff 中唯一被移除的标题，内容未丢）。§B 末尾把 “follow names the hits revealed” 明确为 Pass 2。`literature-synthesis.md` handoff 增 `Search budget: … not a STATE field` 行、lens #6、`## Search budget` 输出段、Stop 规则；`literature-research` Skill 三处接线均写 “not a STATE field / no paper quota”。

### Wave E — 元规则去重

**✓ 未删判断算子。** 全部 diff 位于各文件头部治理段：RI 六件的 “Not Protocol” 段改为 “Layer-2 boundary: cite `scientific-reasoning.md`”，owner 段在 `scientific-reasoning.md` 集中列出 Outcome / Verdict / Story 三 owner 与禁建文件名单；四个 subagent 的 “Load this file first … Do not load every file … task prompt wins on shape” 三句改为 cite `subagent-handoff.md`——该 owner 文件（v0.2 起未改）L37–44 确实拥有这三条规则；`research-lead.md` 删去的 Outcome/Verdict cite 句由 “Protocol enumerations: cite that Layer-2 boundary” 覆盖。`framework-maintenance` 增 checklist #14（owner 三处：`scientific-reasoning.md` / `research-loop` / `subagent-handoff.md`；“not a 13th Skill”）。

标题级核对（`^#{1,3}` 对 v0.2 逐文件 diff）：RI 六件、12 prompts、12 Skills、5 subagents 中**无任何 §A–§H 算子段被删**；变化全部为 Wave A/B/D 的新增标题 + 上述 `Stop / cost → §G` 并入。§H glossary、§F 六标签、`experiment-thinking` §A–H、`scientific-reasoning` 全部段落原位。

治理句族收敛计数（`git grep -c`，v0.2 → HEAD）：`load every file under` 6 文件 → 1（owner）；`task prompt … wins` 6 → 3（owner + `next-research-move.md` 一处 cite + checklist 行）；`Outcome … only` 3 → 1；`Not Protocol` 12 行 / 9 文件 → 13 / 9（每个 RI 文件保留一行 cite owner，符合设计）。**未收敛：**“do not recopy / do not copy that table” 类限定语 44 行 / 25 文件 → 43 / 24（见 N5）。

### Wave F — 来源与路径卫生

**✓ 未猜测许可。** `docs/design/research-intelligence-sources.md` 改为 URL + 具体文件 + 许可证据 + 访问日期 + 概念 + 本地改写 + 是否复制文本；`idea-evaluator` 明确记录 frontmatter `CC-BY-4.0` 与仓库 `LICENSE` `CC-BY-NC-SA-4.0` **冲突且不裁定**，按 “abstract idea only” 处理；skill-doctor / skill-upper / experience-to-skill 记为 “not uniquely identified → License: unknown / abstract idea only”，未挂任何仓库 LICENSE；MIT 项均标 “LICENSE fetched 2026-09-04”。历史 `source-audit/` 快照与 harness 日志中的绝对路径保留为证据。`examples/mock-flow-detection/{README,.research/RESOURCES.md}` 去掉宿主绝对路径，改为相对位置 + host-dependent。README 删去 “Research Intelligence live …” 一行并（Wave E）加 “Harness matrix below is V0.2 tag evidence” 边界句；“Gate A and Gate B have not passed / live Wave G has not run” 表述诚实。

---

## 4. 尺寸度量（`git show <ref>:<path> | wc -l / wc -c`，仅 `*.md`）

### 4.1 目录合计

| 目录 | v0.2 行 | HEAD 行 | Δ行 | v0.2 字节 | HEAD 字节 | Δ字节 |
|---|---:|---:|---:|---:|---:|---:|
| `.agents/prompts/` (12) | 2355 | 2412 | **+57** | 101694 | 104556 | **+2862** |
| `.agents/skills/` (12) | 1669 | 1855 | **+186** | 77396 | 85977 | **+8581** |
| `.agents/subagents/` (5) | 754 | 736 | −18 | 28927 | 27877 | −1050 |
| `.agents/references/research-intelligence/` (6) | 1732 | 1847 | +115 | 61612 | 65962 | +4350 |
| `.agents/references/` Protocol 四件 | 612 | 612 | 0 | 22472 | 22472 | 0 |
| `.agents/templates/` (8) | 328 | 329 | +1 | 7723 | 7905 | +182 |

### 4.2 变更文件逐项

| 文件 | 行 v0.2→HEAD | Δ行 | Δ字节 | Wave |
|---|---|---:|---:|---|
| `skills/experiment-design/SKILL.md` | 131→218 | +87 | +3420 | A |
| `skills/result-analysis/SKILL.md` | 122→185 | +63 | +1981 | A, E |
| `skills/research-loop/SKILL.md` | 159→159 | 0 | +44 | A, E |
| `prompts/experiment-review.md` | 81→99 | +18 | +601 | B |
| `prompts/idea-evaluation.md` | 218→227 | +9 | +515 | B |
| `skills/experiment-review/SKILL.md` | 111→122 | +11 | +839 | B |
| `skills/idea-evaluation/SKILL.md` | 134→141 | +7 | +290 | B |
| `references/…/idea-and-mechanism.md` | 236→236 | 0 | +90 | B, E |
| `templates/REVIEWS.template.md` | 23→24 | +1 | +182 | B |
| `references/…/skill-evolution.md` | 282→346 | +64 | +2379 | C, E |
| `skills/framework-maintenance/SKILL.md` | 215→221 | +6 | +1223 | C, E |
| `prompts/literature-synthesis.md` | 221→251 | +30 | +1746 | D |
| `references/…/deep-literature-mode.md` | 297→348 | +51 | +1824 | D, E |
| `skills/literature-research/SKILL.md` | 125→137 | +12 | +784 | D |
| `references/…/evidence-and-claim.md` | 309→307 | −2 | −173 | E |
| `references/…/experiment-thinking.md` | 346→346 | 0 | −7 | E |
| `references/…/scientific-reasoning.md` | 262→264 | +2 | +237 | E |
| `subagents/experiment-agent.md` | 159→155 | −4 | −235 | E |
| `subagents/literature-scout.md` | 128→125 | −3 | −234 | E |
| `subagents/research-lead.md` | 125→117 | −8 | −389 | E |
| `subagents/result-analyst.md` | 153→149 | −4 | −235 | E |
| `subagents/reviewer.md` | 189→190 | +1 | +43 | E |

### 4.3 Wave E 单独（`607faac → 6b44c72`）

prompts 未动；skills 1855→1855 行、85545→85977 字节（+432，checklist #14 一长行）；subagents 754→736（−18）、28927→27877（−1050）；RI 1848→1847（−1）、66041→65962（−79）。**净 −19 行 / −697 字节。**

### 4.4 普通探索路径的名义默认加载集（Skill 文本要求加载的集合；非实测 token）

| 路径 | v0.2 | HEAD compact |
|---|---|---|
| `experiment-design` | SKILL 131/7179 + `scientific-reasoning` 262/11044 + `experiment-thinking` 346/11578 = **739 行 / 29801 B**（+ optional `experiment-proposal` 295/12050 → 1034 / 41851） | SKILL **218 行 / 10599 B** |
| `result-analysis` | SKILL 122/6809 + `result-diagnosis`（v0.2 步骤 2 默认）267/10452 + `scientific-reasoning` 262/11044 + `evidence-and-claim` 309/10182 = **960 行 / 38487 B**（`failure-diagnosis` 273/10484 条件） | SKILL **185 行 / 8790 B** |

**结论：** 成功判据 “meta-rule lines down, scientific operators not deleted” 在 Wave E 范围内成立（三个治理句族收敛到 owner；算子零删除），但 **全仓指令总量上升**（prompts +2.9 KB、skills +8.6 KB、RI +4.4 KB），增长来自 Wave A/B/D 的结构性新增。真实节省在 compact 默认加载集（名义 ≈ −64% / −77% 字节），README 已如实把 “compact-path token 测量” 列为 v0.2.1 债务，需 Wave G 实测（N5）。

---

## 5. Blocking items

**无。** 架构未破（8 / 1 / 5 / 12 / 6 / 0；Protocol 四件 byte-identical；无第 13 Skill、无第 7 RI 文件、无新 canonical 文件）；SSOT 未分叉（§2 全部 ✓）；`compact` / `full` 未进 Protocol / STATE / EXPERIMENTS / Status / Outcome。

---

## 6. Non-blocking items（措辞 / 后续 Wave 可选；均不要求 Gate A 前修）

| # | Wave | 位置 | 观察 | 建议（有界） |
|---|---|---|---|---|
| N1 | B | `prompts/experiment-review.md` L82；`skills/experiment-review/SKILL.md` L82–83 | Next 同步触发只列 `REVISE / REJECT / ATTENTION_REQUIRED`；`INSUFFICIENT_EVIDENCE` 若改变下一步（如 “先补某 artifact”）不触发，且属 5 选 3 的部分枚举 | 改为 “any Verdict other than `PROCEED` that changes the next action (per reviewer.md §Verdict)”；两文件各一句 |
| N2 | B | `skills/idea-evaluation/SKILL.md` L126；`prompts/idea-evaluation.md` L212 | `Memo: .research/work/idea-evaluation-<slug>.md` 为新引入的行内指针写法，`state-files.md` §DISCOVERY / `DISCOVERY.template.md` 只定义 `Evidence: EXP-xxx`；两消费者一致、非枚举、未触 Protocol | 保持为 prose 亦可；若要登记，需在 `state-files.md` 加一行 owner 句——那是 Protocol 改动，应单独过 gate，不在本轮 |
| N3 | A | `skills/experiment-design/SKILL.md` Compact flow 步骤 1–2 | 步骤 1 “STATE / DISCOVERY / PROJECT only if the Question is not already on disk”，步骤 2 又要求 “Do not redo a negated route”；最小查重集未点名 | 步骤 2 补一短语 “(EXPERIMENTS index Outcome column + DISCOVERY Invalidated / Negative headings)” |
| N4 | C | `live-cases/case03/input.md` L132–137；`case01/input.md` L60 | Case 03 MOCK EXPERIMENTS 段自述 “not given the same window-entropy features … No information-matched strong detector”；Case 01 PROJECT 原则 “Do not treat a renamed diagram as a new method.” 均为**事实/项目原则而非评分标签**，无禁用短语，但 §C 倾向此类事实放 `artifacts/` | Wave G 报告把它们列为残余提问者层面自曝（同 v0.2 E-N7 处理方式）；或后续夹具修订移入 artifacts。不得重写历史 `cases/` |
| N5 | E / 全局 | §4 | 总量上升；“do not recopy / do not copy that table” 限定语仍 43 行 / 24 文件，未按 #14 “>3 files → owner + cite” 收敛（多数为紧贴 cite 的限定短语，可辩护） | 记为下一轮去重候选；Wave G 在 Case 10 实测 compact 路径真实加载字节，替换 §4.4 的名义值 |
| N6 | D | `skills/literature-research/SKILL.md` Deviation 段 “note search scope in STATE if gap remains open”（v0.2 既有） | 与新句 “budget not a STATE field” 相邻，可能被误读 | 可加 “(scope, not pass count / budget)” |
| N7 | README | L71–72 | “Gate A and Gate B have not passed” 在本审核落地后需更新 | 发版步骤处理，不属本 gate |
| N8 | F | `docs/design/research-intelligence-sources.md` “Decision frontier” 行 “Wave D `workspace-resume` only” | 指 V0.2 Wave D；V0.2.1 亦有 Wave D（deep-lit），易混 | 改 “V0.2 Wave D” |

---

## 7. Verdict

**`APPROVE`**

理由：`v0.2..6b44c72` 全部 45 文件静态审读完成；冻结数 8 / 1 / 5 / 12 / 6 / 0 与三历史 tag、`origin/master == v0.2^{}` 全部核实；Protocol 四件与 7/8 模板 byte-identical，`reviewer.md` §Verdict 未动；SSOT 六项无分叉；`compact` / `full` 严格 Skill-internal；Wave A compact 路径在 `experiment-design` 与 `result-analysis` 均存在且映射既有字段；Wave B 落地 / REVIEWS / Next 同步引用的字段全部已存在；Wave C `input.md` 禁用短语 0 命中且历史夹具未重写；Wave D §G 为软预算、非 STATE 字段；Wave E 标题级零算子删除、被删治理句均有在位 owner；Wave F 许可冲突/未知均如实标注、不猜测。所有发现均为非阻塞措辞项（N1–N8）。

**Wave G 可以开始。** 建议 Wave G：只把 `live-cases/caseNN/input.md` + `artifacts/` 喂给被测 agent，`grader.md` / README 评分段 / 历史 `cases/*/README.md` 不进 prompt；逐次填写 README 的 Host memory check 与 Claude file-read trace 字段（trace 缺失就写缺失，不推断）；scorer ≠ candidate author，记录 `Model relation` / `Context relation`；顺带实测 Case 10 compact 路径实际加载文件与字节，替换本文件 §4.4 的名义值。
