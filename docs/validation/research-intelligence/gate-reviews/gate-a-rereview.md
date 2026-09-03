# Gate A Re-review — V0.2 Wave A Layer 2（Research Intelligence）after TARGETED_REVISION

- **角色：** 独立 Gate A Re-reviewer。相对 Wave A 开发者及 B1/B2 补丁作者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘）。
- **审核时点：** 2026-09-04 04:06（UTC+8）
- **对象：** workspace `/Users/herxanadu/research-story-speaker`，分支 `v0.2-research-intelligence`，HEAD = `dd7e43f72db1883209baf7cfef151ac218a17b92`（与首轮审核及 `BASELINE.md` 一致）。单一 worktree。
- **工作树状态：** `git diff` / `git diff --cached` 均为空 → 已跟踪文件（Skills、prompts、Protocol references、`.research/`、`AGENTS.md`、adapters）**零改动**。未跟踪目录仍为四项：`.agents/references/research-intelligence/`、`docs/design/`、`docs/validation/research-intelligence/`、`graphify-out/`。
- **只写：** 本文件。未 git commit；未启动 Wave B。
- **前序：** `gate-a-review.md`（Verdict `TARGETED_REVISION`；Blocking B1、B2；Non-blocking N1–N10）。
- **Gate 判定词汇：** `APPROVE | TARGETED_REVISION` 为 Gate A 任务书规定的框架层 gate 结论，不是 `reviewer.md` §Verdict 词汇，不新增任何 Protocol 枚举。

---

## 0. 补丁范围确认（磁盘证据）

| 文件 | mtime | 行数（首轮 → 本轮） | 判断 |
|------|-------|--------------------|------|
| `idea-and-mechanism.md` | 04:05:03 | 241 → 236 | **被补丁修改**（B1） |
| `deep-literature-mode.md` | 04:05:10 | 302 → 297 | **被补丁修改**（B1） |
| `skill-evolution.md` | 04:05:20 | 279 → 282 | **被补丁修改**（B2） |
| `scientific-reasoning.md` | 03:43:35 | 262 → 262 | 未动（mtime 早于首轮审核 04:02） |
| `evidence-and-claim.md` | 03:43:43 | 309 → 309 | 未动 |
| `experiment-thinking.md` | 03:43:09 | 346 → 346 | 未动 |
| `docs/design/research-intelligence-sources.md` | 03:42:26 | — | 未动（N8 仍开放） |
| `BASELINE.md`、`source-audit/*` | 03:42 | — | 未动 |

补丁只触及首轮 B1/B2 点名的三个文件；三个被修改文件已逐字重读（共 815 行）。

---

## 1. B1 — Idea-gate 四动作单一 owner

**要求：** `ADVANCE | REVISE | PARK | ABANDON` 只在 `idea-and-mechanism.md` §H 定义一次；§D 与 `deep-literature-mode.md` §F 只引用。

**验收命令与结果：**

```text
rg -n "(ADVANCE|REVISE|PARK|ABANDON)\s+—|—\s+(ADVANCE|REVISE|PARK|ABANDON)|\b(ADVANCE|REVISE|PARK|ABANDON)\b.*\bif\b|\bif\b.*\b(ADVANCE|REVISE|PARK|ABANDON)\b" .agents/references/research-intelligence/
→ 仅 4 行命中，全部位于 idea-and-mechanism.md §H L225–228（四个动作各一条 "—" 释义）
```

`rg -n "ADVANCE|ABANDON|\bPARK\b|\bREVISE\b"` 全部 13 处命中逐条核对：

| 位置 | 内容 | 性质 |
|------|------|------|
| `idea-and-mechanism.md` §H L225–228 | 四条 "—" 释义 | **唯一定义处** ✓ |
| `idea-and-mechanism.md` §D L124 | "A single fatal flaw → do not ADVANCE; pick REVISE / PARK / ABANDON per §H." | 引用 §H，无释义 ✓（首轮 L126–129 三条 "if…" 释义已删） |
| `idea-and-mechanism.md` §D L126–130 | "Those four actions … are Idea-gate recommendations only … not Protocol enums / Outcome / Verdicts" | 保留一句 not-Protocol 声明（首轮允许） ✓ |
| `idea-and-mechanism.md` L102、L113、L154、L235 | "Do not ADVANCE…" / "stop ADVANCE" / "REVISE the identity or PARK until…" / "If the honest action is PARK or ABANDON, stopping is progress" | 使用动作，不定义动作；首轮已存在，未被标记 ✓ |
| `deep-literature-mode.md` §F L236–241 | "Then recommend one action per `idea-and-mechanism.md` §H. A novelty threat with no real mechanism or information axis maps to REVISE or PARK per that owner, not ADVANCE. Recommend ADVANCE from this mode only when the matrix shows a mechanism or information difference and a discriminating test exists. These are Idea-gate recommendations, not Outcome and not Verdict." | 首轮 L238–243 的三条文献版释义代码块已删；改为指向 owner 的散文 + 文献特有触发条件 ✓；"列三说四"计数错误随之消失 ✓ |
| `deep-literature-mode.md` §C L145 | "→ not a Core Idea replacement; maybe a cheap EXP, maybe PARK" | 使用，不定义 ✓ |

**一致性核对：** `deep-literature-mode.md` §F 的触发条件（mechanism/information 差异 + discriminating test → ADVANCE；无真实轴 → REVISE / PARK）与 §H 释义（ADVANCE = "identity clear, no fatal flaw, minimum test specified"）及 `idea-and-mechanism.md` §E L151–154（novelty threat → "REVISE the identity or PARK"）语义一致，未形成第二套判据。其余四个 Layer 2 文件中这四个 token **零出现**。

**B1：已关闭。**

---

## 2. B2 — `skill-evolution.md` 悬空 / 错误引用

**要求：** 不引用不存在的 Case 10；不说 13th Skill；冻结事实 = 现有 10 个 Skill、V0.2 之后恰好加 2 个、本文件不是 Skill。

**验收命令与结果：**

```text
rg -n "Case 10|13th|thirteenth" .agents/references/research-intelligence/   → 零命中
（全仓库仅 gate-a-review.md 自身提及这些字串，属审核记录，符合预期）
```

逐处核对：

| 位置 | 现文 | 判断 |
|------|------|------|
| L11–15 | "**Not a Skill.** V0.1.1 has 10 scientific/workflow Skills; V0.2 will add exactly 2 later (`idea-evaluation`, `evidence-verification`). This Layer-2 file is none of those and is not a research-loop step." | 与冻结事实一致 ✓（首轮 L11 "13th scientific Skill" 已除） |
| L134–137 | "The canonical **protection** pattern is: ordinary exploratory EXP must stay light; do not fire every intelligence gate on a sanity rerun. Fixtures for that pattern belong under `docs/validation/research-intelligence/cases/` (none numbered yet)." | 自足描述；如实反映磁盘状态（`cases/` 目前仅 `.gitkeep`）；不再依赖不可解析的编号 ✓ |
| L199–201 | "…it fails that protection pattern (ordinary exploratory EXP stays light). **Reject.**" | 引用模式而非 Case 编号 ✓ |
| L238–241 | "V0.1.1 keeps 10 scientific/workflow Skills; V0.2's two later Skills are `idea-evaluation` and `evidence-verification`. This Layer-2 file is not a Skill and does not authorize adding any other." | 与冻结一致；"thirteenth anything" 已除 ✓ |

**两个未来 Skill 名的来源核对：** `idea-evaluation` / `evidence-verification` 并非本补丁新造——未被补丁触碰的 `scientific-reasoning.md` L219 与 `experiment-thinking.md` L338（mtime 03:43）在首轮审核前已使用同一对名字；`idea-and-mechanism.md` L179 亦称 "idea-evaluation work artifact"。补丁只是让 `skill-evolution.md` 与既有 Layer 2 文件对齐，未引入新实体。

**B2：已关闭。**

---

## 3. 十问复核（针对补丁是否引入新的 Protocol 分叉）

| 问 | 首轮判定 | 本轮复核（仅就补丁增量） |
|----|----------|--------------------------|
| Q1 ownership 重叠 | 一处实质重叠（B1） | B1 已消除；`deep-literature-mode.md` §F 与 `idea-and-mechanism.md` §E 对 novelty threat 的处置一致且均回指 §H，属可接受软重叠。无新重叠。 |
| Q2 第二套 Protocol | 否 | 否。补丁未新增枚举、状态文件、orchestrator。`skill-evolution.md` 提及两个未来 Skill 名是陈述计划，不创建 Skill；`.agents/skills/` 仍恰好 10 个目录。 |
| Q3 重定义 Outcome / Verdict | 否 | 否。三个被修改文件中 Outcome / Verdict 仍只作 "not …" 引用；`.agents/references/*.md`（Protocol）零改动（git 证据）。 |
| Q4 过度僵硬 | 否 | 否。补丁未新增任何面向所有 EXP 的 "must"；protection pattern（"ordinary exploratory EXP stays light"）保留并更清晰。 |
| Q5 三域通用 | 是（deep-lit 偏安全域） | 补丁未触及 §B/§D 示例；N7 状态不变。 |
| Q6 因果 / 统计原则 | 无错 | 补丁不涉及；N3/N4 所在文件未动。 |
| Q7 判断力 vs token | 提升明显 | B1 净减 10 行重复释义；B2 净增 3 行但为消除悬空引用所需。无新增空转文本。 |
| Q8 上游复制 | 零复制 | 对三个被修改文件重跑 6-gram / 8-gram 扫描（vs `deep-research/**`、`idea-evaluator/**`、ARIS `experiment-plan/**` 共 21 个 `.md`）：**8-gram = 0**；6-gram 仅 1 个 = 许可证字串 "cc by nc sa 4 0"（`deep-literature-mode.md` L12），与首轮相同。脚本置于 `/tmp`，未写入仓库。 |
| Q9 registry 完整性 | 四处可补（N8） | registry 未动；N8 保持非阻塞。 |
| Q10 应删内容 | B1、B2 + N1/N6/N9 | B1、B2 项已删除；N1/N6/N9 保持非阻塞。 |

**冻结核心再确认：** `.research/` 恰好 8 个 canonical 文件 + `reviews/.gitkeep` + `work/.gitkeep`（`work/` 无新增）；Skill 目录恰好 10；禁止文件名（HYPOTHESES / RUBRIC / CLAIMS / FINDINGS / PROGRESS / SURVEY / TASK_PLAN）全仓库零命中；Layer 2 六文件在 `AGENTS.md`、`CLAUDE.md`、`README.md`、`.agents/skills`、`.agents/prompts`、`.agents/subagents`、`adapters`、`.agents/references/*.md` 中零引用（Wave B 未开始，符合预期）。

---

## 4. Verdict

**`APPROVE`**

理由：B1、B2 均按首轮验收标准在磁盘上闭合；补丁范围严格限于三个被点名文件；补丁增量未引入任何新枚举、新 owner 争议、新状态文件或新调度者；Protocol / Skills / prompts / `.research/` 零改动；上游零复制结论对被修改文件重跑后不变。六个 Layer 2 文件可作为 Wave B 锚定 Skills / prompts 的基础。

---

## 5. Non-blocking（沿用首轮 N1–N10；补丁未使任何一项升级为阻塞）

- **N1**（小幅扩展）计划期措辞进入长期 reference：首轮所列 `skill-evolution.md` L20–21（"Wave D will name…"）、L243–245（"Wave H may dogfood…"）、`deep-literature-mode.md` L20–21（"Wave D may name…"）仍在；补丁另引入同类措辞——`skill-evolution.md` L11–12、L239–240 "V0.2 will add exactly 2 later / V0.2's two later Skills"（V0.2 发布后即成过去时）与 L137 "(none numbered yet)"（fixture 落盘后即过时）。建议在 V0.2 release audit 一并改为不带 Wave 字母 / 时态承诺的表述。另 L243–244 把 `idea-evaluation` 称为 "one prompt" 而 L12/L240 称其为 Skill；在本框架中每个 Skill 配有 prompt，语义可解，但统一为 "the `idea-evaluation` Skill's prompt" 更清楚。
- **N2–N7、N10** 所在文件（`scientific-reasoning.md`、`evidence-and-claim.md`、`experiment-thinking.md`、`deep-literature-mode.md` §B/§D）未被补丁触及，状态与首轮相同。
- **N8** registry 未动："(`X.md` when Wave A lands)" 措辞与 Q9 (a)–(d) 补记仍待 registry owner 处理。
- **N9** `.agents/references/research-intelligence/.gitkeep` 仍在（目录已有六个实文件）；`graphify-out/` 仍未跟踪且无 `.gitignore`。提交 Wave A 前处理。

---

## 6. 本审核未覆盖

- 三个未被补丁修改的文件仅以 mtime（早于首轮审核）与行数一致性确认未变，未逐字重读。
- 未运行行为 fixture（`cases/`、`prompt-regression/` 仍为空）；Q4 / Q7 仍为静态判断。
- 未复核标签 peeled SHA（`BASELINE.md` 已记录，非本 gate 任务）。
- 未评估 Wave B–H 计划本身。
