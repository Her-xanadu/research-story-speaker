# Gate C Re-review — V0.2 Wave C 任务 prompts after TARGETED_REVISION

- **角色：** 独立 Gate C Re-reviewer。相对 Wave C prompt 作者及 B1–B3 补丁作者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘）。
- **审核时点：** 2026-09-04 05:04（UTC+8）
- **对象：** workspace `/Users/herxanadu/research-story-speaker`，分支 `v0.2-research-intelligence`，HEAD = `72eb338cee83a3213c30763a487e0696e1acf5f2`（与首轮 Gate C 审核一致，Gate B `APPROVE` 后提交）。单一 worktree。
- **工作树状态：** 已跟踪文件改动仍仅 4 个 prompt（`git diff --stat HEAD -- .agents/prompts/` = 4 files, +124 / −12；首轮为 +119 / −12，增量 5 行全部来自 `subagent-handoff.md` 的 B2 段）。未跟踪新增仍为 8 个 prompt + `gate-c-review.md` + `graphify-out/`。`.agents/skills/`（12 目录）、`.agents/references/`（Protocol 四件 + Layer 2 六件，mtime 均早于首轮审核）、`.agents/subagents/`（Sep 3）、`.research/`、`AGENTS.md`、`README.md`、`adapters/` **零改动**。
- **只写：** 本文件。未 git commit；未启动 Wave D；未改 prompts / Skills / references。
- **前序：** `gate-c-review.md`（Verdict `TARGETED_REVISION`；Blocking B1、B2、B3；Non-blocking N1–N12）。
- **Gate 判定词汇：** `APPROVE | TARGETED_REVISION` 由 Gate C 任务书规定，是框架层 gate 结论，**不是** `reviewer.md` §Verdict 词汇，也不是 Idea-gate 四动作，不新增任何 Protocol 枚举。

---

## 0. 补丁范围确认（磁盘证据）

| 文件 | mtime | 行数（首轮 → 本轮） | 判断 |
|------|-------|--------------------|------|
| `evidence-verification.md` | 04:56:18 | 261 → 237 | **被补丁修改**（B1） |
| `idea-evaluation.md` | 04:54:41 | 241 → 218 | **被补丁修改**（B1） |
| `experiment-proposal.md` | 04:54:45 | 305 → 295 | **被补丁修改**（B1） |
| `literature-synthesis.md` | 04:56:21 | 222 → 221 | **被补丁修改**（B1 顺带） |
| `subagent-handoff.md` | 04:53:23 | 88 → 93 | **被补丁修改**（B2） |
| `next-research-move.md` | 04:53:26 | 261 → 265 | **被补丁修改**（B2） |
| `failure-diagnosis.md` | 04:53:28 | 269 → 271 | **被补丁修改**（B3） |
| `result-diagnosis.md` | 04:30:03 | 267 → 267 | 未动（mtime 早于首轮审核 04:40） |
| `story-audit.md` | 04:30:59 | 212 → 212 | 未动 |
| `method-review.md` / `result-review.md` / `experiment-review.md` | 04:28 | 99 / 94 / 81 不变 | 未动 |

补丁只触及首轮 B1/B2/B3 点名的七个文件；七个被修改文件已逐字重读（共 1 600 行）。被引用 owner 中 `idea-and-mechanism.md`、`evidence-and-claim.md`、`experiment-thinking.md`、`experiment-record.md`、`reviewer.md`、`research-lead.md` 逐字重读；`scientific-reasoning.md`、`deep-literature-mode.md`、`story-loop.md` 核对标题结构。

**机械检查：** 首轮脚本目录 `/tmp/gatec/` 已被清空，本轮在 `/tmp/gatec-re/` 按首轮描述的口径重写（词级、小写、去标点、去链接目标；8-gram 共享计数；≥12 词连续逐字片段行号定位）。校准：对五个**未动**的 prompt 重跑，owner 集 = Layer 2 六件 + `experiment-record` / `state-files` / `story-loop` 时得 0.0–1.1%，与首轮记录的"其余 8 个 prompt ≤ 1.0%"一致，可作前后可比的内部对照。未写入仓库。

---

## 1. B1 — 去除对 Layer 2 owner 的逐字重抄

**要求（首轮验收）：** 三个 prompt 与其引用 owner 之间 ≥12 词连续逐字片段 = 0（链接 / Task fields 样板除外）；8-gram 共享占比降到其余 prompt 的水平（≤ 约 2%）；保留镜头名 + § 指针 + 本 prompt 特有的"在此如何应用"句。

### 1.1 计量结果

| prompt | 8-gram 总数 | 与 owner 共享（首轮 → 本轮） | 占比（首轮 → 本轮） | ≥12 词逐字片段（首轮 → 本轮） |
|--------|------------|------------------------------|---------------------|-------------------------------|
| `evidence-verification.md` | 1414 | 173 → **10** | 11.7% → **0.7%** | 11 段 / 231 词 → **0** |
| `idea-evaluation.md` | 1499 | 171 → **12** | 10.4% → **0.8%** | 9 段 / 205 词 → **0** |
| `experiment-proposal.md` | 1775 | 146 → **14** | 8.3% → **0.8%** | 9 段 / 178 词 → **0** |
| `literature-synthesis.md` | 1106 | 66 → **11** | 6.0% → **1.0%** | 4 段 / 74 词 → **0** |
| 未动的五个 prompt（对照） | — | 0–11 | 0.0–1.1% | 见 1.3 |

四个文件均落入未动 prompt 的区间。6-gram 口径下同样成立（B1 四件 2.7–3.8%，对照组 `result-diagnosis.md` 3.1%、`method-review.md` 3.7%、`result-review.md` 4.1%）。

### 1.2 首轮点名热点逐处核对（保留 § 指针 + 本地应用，删除 owner 复本）

| 首轮热点 | 本轮现文 | 判断 |
|----------|----------|------|
| `evidence-verification.md` 旧 L86–100 ← `evidence-and-claim.md` L25–40（四层不等式 + 整段解释） | L76–79："Keep four layers separate (evidence-and-claim.md §A). Cite §A; do not treat this prompt as a second owner. For each Task criterion, walk existence → validity → criterion-met → claim-supported, and stop at the first layer that fails." | 只留层名 + 本 gate 的走法；`metrics.json` 解释段已删 ✓ |
| 旧 L70–73 ← L57–60（四来源） | L67–70："pull from PROJECT, STORY, this EXP section, and any on-disk reviews — §B says what each source is for." | 来源名保留，释义交回 §B ✓ |
| 旧 L102–105 ← L263–265 / L250–253 | L85–88："Apply claim-kind discipline (scientific-reasoning.md §B) … Apply scope (evidence-and-claim.md §G) … do not inflate the sentence." | 一句应用 + 指针 ✓ |
| 旧 L112–115 ← L152–160（integrity 停止句、crash 非 negative） | L93–97：Integrity **before** interpretation (§D)；"Status / Outcome / DISCOVERY after an unusable run are Protocol plus result-analysis — cite result-diagnosis.md §8; do not restate that mapping." | 收成指针；`result-diagnosis.md` §8（L137–159）确为该映射的 prompt 侧唯一完整应用，N9 顺带部分闭合 ✓ |
| 旧 L187–190 ← L44–47（collapse 列表） | L161–170 "Also refuse:" 七条，以本 prompt 字段名重写（"Treating `Evidence found? = yes` as `Satisfaction` supports"…） | 非逐字；条目 ≤ 8 词；属本地应用 ✓ |
| `idea-evaluation.md` 旧 L81–85 ← `idea-and-mechanism.md` L75–88（§C 九轴 + identity 定义） | L78–81："Name the axis … (use only axes that apply — §C owns the list). Then run the **deletion test** (§G) … If identity is a costume named in §C, do not ADVANCE." | 九轴与六种 costume 表均不再复制 ✓ |
| 旧 L109–113 ← L152–155（§E novelty threat 整句） | L91–95："If §E's threat condition holds for this candidate, REVISE identity or PARK; do not ADVANCE on a new dataset alone." | 条件本体回到 §E ✓ |
| 旧 L139–141 ← L175–177（§F 37 词逐字） | L117–120：只列 `reuse` / `new` / `excluded` 三个名字 + "names from §F; excluded is mandatory" + 多 gadget 处置一句 | 三列表定义块未复制 ✓ |
| 旧 L147–152 ← L213–219（§H 最小实验整段） | L124–129："For this gate: name the smallest comparison that could change judgment, usually an isolating control from experiment-thinking.md §D … hitting the §B condition at the right unit. More seeds on a comparison that cannot split target vs rival do not make this test more decisive." | 37 词逐字块已消失；现为改写的本地应用（见 1.3 残留说明） ✓ |
| `experiment-proposal.md` 旧 L157–166 rival→control 映射 ← `experiment-thinking.md` §D L172–179 | L153–156："List only the controls this Question needs. Pick the kinds from experiment-thinking.md §D, driven by the named rival — cite §D; do not recopy that taxonomy as a second owner." | 与首轮 B1 处方逐字一致 ✓ |
| 旧 L170–172 confounder 十项 ← §C | L160–165："Pick from §C; do not recopy that list. Equalize the knobs that are the **best rival**." | ✓ |
| 旧 L177–180 ← §E L189–192（矩阵行） | L169–174："Cite §E; do not recopy that matrix as a second vocabulary. Every cell in *this* work file must split hypothesis vs primary rival." | 矩阵行零复制 ✓ |
| 旧 L213–218 ← §H L324–327（stop condition 37 词逐字） | L206–208："Write a stop line in this work file: when Must-run has enough to move the target claim, remaining cells are Nice-to-have or Cut (story-loop.md gap priority)." | 压成一句应用 ✓ |
| 单位 §B（旧整段定义 + 九种 grouping） | L130–142：四个单位只列名；"only groupings that exist in *this* dataset (§B owns the kinds)" | ✓ |
| `literature-synthesis.md` 旧 L28–29 / L33–34 / L107–109 / L119–121 | L28–29 "cite that file's load rules; do not restate its skip list"；L58 §A–F 只列节名；L74、L91 depth label 用字串、不抄映射表；L138 §H "do not recopy that glossary" | ✓ |

### 1.3 残留（均在验收线内，非阻塞）

- 8 个新 prompt 中仅剩 **一处** ≥12 词逐字片段，且不在 B1 三件：`failure-diagnosis.md` L160–161 "Sweeps are last, and only when the Question is about a threshold or budget" ← `experiment-thinking.md` L282–283（14 词），紧跟 §G 引用；首轮已将该文件计入"≤1.0%"对照组，未被点名。其余 ≥12 词命中均为 Task fields 样板（← `reviewer.md` L81–84）或首轮已接受的 `method-review.md` L29–30 触发句、`result-review.md` L60 integrity 项目串，文件未动。
- B1 四件的 9–11 词残留片段共 13 处，全部为单句守则或触发短语（例：`evidence-verification.md` L11–14 "§F labels are verification-report labels…" / "Do not paste a report label into EXPERIMENTS.md as Outcome"；`idea-evaluation.md` L179–180 "…is progress. Adding components until the Idea looks busy is not"；`experiment-proposal.md` L110 / L123 的 Hypothesis / Prediction 九词定义短语），无定义表、无列表、无整段解释。
- `idea-evaluation.md` §9（L124–129）与 `experiment-proposal.md` Cut（L192–195）虽已无逐字复本，仍是 owner 逻辑的改写而非纯指针；若 §H / §F 日后修订，这两处需人工同步。这属首轮 B1 "允许 prompt 更长，只要新增的是 prompt 自己的判断句"的边缘情况，记入 N15 供 Wave D / release audit 观察，不构成阻塞。

**B1：已关闭。**

---

## 2. B2 — 任务 prompt 与角色文件的输出结构优先级

**要求：** `subagent-handoff.md` 声明——任务 prompt 赢工作产物 *shape*（本次 dispatch）；角色文件赢 *write permissions*。`next-research-move.md` 不得自称替代 `research-lead.md`。

| 位置 | 现文 | 判断 |
|------|------|------|
| `subagent-handoff.md` L40–44（`git diff HEAD` 新增） | "When a task prompt is attached and it disagrees with `.agents/subagents/<role>.md` on work-artifact **shape** (Required output headings), the **task prompt** wins for this dispatch. The role file still wins on **write permissions** (work/reviews only; Main owns the canonical eight)." | 两个维度、两个赢家、限定"this dispatch"，与 B2 要求逐项对应 ✓。"canonical eight" 与同文件 L63 Write rules 的八个文件一致 ✓ |
| `next-research-move.md` L212–216 | "Work-file headings for this dispatch (task prompt wins on artifact shape per subagent-handoff.md; research-lead.md still owns write permissions, independence, and the quality bar — this prompt does not replace that role contract). Fourth heading is `## why now` in the work file; the role file's default fourth heading is `## reasoning summary`" | 首轮 L212 "superset" 表述已删（`rg -i superset .agents/prompts/` 零命中）；显式声明不替代角色契约；第四标题差异被明说而非掩盖 ✓ |
| `next-research-move.md` L238–240 | "Return to caller: bottleneck, candidates, recommended action, and why now. Keep the return concise; the work file holds the per-candidate fields." | 与 `research-lead.md` L72 "~80 lines" 返回约束相容（N7） ✓ |

**验收核对：** 首轮补充发现表中四条线（research-lead / result-analyst / experiment-agent / literature-scout）在"角色文件 + 任务 prompt"下现在各只有一套 Required output——附带任务 prompt 时用 prompt 的标题，不附带时用角色文件默认结构。四个角色文件本身未动（Sep 3），其后续对齐仍留给 Wave D（首轮 §6 第二条）。

**一处潜在边界（非阻塞，记 N13）：** 该规则是通用措辞，未把 Reviewer 排除在外；`reviewer.md` L161–162 规定 "Task prompts … must use these headings"（Protocol 冻结）。当前 `method-review.md` L83–90 / `result-review.md` L78–85 与 `reviewer.md` 五标题逐字一致，规则的"disagrees"前提不成立，故无实际冲突；建议 Wave D 在 L40–44 加半句 "(Reviewer excepted: reviewer.md §Verdict and its five headings always win)" 以封住未来漏洞。

**B2：已关闭。**

---

## 3. B3 — `failure-diagnosis.md` 伪 Status 令牌 `blocked`

```text
rg -n 'blocked' .agents/prompts/
→ 仅 result-diagnosis.md:73 "If a step is blocked by an earlier failure, write `n/a — <why>`"（普通英语动词，无反引号，文件未动）
→ failure-diagnosis.md 零命中
```

| 首轮位置 | 现文 | 判断 |
|----------|------|------|
| 旧 L130 "then `blocked` with a reason" | L129–130 "then stop debugging and have Main record a blocker in STATE." | 反引号令牌已除；写入者与落点（Main → STATE blockers）明确 ✓ |
| 旧 L263 "record `blocked` and stop" | L263–265 "Main records a blocker in STATE; this prompt does not invent a Status token." | 同上，并显式否认造 Status ✓ |

`experiment-record.md` §Status 值六个令牌不变（L52–59）；prompt 侧不再出现第七个候选。

**B3：已关闭。**

---

## 4. Protocol 冻结再确认（任务书要求项）

| 项 | 实测 | 判定 |
|----|------|------|
| `reviewer.md` 五标题 / Verdict 契约 | `git diff HEAD -- .agents/subagents/` 为空；`method-review.md` / `result-review.md` 未动（mtime 04:28）；Verdict 五值中仅 `REVISE` 出现于 `method-review.md` L56 且同句写 "Use reviewer.md §Verdict — do not…"；`PROCEED / REJECT / INSUFFICIENT_EVIDENCE / ATTENTION_REQUIRED` 在 prompts 零出现 | **PASS**（未被重抄） |
| Outcome / Status 表 | 定义型正则（`\| token \|` 表行、`token — …` 词条行）在 `.agents/prompts/` **零命中**；令牌只在应用句中紧跟 § 引用（`result-diagnosis.md` L146–158、`failure-diagnosis.md` L185 / L244、`result-review.md` L94） | **PASS**（未被重抄） |
| §F 六个报告标签 / Idea-gate 四动作 | 六标签无独立列表（`rg '^supports$'` 零命中）；四动作无 `TOKEN —` 行；`idea-evaluation.md` L6–10、L190–193、`literature-synthesis.md` L137–138 只引 §H | **PASS** |
| 相对链接 | 128 / 128 可解析（首轮 125；新增 3 条均为 B1/B2 改写时加入的 owner 指针） | **PASS** |
| `§` 引用 | 54 处字母 / 数字节引用逐条映射：`scientific-reasoning` §A–G、`evidence-and-claim` §A–G、`experiment-thinking` §A–H、`idea-and-mechanism` §A–H、`deep-literature-mode` §A–F、`result-diagnosis.md` §8（新增指针，对应 "### 8. Outcome candidate" L137）全部存在；中文标题 `§Outcome 值`(11×) / `§Status 值`(1×) / `§Verdict`(7×) / `§Gap 优先级`(3×) / `§反重复`(2×) / `§停滞处理`(3×) / `§更新顺序`(2×) 均对应 owner 真实标题。脚本 3 个"未找到"均为 `§Gap 优先级` 被截为 `§G` 的正则伪命中（与首轮同型） | **PASS** |
| 禁止文件名 / `.research/` | `HYPOTHESES / RUBRIC / CLAIMS / FINDINGS / PROGRESS / SURVEY / TASK_PLAN` 在 prompts 中只以否定句出现；`.research/` 仍恰好 8 个 canonical 文件 + `work/.gitkeep` + `reviews/.gitkeep` | **PASS** |
| Skill 标题对齐（B1 改写是否漂移） | `evidence-verification.md` L203–205 列出的十个 block 标题与 `skills/evidence-verification/SKILL.md` L72–81 逐字一致；`idea-evaluation.md` L190 "Problem Anchor through Recommended Action" 与 `skills/idea-evaluation/SKILL.md` L55–65 首尾一致 | **PASS** |
| 新 prompt 接线 | 8 个新 prompt 在 `.agents/prompts/` 之外仍零引用；唯一入口 `subagent-handoff.md` L46–52 角色表 | 符合 Wave C 范围；接线留 Wave D |

---

## 5. 十问增量复核（仅就补丁是否削弱首轮 PASS 的 operator）

| 问 | 首轮判定 | 本轮复核 |
|----|----------|----------|
| Q1 不止于"再跑更多" | PASS | B1 删的是 owner 复本，非 prompt 判断句：`evidence-verification.md` L138–140 Required action 五选一仍在；`idea-evaluation.md` L124–129 minimum test 仍在；`next-research-move.md` L142–153、`failure-diagnosis.md` L111–124 未动。**PASS** |
| Q2 最关键 rival | PASS | `idea-evaluation.md` L99–104、`experiment-proposal.md` L85–90 / L114–119 保留 "best rival → 削弱它的 control / split"。**PASS** |
| Q3 缺哪个 control | PASS | `experiment-proposal.md` L153–156 由 rival 反推（改为 §D 指针，不损判断）；L240–241 mechanism claim 无隔离 control → revise；`evidence-verification.md` L138–139 Gap 必须写明缺什么。**PASS** |
| Q4 must / nice / cut | PASS | `experiment-proposal.md` L178–195 三个独立标题、"cutting them is the successful design"；L206–208 stop line。**PASS** |
| Q5 execution vs hypothesis failure | PASS | `failure-diagnosis.md` 九类 / 决策逻辑未动（B3 仅改两行）；`evidence-verification.md` 改为指向 `result-diagnosis.md` §8，不再第四次复述映射。**PASS** |
| Q6 cosmetic innovation | PASS | `idea-evaluation.md` L78–81 deletion test + costume（由 §C 持有种类）；`experiment-proposal.md` L225–228、L290–291；`literature-synthesis.md` L214–215。**PASS** |
| Q7 Story scope | PASS | `story-audit.md` 未动；`evidence-verification.md` L131–134 Claim scope；`next-research-move.md` L129–133。**PASS** |
| Q8 最小判别实验 | PASS | `idea-evaluation.md` §9；`experiment-proposal.md` L144–149 Primary Comparison；`next-research-move.md` L150–153；`literature-synthesis.md` L100–101、L183。**PASS** |
| Q9 重复规则 | **FAIL → B1** | 见 §1：四件均降到对照组水平，≥12 词片段归零。prompt ↔ prompt 共享未因补丁上升（最高仍是 `method-review ~ result-review` 91 个 Reviewer 样板 8-gram；其余对 ≤ 26）。**PASS** |
| Q10 简单任务不做重 | PASS（软点 N3/N4/N8） | 触发不符 → 停止不写文件的保护均保留（`idea-evaluation.md` L32–34 / L166；`evidence-verification.md` L44–47 / L184；`experiment-proposal.md` L48–51；`literature-synthesis.md` L3–6 / L31–33；`next-research-move.md` L54–57 / L184；`subagent-handoff.md` L37–39 / L54–58）。`experiment-proposal.md` L75–77 新增 "unless Task fields already mark the EXP explicitly exploratory (scientific-reasoning.md §F)"，朝 N4 方向走了半步。**PASS** |

---

## 6. Verdict

**`APPROVE`**

理由：B1、B2、B3 均按首轮验收标准在磁盘上闭合——B1 三个 prompt（加 `literature-synthesis.md`）与 Layer 2 owner 的 ≥12 词逐字片段从 33 段 / 688 词降到 0，8-gram 共享占比从 6.0–11.7% 降到 0.7–1.0%（与未动 prompt 同一区间），且每处热点都保留了镜头名 + § 指针 + 本 prompt 的应用句而非单纯删除；B2 在映射处 `subagent-handoff.md` 一次性声明了 shape / write-permission 两个维度的赢家，`next-research-move.md` 撤回 "superset" 并明说不替代角色契约；B3 两行伪 Status 令牌改为 "Main records a blocker in STATE"。补丁范围严格限于七个被点名的 Wave C 交付文件；Protocol / Skills / 角色文件 / `.research/` 零改动；`reviewer.md` 契约与 Outcome / Status 表仍未被重抄；链接与 § 引用全部可解析；Q1–Q8 的 judgment operators 在改写后全部在场且跨 prompt 一致。12 个 prompt 可作为 Wave D 路由接线的基础。

---

## 7. Non-blocking（沿用首轮 N1–N12；补丁未使任何一项升级为阻塞；新增 N13–N15）

- **N1** 未处理：`idea-evaluation.md` L170–171 "(this mechanism cannot be the answer)"、L175 "(sequencing / budget)"、L177 "Identity clear, no fatal flaw, minimum test specified → ADVANCE per §H" 仍回声 §H 四行 glossary 的释义。保留路由、删括号释义即可。
- **N2** 未处理：`failure-diagnosis.md` L68、L113 仍只写 "not Status / Outcome / Verdict"，未加 "not Idea-gate actions"；九类 / 八动作的 owner 声明仍缺。
- **N3** 未处理：`subagent-handoff.md` L49 literature-scout 的 Task prompt 列仍无条件列出 `literature-synthesis.md`（Layer 2 列已写 "when requested"）；L50 experiment-agent 两个 prompt 未注明各自触发条件。
- **N4** 部分处理：`experiment-proposal.md` L75–77 允许显式 exploratory EXP 跳过"四要素先于 Compute"，但 L96–97 "must contain all of the following headings … not empty labels" 仍无 `n/a — exploratory, <why>` 规则；L55 "New EXP" 触发未收窄。
- **N5 / N6 / N8** 未处理（`method-review.md` L55–56 预设 `REVISE`；`evidence-verification.md` L99–141 七个教学小节名仍是第三套命名，但 L203 已明说输出标题以 Skill 为准；`failure-diagnosis.md` L237 对工程类失败仍要一段 "why not a sweep"）。
- **N7** 已相容（§2）。
- **N9** 部分处理：`evidence-verification.md` 已收成 `result-diagnosis.md` §8 指针；`failure-diagnosis.md` L185、L244 仍各复述一次（均引 Protocol，形式合规）。
- **N10** 保持现状 ✓。**N11** Skill 级 B-N1–N8 仍待 Wave D 回到 Skill 文件。
- **N12** 提交卫生：`graphify-out/` 未跟踪且仓库仍无 `.gitignore`；`gate-reviews/.gitkeep` 与 5 个实文件并存。提交 Wave C 前处理。
- **N13（新）** `subagent-handoff.md` L40–44 的 shape 优先级规则未排除 Reviewer；当前两个 review prompt 与 `reviewer.md` 五标题一致故不触发，但建议加半句 Reviewer 例外，避免未来某个 review prompt 借此规则绕过 `reviewer.md` L161–162。
- **N14（新）** `failure-diagnosis.md` L160–161 是 8 个新 prompt 中唯一残留的 ≥12 词逐字句（14 词，紧跟 §G 引用）；`experiment-proposal.md` L224 "All eight controls" 把 §D 表的行数写死。两处均可在 Wave D 顺手改为 "Sweeps are last (experiment-thinking.md §G)" / "every §D control"。
- **N15（新）** `idea-evaluation.md` L124–129、`experiment-proposal.md` L192–195 已无逐字复本，但仍是 §H / §F 逻辑的完整改写；Layer 2 修订时需人工同步。若 V0.2 release audit 希望进一步收紧，可把这两段压成"§H 的 minimum test，落在 §B 条件与正确单位上；不要在此分配 EXP-ID"一类的纯应用句。

---

## 8. 本审核未覆盖

- 五个未被补丁修改的 prompt 仅以 mtime（早于首轮审核）与行数一致性确认未变，未逐字重读；其首轮结论沿用。
- 未运行行为 fixture（`cases/`、`prompt-regression/` 仍为空）；Q1–Q10 与 B2 的"只有一套 Required output"均为静态判读。
- n-gram 脚本为按首轮描述重写的近似口径，已用未动 prompt 校准（0.0–1.1% vs 首轮 ≤1.0%），用于定位与前后比较，非精确计量。
- 未复核 Layer 2 六件、两个新 Skill、四个角色文件的内容本身（Gate A / A-re / B 已 `APPROVE`，且本轮零改动）。
- 未评估 Wave D–H 计划。
