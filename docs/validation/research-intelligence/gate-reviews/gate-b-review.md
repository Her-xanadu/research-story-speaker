# Gate B Review — V0.2 Wave B 两个新 Skill（idea-evaluation / evidence-verification）

- **角色：** 独立 Gate B Reviewer。相对 Wave B Skill 作者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘）。
- **审核时点：** 2026-09-04 04:20（UTC+8）
- **对象：** workspace `/Users/herxanadu/research-story-speaker`，分支 `v0.2-research-intelligence`，HEAD = `8008a7cbb7252305645eab8a42e9f35515afd0e0`（"V0.2 Wave A: add research-intelligence reference layer"，其父即 `BASELINE.md` 冻结 HEAD `dd7e43f7`；Wave A 已在 Gate A `APPROVE` 后提交，提交内容为六个 Layer 2 文件 + docs，未触及 Skills / prompts / Protocol / `.research/`）。单一 worktree。`v0.1.1` / `v0.1` peeled commit 与 `BASELINE.md` 一致（`762deb4c` / `8db3b301`）。
- **工作树状态：** `git diff --stat` / `git diff --cached --stat` 均为空 → **没有任何已跟踪文件被修改**（现有 10 个 Skill、prompts、Protocol references、Layer 2 六文件、`.research/`、`AGENTS.md`、`README.md`、adapters 全部未动）。未跟踪新增：`.agents/skills/idea-evaluation/`、`.agents/skills/evidence-verification/`、`.claude/skills/idea-evaluation`、`.claude/skills/evidence-verification`（两个符号链接）、`docs/validation/research-intelligence/gate-reviews/.gitkeep`、`graphify-out/`（工具残留，沿用 Gate A N9）。
- **只写：** 本文件。未 git commit；未启动 Wave C。
- **Gate 判定词汇：** `APPROVE | TARGETED_REVISION` 由 Gate B 任务书规定，是框架层 gate 结论，**不是** `reviewer.md` §Verdict 词汇，也不是 Idea-gate 四动作，不新增任何 Protocol 枚举。

---

## 0. 阅读范围与方法

**新 Skill（逐字读完）：** `.agents/skills/idea-evaluation/SKILL.md`（122 行 / 4,924 B）、`.agents/skills/evidence-verification/SKILL.md`（120 行 / 5,056 B）。

**被引用的 Layer 2（逐字读完）：** `idea-and-mechanism.md`（236）、`evidence-and-claim.md`（309）、`scientific-reasoning.md`（262）、`experiment-thinking.md`（346）；`skill-evolution.md` §C–E（protection pattern）。

**被引用的 Protocol / 契约：** `experiment-record.md`、`reviewer.md`、`git-linking.md`、`state-files.md`、`.agents/prompts/subagent-handoff.md`。

**重复性对照（逐字读完）：** `experiment-design/SKILL.md`、`result-analysis/SKILL.md`、`experiment-review/SKILL.md`、`research-loop/SKILL.md`；`AGENTS.md`、`README.md` L54、`adapters/codex.md` L20。

**机械检查（脚本置于 `/tmp`，未写入仓库）：** 目录计数与符号链接解析；YAML frontmatter 解析；相对链接逐条 `test -e`；`rg` 扫描 Outcome / Status / Verdict 枚举、§F 报告标签、§G 措辞词、四动作词汇及 Gate A 的"定义型"正则；`find` 扫描禁止文件名；两个新 Skill vs 上游 `~/.agents/skills/idea-evaluator/**`、`~/.agents/skills/deep-research/**`、`~/aris_repo/skills/experiment-plan/**`（21 个 `.md`）的 6-gram / 8-gram 共享片段；vs 三个既有 Skill 的 8-gram；vs Layer 2 与 Protocol 文件的 10-gram。

---

## 1. 计数与冻结核心确认

| 项 | 要求 | 实测 | 判定 |
|----|------|------|------|
| `.agents/skills/` 目录数 | 恰好 10 + 2 = 12，无第三个新 Skill | `find -mindepth 1 -maxdepth 1 -type d` = **12**：原 10 个 + `idea-evaluation` + `evidence-verification`；每个新目录仅含一个 `SKILL.md` | **PASS** |
| `.claude/skills/` 符号链接 | 若存在须指向 `.agents/skills/` 对应目录 | 12 个链接；两个新链接 `idea-evaluation -> ../../.agents/skills/idea-evaluation`、`evidence-verification -> ../../.agents/skills/evidence-verification`，`test -f .claude/skills/<x>/SKILL.md` 均解析成功；形式与既有 10 个链接一致 | **PASS** |
| `.research/` canonical 文件 | 恰好 8 个，`work/` 无新增 | `DISCOVERY / EXPERIMENTS / LITERATURE / PROJECT / RESOURCES / REVIEWS / STATE / STORY`.md = 8；`reviews/.gitkeep`、`work/.gitkeep`；无其他文件 | **PASS** |
| 禁止文件名 | 无 HYPOTHESES / RUBRIC / CLAIMS / FINDINGS / PROGRESS / SURVEY / TASK_PLAN | `find -iname` 全仓库（排除 `.git`、`graphify-out`）零命中 | **PASS** |
| 已跟踪文件改动 | Wave B 不改既有 Skills / prompts / references / `.research/` | `git diff` 与 `--cached` 均空 | **PASS** |
| Frontmatter | 可解析，含 `name` / `description` | `yaml.safe_load` 成功；keys = `[name, description]`；description 长度 410 / 481 字符；`name` 与目录名一致 | **PASS** |
| 相对链接 | 全部可解析 | idea-evaluation 3 个、evidence-verification 5 个链接目标全部存在（`../../references/research-intelligence/*.md`、`../../references/experiment-record.md`、`../../references/git-linking.md`、`../../subagents/reviewer.md`） | **PASS** |

---

## 2. 八项检查（证据均为 文件 + 行号）

### C1. idea-evaluation 是否**只**是 Idea Gate？

**判定：PASS。**

- 触发集合与任务书逐项相同：`idea-evaluation/SKILL.md` L27–34 "Only: new Core Idea / route competition / mechanism replacement / major pivot / expensive experiment / high-stakes successor"；frontmatter L4–5 同一列表。
- 排除集合明确：L36–38 "Do **not** use for: routine seed, small ablation, bugfix, or simple replication. Cheap exploratory / sanity EXP stays on `experiment-design` → `experiment-execution` → `result-analysis` and must not auto-load this Skill."；frontmatter L7–8 重复排除。
- 流程第一步即门控：L73–74 "**Confirm trigger** — If the move is a routine seed, small ablation, bugfix, or simple replication, stop. Write nothing."；Deviation L114–115 "Stop with no file when the trigger does not match (protection: cheap exploratory EXP stays light)."
- 与 owner 一致：`idea-and-mechanism.md` L9–11 的加载条件 / 跳过条件语义相同（措辞差异见 N1）。

### C2. evidence-verification 是否**只**是 Evidence Gate，普通探索结果不强制？

**判定：PASS。**

- L29 "Prefer — not every EXP. This is an **Evidence Gate**, not a universal blocker."
- 触发列表 L31–37（Story Evidence 候选 / 高风险 result review / 主基线对比 / 意外强结果 / 机制主张 / Story 核心改动 / READY_FOR_WRITING 候选）是 `evidence-and-claim.md` L10–11 加载条件的超集，全部为高风险情形；用词是 "Prefer"，非 "must"。
- 排除：L39–42 "Do **not** use for: ordinary exploratory or sanity results (not mandatory), running code…, primary interpretation and Outcome writes (`result-analysis`), independent method/result critique (`experiment-review`), or idea-level mechanism identity (`idea-evaluation`)."；frontmatter L9–11 同。
- 流程第一步：L57 "Ordinary exploratory result → stop; do not run this gate."；Deviation L115 "Skip entirely for ordinary exploratory results."；L119–120 把轻量检查归还 `result-analysis`，与 `evidence-and-claim.md` L165–167（"light existence + this EXP / this commit check is enough; do not expand this list into a universal gate"）一致。

### C3. 是否新增 canonical 状态文件？

**判定：否。PASS。**

- 磁盘：`.research/` 仍恰好 8 文件；禁止文件名零命中（§1）。
- idea-evaluation：唯一输出 L48–50 `.research/work/idea-evaluation-<slug>.md`；L44 "does not write canonical science files and does not create an EXP"；L108 "Must **not** add canonical state files"；Complexity Budget 落在 work artifact，与 `idea-and-mechanism.md` L179–180 一致。
- evidence-verification：唯一输出 L51–53 `.research/work/EXP-xxx/evidence-verification.md`；L65 "Do **not** create `RUBRIC.md` or freeze criteria across EXPs"；L104–105 "Do **not** create `RUBRIC.md`, `CLAIMS.md`, or other canonical state files"，与 `evidence-and-claim.md` L8、L80–81 一致。
- 两者都不新增 `.agents/templates/`（git 零改动）。work 路径布局的一致性问题见 N4（非 canonical）。

### C4. 两个 Skill 是否都不写 STORY / DISCOVERY / EXPERIMENTS / Outcome / Story Evidence，由 Main 整合？

**判定：PASS。**

- idea-evaluation：frontmatter L6–7 "do not edit STORY, DISCOVERY, or EXPERIMENTS"；L43 "Main Agent integrates"；L88–89 "Do not assign EXP-IDs, rewrite Core Idea, or skip Reviewer contract"（与 `idea-and-mechanism.md` L231–233 逐义一致）；L90–91 "**Write and stop** — Only the work file. Hand off to Main."；Updates 表 L103–105 只有一行 work 文件；L107–110 "Must **not** write STORY / DISCOVERY / EXPERIMENTS… This Skill may run in-session or as a subagent; both write only `.research/work/`"（覆盖 STATE 等其余 canonical 文件，符合 `subagent-handoff.md` L38、`AGENTS.md` L69 的写权限）。
- evidence-verification：L49 "Output is a work artifact only"；Updates 表 L99–101 只有一行；L103–104 "Do **not** write `.research/STORY.md`, DISCOVERY, EXPERIMENTS Outcome/Index, or Reviewer files"；Forbidden L109–111 "Must **not** auto-change Outcome, auto-write Story, or auto-approve claims… or promote §F report labels into EXPERIMENTS as Outcome"；L84–85 "Main / Reviewer make the final scientific judgment"。frontmatter L4–5 "without treating a file as Story Evidence"。措辞可收紧处见 N5。

### C5. 是否重复 experiment-design / result-analysis / reviewer？是否 thin 且链到 Layer 2？是否抄 Outcome / Verdict 表？

**判定：PASS（thin、零表格复制、职责边界清晰）。**

- **Thin：** 两文件 120 / 122 行，与既有 Skill（102–135 行）同量级；idea-evaluation L15 "Judgment lives in Layer 2 — load, do not copy"；每个流程步骤只给 § 指针（L78–87：§A–B、§C、§G、§D、§E、§C–D、§H、§F），未复制 costume 表、fatal-flaw 表、rival 列表、controls 表。evidence-verification L17–25 同法：operators 指向 `evidence-and-claim.md`，Outcome / Verdict "cite only"。
- **零 Outcome / Verdict / Status / 报告标签复制（`rg`）：** 两文件中 `not-assessed | inconclusive | invalid | null | PROCEED | REJECT | INSUFFICIENT_EVIDENCE | ATTENTION_REQUIRED | planned | running | completed | superseded | abandoned` 作为枚举**零出现**（仅有的命中是普通英语 "running code" / "abandoned" 动词，L43、L91；evidence-verification L10、L40）；§F 六个报告标签、§G 措辞词零出现。与 `experiment-record.md` L77（"只在本文件定义…不要再抄这张表"）、`reviewer.md` L134–136（"complete list, only here"）及既有纪律（`experiment-review/SKILL.md` L17、`result-analysis/SKILL.md` L38）一致。
- **vs experiment-design：** idea-evaluation L44 "does not create an EXP"、L88–89 "Do not assign EXP-IDs"、L121–122 "After ADVANCE, Main may route to `experiment-design`; this Skill does not design or register the EXP"。8-gram 共享 = 0。
- **vs result-analysis：** evidence-verification L40–41 把 "primary interpretation and Outcome writes" 划给 `result-analysis`；L119–120 把轻量 this-EXP / this-commit 检查也划回 `result-analysis`（与 `evidence-and-claim.md` L165–167 一致）。8-gram 共享 = 3，全部是同一句链接样板 "experiment-record.md §Outcome 值"，非正文复制。
- **vs reviewer / experiment-review：** evidence-verification L41–42 排除 "independent method/result critique (`experiment-review`)"；L84–85 结束于 "Recommend `experiment-review` and/or Main integration"；L116–117 "still write the work artifact, not a review file"；L103–104 不写 Reviewer 文件；无 Verdict、无 independence header、无五段体。其 work artifact 落在 `.research/work/`，正好是 `reviewer.md` L94 规定 Reviewer 必须"当作待验证假设"的位置——从属关系正确。8-gram 共享 = 0。可再加一句"不计入 Reviewer 独立性"，见 N6。
- **vs Layer 2（"load, do not copy"）：** 10-gram 共享仅 2 个，均为 idea-evaluation L4–5 / L29–34 与 `idea-and-mechanism.md` L9–10 的触发词列表（必须一致的枢纽句），非算子复制。
- **vs 上游（idea-evaluator CC-BY-4.0、deep-research CC-BY-NC-SA、ARIS experiment-plan）：** 6-gram / 8-gram 共享 **均为 0**。两个 Skill 未复制任何上游文本；也未引入上游的五维打分、F1–F10、Strong Accept 等结构。

### C6. 是否迫使每个 exploratory EXP 走 Idea Gate + Evidence Gate + Reviewer？

**判定：否。PASS。**

- 两个 Skill 各自在四处写明轻量保护：idea-evaluation L36–38、L73–74、L114–115；evidence-verification L29、L39、L57、L115、L119–120。
- 无一条对所有 EXP 生效的 "must"：idea-evaluation 的 "must not" 全是写权限限制；"Required headings"（L52–66）只在 gate 触发后适用；evidence-verification L32 对 Reviewer 的关系是 "recommended before `experiment-review` result-review"，且限定于 "High-stakes result review"。
- 路由层未变：`AGENTS.md` L40–54 Skill Routing、`research-loop/SKILL.md` §4 路线表（L61–66）、`experiment-review/SKILL.md` L100（"Not every experiment needs review"）、`result-analysis/SKILL.md` L99（"Trivial exploratory runs: merge with execution"）均未被修改（git 零改动）。两个新 Skill 目前**不在**默认 `experiment-design → experiment-execution → result-analysis` 链上，只能由 Main 按触发条件显式加载。
- 与 Layer 2 保护语句一致：`scientific-reasoning.md` L218–219；`experiment-thinking.md` L337–338；`skill-evolution.md` L134–135、L143、L200–201（"forcing `idea-evaluation` on every EXP … **Reject.**"）。
- 限制：静态判断；`docs/validation/research-intelligence/cases/` 仍只有 `.gitkeep`，protection fixture 待后续 Wave 落盘。

### C7. Idea-gate 四动作是否只引用 `idea-and-mechanism.md` §H、未重定义？

**判定：PASS（一处可收紧的用法句，见 N2）。**

- L21–23 "Idea-gate `ADVANCE` | `REVISE` | `PARK` | `ABANDON` are defined **only** in idea-and-mechanism.md §H. Cite that section; do **not** recopy the four-line glossary. They are **not** Protocol, **not** Outcome, **not** Verdict."
- L68–69 "**Recommended Action** is exactly one of … per idea-and-mechanism.md §H."；L88 "One §H action."
- Gate A 验收正则（带 "—" 或 "if" 的释义行）在 `.agents/skills/ prompts/ subagents/ references/` 全域只命中 `idea-and-mechanism.md` L225–228（唯一 owner）与 `idea-evaluation/SKILL.md` L120 一行。L120 "Recommend REVISE and stop if identity or the minimum test is not yet honest." 是**用法**（何时提前收笔），条件词 "mechanism or test not yet honest" 与 §H L226 REVISE 释义相同、未分叉，不构成第二定义；但形式上是一句 "if" 型描述，建议加 "per §H" 消除歧义（N2）。
- 其余四动作出现（L88 "PARK or ABANDON is progress"、L121 "After ADVANCE"）与 `idea-and-mechanism.md` L235、L124 同为使用而非定义。evidence-verification 中四动作零出现。

### C8. evidence-verification 的 claim-calibration 是否只引用 `evidence-and-claim.md`、未重定义 Outcome？

**判定：PASS。**

- L20–21 "Outcome values: experiment-record.md §Outcome 值 — cite only; do not copy that table."；L24–25 "Claim-calibration words in evidence-and-claim.md are **report labels**, not Outcome."（与 `evidence-and-claim.md` L17、L213–217 一致）。
- L67–68 "Satisfaction using §F **report labels** (not Outcome)"；L110–111 禁止 "copy the Outcome table or Verdict list into the report, or promote §F report labels into EXPERIMENTS as Outcome"。
- 六个报告标签、七个 Outcome 值在 Skill 中**均未列出**（`rg` 零命中）；Skill 只给 § 指针。未出现第二张标签表或标签→Outcome 映射。

---

## 3. Verdict

**`APPROVE`**

理由：计数恰为 12 且无第三个新 Skill；符号链接正确；已跟踪文件零改动；两个 Skill 都是 trigger-only 的 gate，四处写明"普通 exploratory 不触发 / 不写文件"；输出只落 `.research/work/`，不写八个 canonical 文件、不创建 EXP、不写 Outcome / Story Evidence，由 Main 整合；四动作与报告标签均只引用 owner 段（`idea-and-mechanism.md` §H、`evidence-and-claim.md` §F），Outcome / Verdict 表零复制；与 experiment-design / result-analysis / reviewer 的边界逐条写明且 8-gram 零正文重复；上游零复制。以下 N 项均为措辞与一致性层面，可在 Wave C 或 V0.2 release audit 处理，不阻塞 Wave C 开始。

---

## 4. Blocking

无。

---

## 5. Non-blocking（建议在 Wave C 或 V0.2 release audit 处理；均为 Skill 文件内 1–3 行改动）

- **N1** 触发词措辞与 owner 微差：`idea-evaluation/SKILL.md` L33–34 "expensive experiment / high-stakes successor" vs `idea-and-mechanism.md` L10 "expensive successor / high-stakes method change"。语义同集合；任务书采用的是 Skill 的写法。建议二者择一统一（改 Skill 即可，不必动已 APPROVE 的 Layer 2）。
- **N2** `idea-evaluation/SKILL.md` L120 改为 "Recommend REVISE **per §H** and stop early when identity or the minimum test is not yet honest."，使 Gate A 的"定义型"正则在 Skills 目录零命中，四动作释义只留 §H 一处。
- **N3** `idea-evaluation/SKILL.md` Reads 表 L97–99 缺 `.research/RESOURCES.md`，而 §D 的 "Cost vs RESOURCES"（`idea-and-mechanism.md` L121）与 §F Complexity Budget 都需要它。建议加一行 "| If cost / expensive experiment | `.research/RESOURCES.md` |"，并在 L75–77 步骤 2 提及。
- **N4** work 路径布局：evidence-verification 用嵌套 `.research/work/EXP-xxx/evidence-verification.md`（L52、L101），idea-evaluation 用平铺 `.research/work/idea-evaluation-<slug>.md`（L49）；既有约定又有 `subagent-handoff.md` L15 `<task-slug>.md` 与 `adapters/codex.md` L20 `<role>/` 两种。均非 canonical，不阻塞；但建议 (a) 两个新 Skill 统一为一种（如 `.research/work/evidence-verification-EXP-xxx.md`），(b) evidence-verification 补一条与 idea-evaluation L116–117 对称的 "re-verifying the same EXP updates the existing file"，避免固定文件名在第二轮验证时被静默覆盖、或与 `.research/reviews/EXP-xxx/` 的 `r<N>` 轮次语义混淆。
- **N5** `evidence-verification/SKILL.md` L103 "EXPERIMENTS Outcome/Index" 比 idea-evaluation L107–108 的 "EXPERIMENTS" 窄；建议改为 "EXPERIMENTS (any field, including Outcome / Index / Review)"，并与 idea-evaluation L109–110 一样加一句 "in-session or as a subagent, writes only `.research/work/`"，把 STATE 等其余 canonical 文件一并覆盖。
- **N6** `evidence-verification/SKILL.md` 建议在 Forbidden（L107–111）加一句 "This artifact is not a review, carries no Verdict, and does not count toward Reviewer independence (`reviewer.md`)"，与 `reviewer.md` L53 对 synthesis 的规则同型，防止后续 Agent 把 verification 报告当作第二位 Reviewer。
- **N7** 分块标题两套：`evidence-verification/SKILL.md` L69–82 "with exactly: … Integrity status / Evidence match / Evidence gap" vs `evidence-and-claim.md` L278–290 "Integrity / Match / Gap"。owner 已声明 "not a new canonical schema"（L278）且 "executing Skill owns the work-artifact path"（L237–238），所以由 Skill 定标题合法；但两份差三个词的列表是可避免的漂移，建议 Skill 与 reference 用词对齐（改 Skill 三个词即可）。
- **N8** 可选：idea-evaluation frontmatter L8–9 触发词只有英文，其他 Skill（如 `experiment-design/SKILL.md` L9 "设计实验"）带中文触发词；可补 "评估想法 / 机制替换 / 换路线"。evidence-verification 亦可补 "核对证据 / 证据门"。

---

## 6. 给 Wave C（路由接线）的非阻塞提醒

本 gate 不评估 Wave C 计划，但两个 Skill 目前**只**能靠 frontmatter 被发现，以下现有文件在后续接线时需注意（本 Wave 未改、也不应由 Gate B 改）：

- `README.md` L54 "Canonical Skills（10）" 与列表将过时 → 12。
- `AGENTS.md` L40–54 Skill Routing 表若加两行，场景列必须写成 gate 语义（"评估新 Core Idea / 换路线 / 高代价实验 → `idea-evaluation`"、"结果拟进 Story Evidence / 高风险结果 → `evidence-verification`"），**不要**并入 `experiment-design → experiment-execution → result-analysis` 默认链，否则 C6 失效、触发 `skill-evolution.md` L200–201 的 Reject 条件。
- `research-loop/SKILL.md` §4 路线表（L61–66）同理：作为条件分支，不作为每次迭代的固定步骤。
- `subagent-handoff.md` L29–32 Role → files 表与 `adapters/codex.md` L20 的 work 路径约定，可借 N4 一并收敛。
- 提交卫生：`docs/validation/research-intelligence/gate-reviews/.gitkeep`（目录已有实文件）、`.agents/references/research-intelligence/.gitkeep`（已随 Wave A 提交，目录有六个实文件）、`graphify-out/`（未跟踪，无 `.gitignore`）——沿用 Gate A N9。

---

## 7. 本审核未覆盖

- 未运行任何行为 fixture（`cases/`、`prompt-regression/` 仍为空）；C1 / C2 / C6 为静态判断。
- 未评估两个 Skill 是否需要配套 `.agents/prompts/*.md`（Wave B 未提供，任务书未要求；如后续 Wave 增加 prompt，须沿用 "cite §H / §F, do not recopy" 纪律）。
- 未复核 Layer 2 六文件内容（Gate A / Gate A Re-review 已 APPROVE，本轮仅核对新 Skill 对其 § 引用的准确性：idea-evaluation 引用的 §A–H、`scientific-reasoning.md` §C–D 与 evidence-verification 引用的 §A、§B、§D–G 均与实际段落标题一致）。
- 未评估 Wave C–H 计划本身。
