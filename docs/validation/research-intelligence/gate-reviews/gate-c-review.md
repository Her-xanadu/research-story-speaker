# Gate C Review — V0.2 Wave C 任务 prompts（8 新增 + 4 升级）

- **角色：** 独立 Gate C Reviewer。相对 Wave C prompt 作者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘）。
- **审核时点：** 2026-09-04 04:40（UTC+8）
- **对象：** workspace `/Users/herxanadu/research-story-speaker`，分支 `v0.2-research-intelligence`，HEAD = `72eb338cee83a3213c30763a487e0696e1acf5f2`（"V0.2 Wave B: add idea-evaluation and evidence-verification skills"，Gate B `APPROVE` 后提交）。单一 worktree。
- **工作树状态：** 已跟踪文件改动仅 4 个 prompt（`git diff --stat HEAD -- .agents/prompts/` = 4 files, +119 / −12）：`experiment-review.md`、`method-review.md`、`result-review.md`、`subagent-handoff.md`。未跟踪新增 8 个 prompt：`idea-evaluation.md`、`experiment-proposal.md`、`result-diagnosis.md`、`failure-diagnosis.md`、`evidence-verification.md`、`next-research-move.md`、`story-audit.md`、`literature-synthesis.md`。`.agents/skills/`、`.agents/references/`、`.agents/subagents/`、`.agents/templates/`、`.research/`、`AGENTS.md`、`README.md`、`adapters/` **零改动**（`git status --short` 仅列 prompts 与 `graphify-out/`）。
- **只写：** 本文件。未 git commit；未启动 Wave D；未改 prompts / Skills / references。
- **Gate 判定词汇：** `APPROVE | TARGETED_REVISION` 由 Gate C 任务书规定，是框架层 gate 结论，**不是** `reviewer.md` §Verdict 词汇，也不是 Idea-gate 四动作，不新增任何 Protocol 枚举。
- **评分口径（任务书）：** 不按篇幅打分；只看 judgment operators 是否在场、是否一致、是否引用 owner 而非重抄。

---

## 0. 阅读范围与方法

**12 个 prompt 逐字读完**（行数 / mtime）：`idea-evaluation.md` 241 / 04:27、`experiment-proposal.md` 305 / 04:29、`result-diagnosis.md` 267 / 04:30、`failure-diagnosis.md` 269 / 04:30、`evidence-verification.md` 261 / 04:29、`next-research-move.md` 261 / 04:28、`story-audit.md` 212 / 04:30、`literature-synthesis.md` 222 / 04:30、`method-review.md` 99、`result-review.md` 94、`experiment-review.md` 81、`subagent-handoff.md` 88；四个升级件同时读 `git diff`。

**被引用的 owner 逐字读完：** Protocol `experiment-record.md`、`reviewer.md`、`state-files.md`、`story-loop.md`；Layer 2 六件（`idea-and-mechanism.md`、`experiment-thinking.md`、`scientific-reasoning.md`、`evidence-and-claim.md`、`deep-literature-mode.md`、`skill-evolution.md`）；subagent 角色文件 `research-lead.md`、`result-analyst.md`、`experiment-agent.md`、`literature-scout.md`；Skills `idea-evaluation`、`evidence-verification`、`experiment-design`、`experiment-execution`、`result-analysis`、`story-maintenance`、`literature-research`、`experiment-review`；`docs/design/research-intelligence-sources.md`；Gate A / A-re / B 三份前序审核。

**机械检查（脚本置于 `/tmp/gatec/`，未写入仓库）：** 12 个 prompt 全部相对链接 `os.path.exists`；`§X` 引用逐条映射到 owner 文件实际标题；`rg` 扫描 Outcome / Status / Verdict / Idea-gate 四动作 / §F 报告标签 / §G 措辞词的"表格行"与"词条 —"定义型出现；`find` 扫禁止文件名；prompt ↔ owner 及 prompt ↔ prompt 的词级 8-gram 共享计数与 ≥12 词连续逐字片段定位（行号级）。

---

## 1. Protocol 冻结确认（任务书要求项）

| 项 | 要求 | 实测 | 判定 |
|----|------|------|------|
| `reviewer.md` 五标题与 Verdict 契约 | 未变 | `git diff HEAD -- .agents/subagents/reviewer.md` 为空；该文件最后一次改动为 `9f8a315`（Gate A candidate，早于 Wave A/B/C）。L134–136 Verdict 五值、L141–159 五个必需标题原样。`method-review.md` L83–90 与 `result-review.md` L78–85 仍写"Header, then `## Verdict`, then the five sections in reviewer.md"，五个标题名与 reviewer.md L144–158 逐字一致 | **PASS** |
| `experiment-record.md` / `state-files.md` / `story-loop.md` | 未变 | `git diff HEAD` 为空 | **PASS** |
| prompts 引用而非重抄 Outcome / Verdict 表 | 零表格复制 | 定义型正则（`| token | …|` 行、`TOKEN — …` 词条行）在 `.agents/prompts/` **零命中**。Outcome / Status 令牌只出现在"应用句"中并紧跟 §引用：`result-diagnosis.md` L146–158（引 §Outcome 值）、`failure-diagnosis.md` L109、L167、L184、L242–243、`result-review.md` L94（Wave C 之前已存在，引 §Outcome 值）。Verdict 五值中只有 `REVISE` 出现于 `method-review.md` L56，且 L57–58 显式区分"Reviewer contract, not idea-and-mechanism.md §H"；`PROCEED / REJECT / INSUFFICIENT_EVIDENCE / ATTENTION_REQUIRED` 在 prompts 中零出现。`evidence-and-claim.md` §F 六个报告标签、`scientific-reasoning.md` §G 措辞词作为令牌零出现 | **PASS** |
| Idea-gate 四动作 | 只引 §H，不重抄四行 glossary | `TOKEN —` 型行零命中；`idea-evaluation.md` L6–10、L214–215 明确"do not recopy the four-line glossary / not Protocol, not Outcome, not Verdict"；`story-audit.md` L160–162、`literature-synthesis.md` L136–139 只引 §H。决策逻辑段的释义回声见 N1（非阻塞） | **PASS** |
| 相对链接 | 全部可解析 | 125 / 125 | **PASS** |
| `§` 引用 | 均对应 owner 真实标题 | idea-and-mechanism §A–H、experiment-thinking §A–H、scientific-reasoning §A–G、evidence-and-claim §A–G、deep-literature-mode §A–F、story-loop §Gap 优先级 / §反重复 / §停滞处理、experiment-record §Status 值 / §Outcome 值、reviewer §Verdict、state-files §更新顺序 全部存在（脚本 5 个"未找到"均为正则伪命中：`§Verdict\`` 尾反引号、`§Gap 优先级` 被截为 `§G`） | **PASS** |
| 禁止文件名 / `.research/` | 无新增 canonical 文件 | `HYPOTHESES / RUBRIC / CLAIMS / FINDINGS / PROGRESS / SURVEY / TASK_PLAN` 零命中；`.research/` 仍恰好 8 文件，`work/`、`reviews/` 为空 | **PASS** |
| 新 prompt 的接线 | Wave C 不改 Skills / 路由 | 8 个新 prompt 在 `.agents/prompts/` 之外**零引用**（`rg` 排除 prompts 目录后仅 `gate-b-review.md` 提及）；唯一入口是 `subagent-handoff.md` L41–47 的角色表与 `experiment-review.md` L40、L47–59 | 符合范围；接线留给 Wave D（见 §6） |

---

## 2. 十问（证据均为 文件 + 行号）

### Q1. 它们会不会只产出模糊的"还需要更多实验"？

**否。PASS。** 每个会给出"下一步"的 prompt 都把"再跑更多"明确列为不合格答案，并要求一个可改变判断的具体动作：

- `result-diagnosis.md` L169–174（§10 "'Run more experiments' is not an action. Parameter sweeps are not a discriminating action unless the Question is itself a threshold"）；L242–243 输出要求"one action that could change judgment"。
- `next-research-move.md` L143–153（只选**一个**动作 + "why now must beat the alternatives at this moment"；证据不足时"the cheapest discriminating step"）；L175–184 反模式列表含"50-seed leaderboard that cannot split rivals"。
- `failure-diagnosis.md` L111–124（**exactly one** action，八选一）；L235–236 "why not a sweep — required even if obvious"。
- `evidence-verification.md` L160–165（Required action 限定为 "verify, rerun, method-review, narrow the claim, or refuse Story Evidence"）。
- `idea-evaluation.md` L144–152、L232–233（返回必须含 minimum test）。
- `method-review.md` L90 / `result-review.md` L85 的 "recommended next move" 给出具体选项集，owner `reviewer.md` L157–158 本就禁止 "vague more work"。

### Q2. 能否点名**最关键**的替代解释？

**能。PASS。** 各 prompt 使用同一个 operator："best rival → 能削弱它的 control / split"，且一致拒绝十项清单式背诵：

- `idea-evaluation.md` L116–123（"the **best** rival a skeptical colleague would actually offer — not a mandatory ten-item chant"；"If no control or split would make that rival less tenable, the mechanism claim is not ready"）。
- `experiment-proposal.md` L85–89、L113–118（"Name one primary rival; mention others only if they could actually produce this pattern"）。
- `result-diagnosis.md` L129–135（"single best rival … State what control or split would make that rival less tenable"）；L243（"the best rival, not a list of ten"）。
- `result-review.md` L67；`method-review.md` L78；`story-audit.md` Q4 L88–92；`evidence-verification.md` L119–121 把 "result review rival still standing" 作为 criterion 来源。

### Q3. 能否点名**缺哪个 control**？

**能。PASS。** 不是"加更多对照"，而是从 rival 反推缺失的那一个：

- `experiment-proposal.md` L152–166（rival → control 映射；"List only the controls this Question needs"）；L250–251（"Mechanism claim without mechanism-off or information-matched control → revise method before any run"）。
- `method-review.md` L49–58 Attribution gate（"missing isolating control, unfair baseline, information or compute asymmetry, leaked unit, or a costume"）；L69 Control sufficiency。
- `result-diagnosis.md` L105–111（"Name the missing match"）、L113–121（无隔离设计时 "recommend a discriminating control as the next action"）。
- `evidence-verification.md` L160–163（Gap 必须写明 "control, unit, replication, commit bind, prediction that was never written"）、L171–172。
- `failure-diagnosis.md` L90–92 把 "missing control that was required to isolate anything" 归为 experimental invalidity，而非"想法死了"。

### Q4. 能否叫停低价值实验（must / nice / cut）？

**能。PASS。**

- `experiment-proposal.md` L185–204 三个**独立**标题 Must-run / Nice-to-have / Cut；"**Cut is a success.** Cut items do not run"；L213–217 stop condition；L233 "All eight controls on a sanity probe (process failure, not rigor)"。
- `failure-diagnosis.md` L153–162 HARD RULE（见到失败不得直接开 sweep）；L140–143 `park` / `close`。
- `next-research-move.md` L139–140（"If the action would not resolve any named uncertainty, drop it"）；L155–159 停滞信号 → "do not recommend 'one more similar config'"。
- `idea-evaluation.md` L202（"PARK or ABANDON is progress"）；`story-audit.md` L211–212（"stopping the large rewrite is the successful audit"）；`method-review.md` L76（"smallest comparison … or a decorative grid?"）；`literature-synthesis.md` L143–144、L183–184。

### Q5. 是否区分 execution failure 与 hypothesis failure？

**是。PASS（这是本轮最强的一组 operator）。**

- `failure-diagnosis.md` 整体即为此设计：L17–22（"Engineering failure is **not** a scientific negative"）；L66–109 九个诊断类把 engineering / environment / measurement 与 hypothesis contradiction / mechanism failure 分开，optimization failure（L100–103）保留"可能是 bug 也可能是 confounder，先 budget-matched 重跑再判死"的细分；L108–109 "Inconclusive is not `contradicts`"；L137–139 "Do not pivot from a crash"；L178–198 决策逻辑把科学类交给 `result-diagnosis.md`。
- `result-diagnosis.md` L86–88（"A broken measurement does **not** prove the hypothesis false. It is also not Story Evidence"）；L145–159 三条硬映射（technical → `failed` + `not-assessed`、无 Negative Discovery；completed-but-unusable → `invalid`；previously trusted → Invalidated Findings）；L158–159（"A valid `contradicts` or `null` on a `completed` EXP is a scientific finding. A failed job is not."）。
- `result-review.md` L60、L94；`evidence-verification.md` L112–116。
- 与 owner 一致：`experiment-record.md` L75、`scientific-reasoning.md` L256（"After a failure: did engineering fail, or did the hypothesis fail?"），prompts 把这句习惯做成了可执行的分类。九类词表的 ownership 问题见 N2。

### Q6. 能否识别 cosmetic innovation？

**能。PASS。**

- `idea-evaluation.md` L79–92（deletion test；"Parameter tweaks and renames are not deletion"；六种 costume；"If the only honest difference is a name, a knob, or stacked unchanged parts, do not ADVANCE"）；L193–194。
- `method-review.md` L67（Mechanism identity 镜头）、L77（Unnecessary complexity / deletion test）。
- `story-audit.md` Q7 L102–106（数 Core Idea 里的独立 gadget；"Excluded is mandatory"）、L130–133 反模式。
- `experiment-proposal.md` L235–237（"Weight 1.0 → 0.9 called 'mechanism-off'"、"Parameter sweep sold as ablation"）；L300–301（"Idea identity still a costume → send back to idea-evaluation; do not launder it into an EXP"）。
- `literature-synthesis.md` L71–73、L215–216（"do not invent a cosmetic difference"）；`result-review.md` L66（"Full beat a weak default"）。

### Q7. 能否控制 Story scope（story-audit）？

**能。PASS。**

- `story-audit.md` L19–22（story creep / mechanism creep / post-hoc / claim expansion；"Keep **one** dominant contribution"）；L67（"Story must not get more complex **in order to save a method**"）；Q5 L93–97（Boundary 是否被沉默冒充为 generality）；Q8 L107–111（事后解释只能进**新** EXP 的 Question）；Q9 L112–115；L140–144（Q2/4/5 的 "yes" 必须带 `EXP-xxx` 或 DISCOVERY 行指针）；L146–158 决策逻辑（"cut extras; do not grow Story to rescue the method"；"hold for Reviewer"）；L206–207 证据有争议 → hold。
- Main-writes-STORY 契约保留（L17、L193–204），subagent 不得写 STORY（L138）。
- 配套：`result-diagnosis.md` L161–167（"Do not inflate dataset A + seed B + condition C into 'robust / general'"）；`evidence-verification.md` L155–158；`result-review.md` L70；`next-research-move.md` L129–133（"Do not draft replacement Story prose"）。

### Q8. 能否提出**最小判别实验**？

**能。PASS。**

- `idea-evaluation.md` L144–152 §9（mechanism-off / information-matched，在 §B 的 failure condition 上、在正确的 experimental unit；"A 50-seed leaderboard that cannot split rivals is not 'more decisive'"；"Do **not** assign EXP-IDs here"）。
- `experiment-proposal.md` L144–149 Primary Comparison、L189–191 Must-run 拥有预算。
- `result-diagnosis.md` L169–174 §10；`method-review.md` L76；`next-research-move.md` L150–153（"minimum decisive test before a grid"）；`literature-synthesis.md` L100–102、L183–184；`failure-diagnosis.md` L211（"targeted validation (the must-run comparison, not a grid)"）。
- `story-audit.md` 按设计不自己出实验，L160–162 转 `idea-evaluation` —— 职责边界正确。

### Q9. prompts 之间 / prompts 与 Layer 2 是否大量重复规则？（应引用 owner）

**Protocol 表零复制（PASS，见 §1）；但 Layer 2 operator 在 3 个新 prompt 中被大段逐字重抄（FAIL → B1）。**

词级 8-gram 共享（分母 = 该 prompt 的 8-gram 数；对照集 = Layer 2 六件 + Protocol 四件）：

| prompt | 8-gram 总数 | 与 owner 共享 | 占比 | 主要来源 |
|--------|------------|--------------|------|----------|
| `evidence-verification.md` | 1484 | 173 | **11.7%** | `evidence-and-claim.md` 173 |
| `idea-evaluation.md` | 1652 | 171 | **10.4%** | `idea-and-mechanism.md` 163 |
| `experiment-proposal.md` | 1753 | 146 | **8.3%** | `experiment-thinking.md` 140 |
| `literature-synthesis.md` | 1106 | 66 | 6.0% | `deep-literature-mode.md` 66 |
| 其余 8 个 prompt | — | 0–11 | ≤ 1.0% | — |

对照：Gate B 两个新 Skill 与 Layer 2 的 10-gram 共享仅 2 个（触发词枢纽句）。

≥12 词**连续逐字**片段（行号级定位；均为 operator 正文，不是链接样板）：

- `evidence-verification.md` ← `evidence-and-claim.md`：11 段 / 231 词。L70–73←L57–60（四来源）、L86–100←L25–40（四层不等式与整段解释）、L102–105←L263–265 与 L250–253（claim-kind 纪律、scope）、L112–115←L152–160（integrity 停止句、crash 非 negative）、L129–130←L129–130、L140–142←L162–163、L187–190←L44–47（collapse 列表）。
- `idea-evaluation.md` ← `idea-and-mechanism.md`：9 段 / 205 词。L64–66←L36–38（§A anchor 定义）、L75–76←L64–65、L81–85←L75–88（§C 九轴 + identity 定义）、L109–113←L152–155（§E novelty threat 整句）、L139–141←L175–177（§F 37 词逐字）、L147–152←L213–219（§H 最小实验整段）。
- `experiment-proposal.md` ← `experiment-thinking.md`：9 段 / 178 词。L75–77←L37–38、L81–83←L63–64、L88–89←L167–169、L127–129←L68–70、L174–175←L146–147、L177–180←L189–192、L202–204←L256–258、L213–218←L324–327（§H stop condition 37 词逐字）。此外 L157–164 逐条重抄 §D L172–179 的 rival→control 映射、L166 逐字 §D L185、L170–172 重抄 §C 十项 confounder 列表 —— 而 L154 同段正写着 "cite; do not recopy the eight-row taxonomy as a second owner"。
- `literature-synthesis.md` ← `deep-literature-mode.md`：4 段 / 74 词（L28–29、L33–34、L107–109、L119–121），量级可接受，B1 顺带清理即可。

**为什么算问题而不是"篇幅"：** 三个 prompt 自己声明 "load, do not copy"（`idea-evaluation.md` L38–39、L97、L214）、"cite; do not recopy"（`experiment-proposal.md` L97–98、L154、L180）、"Cite §A; do not treat this prompt as a second owner"（`evidence-verification.md` L83–84），紧接着就逐字抄 owner。这违反 `skill-evolution.md` L95–96（"Prefer a link … over pasting a new essay … Skills stay thin; this layer stays detailed"）与 L92–93（operator 只在 Layer 2 修）：将来 Layer 2 一处 operator 修正，三个 prompt 里的复本会静默过期。同时若 subagent 已按 prompt 指令加载 Layer 2，这些复本是纯 token 重复。

**prompt ↔ prompt：** `method-review.md ~ result-review.md` 90 个共享 8-gram，全部是 Reviewer 样板（Task fields、header、五段），可接受。"technical failure → `failed` + `not-assessed` → 无 Negative Discovery"这条规则在 4 个 prompt（`result-diagnosis.md` L146–148、`failure-diagnosis.md` L184 / L242–243、`result-review.md` L94、`evidence-verification.md` L112–116）+ 2 个 Skill + 2 个 owner 中各复述一遍，每处都引 owner，形式合规，但漂移面偏大（N9）。

### Q10. 是否把简单任务做重？（渐进披露；普通 exploratory 不强制过全部 gate）

**总体否。PASS，三处软点（N3 / N4 / N8）。**

- 两个 gate prompt 都有"触发不符 → 停止、不写文件"：`idea-evaluation.md` L31–34、L189；`evidence-verification.md` L44–47、L207。
- 其余 prompt 各有轻量保护：`experiment-proposal.md` L48–51（"Do not fire idea-evaluation or evidence-verification because this file exists"）；`result-diagnosis.md` L34–37、L73–74（可写 `n/a — <why>`）、L266–267；`failure-diagnosis.md` L62（"Do not load idea-evaluation or deep literature to debug a crash"）；`next-research-move.md` L57–58、L171–173、L184（"Firing every intelligence gate on a sanity rerun (protection case)"）；`story-audit.md` L32–33；`literature-synthesis.md` L3–6、L32–34；`method-review.md` L29–31 / `result-review.md` L31–33（Optional Layer 2，sanity rerun 跳过）；`experiment-review.md` L53–55（"This is **not** mandatory … Do not block every review on that Skill"）；`subagent-handoff.md` L38–39、L49–53（"Do not dump all intelligence files into the reviewer context"）。
- 没有任何 prompt 对"所有 EXP"生效的 must；`AGENTS.md`、`research-loop`、`experiment-design → experiment-execution → result-analysis` 默认链未被修改，且 8 个新 prompt 尚未被任何 Skill 引用（§1 末行），因此**当前**不可能让默认链变重；这是静态判断，行为 fixture 仍为空（`cases/`、`prompt-regression/` 仅 `.gitkeep`）。
- 软点：(a) `subagent-handoff.md` L44 把 `literature-synthesis.md`（自称 "Deep, optional, expensive"，L3–6）无条件列为 literature-scout 的 Task prompt，而同行 Layer 2 列写的是 "**when requested**" —— Main 可能给一次轻量找基线的 scout 也挂上深模式 prompt（N3）；(b) `experiment-proposal.md` L53–55 触发条件含笼统的 "New EXP"，L95–98 要求 14 个标题"全部填写、不得空标签"，却没有 `result-diagnosis.md` 那样的 `n/a` 规则给显式 exploratory EXP（N4）；(c) `failure-diagnosis.md` L235–236 对路径拼错这类纯工程失败也要一段 "why not a sweep"（N8）。

### 补充发现（不在十问内，但影响交付物是否可用）：输出结构冲突

四个角色的**角色文件**各自规定了 `Required output` 结构，四个新任务 prompt 又规定了**不同**结构，而两者的优先级在任何地方都没有声明：

| 角色文件（`.agents/subagents/`） | 角色文件的输出标题 | 新 prompt 的输出标题 |
|---|---|---|
| `research-lead.md` L57–70（"Use this structure exactly"；L72 "under ~80 lines"） | current bottleneck / candidate next actions / recommended action / **reasoning summary** | `next-research-move.md` L214–232：前三个相同，第四个改为 **why now**；L212 却自称 "superset of research-lead.md's four return sections" |
| `result-analyst.md` L70–85 | supported interpretation / alternative explanations / discovery impact / story impact / next experiment（5） | `result-diagnosis.md` L224–236：`## 1 Integrity` … `## 10 Next discriminating action`（10，完全不同） |
| `experiment-agent.md` L70–85 | what was done / git commit / result location / raw findings / issues（5） | `failure-diagnosis.md` L220–244：failure class / why this class / scientific contract / recommended action / why not a sweep / bounded debug plan / EXPERIMENTS hint（7）；`experiment-proposal.md` L94–217：14 个设计标题 + L269–273 四个尾字段 |
| `literature-scout.md` L63–83 | story gap addressed / sources found / key claims and evidence / gaps remaining / relation to our story / suggested next literature actions（6） | `literature-synthesis.md` L149–196：Research Questions … Experiment implications + citation depths + still have not read（14） |

只有 Reviewer 一条线有显式规则（`reviewer.md` L161–162 "Task prompts keep method/result fill-in questions but must use these headings"）。`subagent-handoff.md` L36–38（Wave C 新增）只说 "Load `.agents/subagents/<role>.md` first, then **this dispatch's** task prompt"，没有说输出结构谁赢。结果：被派出的 result-analyst 同时收到两套 Required output；若角色文件赢，四个新 prompt 的输出契约成为死文字；若 prompt 赢，`research-lead.md` 的 "exactly" 被违反。这使 Wave C 四个角色 prompt 的输出契约不可预测（B2）。角色文件不在 Wave C 范围，但 `subagent-handoff.md` 在范围内且正是角色→prompt 的映射处，一两句优先级规则即可修复。

---

## 3. Verdict

**`TARGETED_REVISION`**

理由：任务书聚焦的 judgment operators（Q1–Q8）**全部在场且跨 prompt 一致** —— 同一条链路 "best rival → 能削弱它的 control → 最小判别比较 → must / nice / cut → execution 与 hypothesis 分开 → costume 识别 → Story scope 收紧" 在 12 个文件里没有互相矛盾的说法；Protocol 表零复制、`reviewer.md` 契约未动、链接与 §引用全部可解析、无新 canonical 文件、默认链未被加重。阻塞项不在判断力，而在三处**可在 Wave C 范围内（只改 prompts + `subagent-handoff.md`）修完**的卫生 / 契约问题：B1 三个 prompt 逐字重抄 Layer 2 operator（直接对应 Q9，且与 prompt 自身的 "cite, do not recopy" 声明矛盾）；B2 四个角色 prompt 的输出结构与角色文件冲突且无优先级规则；B3 一个伪 Status 令牌。修完后预期 `APPROVE`，不需要动 Layer 2、Skills、角色文件或 Protocol。

---

## 4. Blocking（Wave D 开始前必须完成；均限于 Wave C 交付文件）

### B1. 去除对 Layer 2 owner 的逐字重抄（`evidence-verification.md`、`idea-evaluation.md`、`experiment-proposal.md`；`literature-synthesis.md` 顺带）

- **改什么：** §2 Q9 列出的行段。保留每个镜头的**名称 + § 指针 + 本 prompt 特有的"在此如何应用"句**（例如 `result-diagnosis.md` 的写法：镜头名 + 一句本地应用 + §引用，8-gram 共享 0.8%），删除与 owner 逐字相同的定义句、列表与整段解释。`experiment-proposal.md` L157–166 的 rival→control 映射与 L170–172 的 confounder 列表改为 "pick from the rival per experiment-thinking.md §D / §C" 一句。
- **不是什么：** 不是缩短要求。允许 prompt 更长，只要新增的是 prompt 自己的判断句而不是 owner 的复本。
- **验收：** 三个 prompt 与其引用 owner 之间 ≥12 词连续逐字片段 = 0（链接 / Task fields 样板除外）；8-gram 共享占比降到其余 prompt 的水平（≤ 约 2%）。复审时用同一脚本口径。

### B2. 声明任务 prompt 与角色文件的输出结构优先级（`subagent-handoff.md`，可选地在四个角色 prompt 各加一句）

- **改什么：** 在 `subagent-handoff.md` "Layer 2 / task prompt by role"（L35–53）加一两句，例如："When a task prompt is attached in the handoff, its `Required output` headings **replace** the role file's default output structure; the role file's Read-first / Do-not / quality-bar rules still apply. Without a task prompt, the role file's structure stands." 同时把 `next-research-move.md` L212 的 "superset" 表述改为准确说明（第四段改回 `## reasoning summary`，或写明 `## why now` 替代之）。
- **为什么在范围内：** 角色文件不在 Wave C 范围；但优先级归属映射处，`subagent-handoff.md` 是 Wave C 升级件。角色文件本身的后续对齐留给 Wave D（§6）。
- **验收：** 任一 dispatched result-analyst / research-lead / experiment-agent / literature-scout 在"角色文件 + 任务 prompt"下只有一套明确的 Required output。

### B3. `failure-diagnosis.md` 中的伪 Status 令牌 `blocked`（L130、L263）

- **问题：** `experiment-record.md` §Status 值只有 `planned | running | completed | failed | abandoned | superseded`。L130 "then `blocked` with a reason"、L263 "record `blocked` and stop" 用反引号呈现且紧邻 Status 讨论（L184、L242–243），Agent 可能写出 `Status: blocked`，形成事实上的第七个 Status。
- **改什么：** 去掉反引号，改为普通语义，例如 "record the blocker (Main writes it to STATE blockers; `experiment-agent.md` quality bar) and stop"。两行改动。

---

## 5. Non-blocking（建议随 B1–B3 一并处理，或留给 Wave D / V0.2 release audit）

- **N1** `idea-evaluation.md` L189–200 决策逻辑在路由之外回声了 §H 四行 glossary 的定义文字：L200 "Identity clear, no fatal flaw, minimum test specified → ADVANCE" ≈ owner L225；L193 "(this mechanism cannot be the answer)" ≈ L228；L198 "(sequencing / budget)" ≈ L227。保留"镜头结果 → 动作"的路由，删掉括号里的释义，让"每个动作是什么意思"只留在 §H 一处（Gate B N2 同型）。
- **N2** `failure-diagnosis.md` 的 9 个诊断类（L72–82）+ 8 个动作串（L116–124）目前只有这个 Layer 3 prompt 拥有定义；Layer 2 只有种子句（`scientific-reasoning.md` L256），且 `result-diagnosis.md` L210 已经反向依赖它。建议二选一：在文件内加一句 "this prompt is the owner of these diagnosis classes / move strings"；或在后续一次原子改动中把类表提升为 `experiment-thinking.md` 新节。另外小写 `park` / `close` / `pivot` 与 Idea-gate `PARK`、Status `abandoned` 语义相邻，L67–68、L113–114 已声明"not Status / Outcome / Verdict"，再加 "not Idea-gate actions" 四个词即可。
- **N3** `subagent-handoff.md` L44 literature-scout 的 Task prompt 列改为 `literature-synthesis.md` **(deep mode only: novelty / conflict / convention / new Core Idea)**；L45 experiment-agent 列注明 `experiment-proposal.md` (EXP-ID: NEW design only)、`failure-diagnosis.md` (on failure only)。可与 B2 同次改动。
- **N4** `experiment-proposal.md` L53–55 "New EXP" 收窄为 "New EXP that claims a mechanism or spends serious compute"（对齐 `experiment-thinking.md` L10–12），并在 L95–98 加一句：显式 exploratory 的 EXP 可在 Rival / Matrix / Controls / Confounders 写 `n/a — exploratory, <why>`（对齐 `result-diagnosis.md` L73–74 的做法）。
- **N5** `method-review.md` L55–56 "the method Verdict is `REVISE` even before results" 预先替 Reviewer 定了 Verdict；改为 "cannot be `PROCEED`; default `REVISE`, or `REJECT` if fatal — per reviewer.md §Verdict"，与 L98–99 "If method is fatally flawed, say so explicitly" 一致。
- **N6** `evidence-verification.md` L117–165 七个教学小节名（Criterion source / Required evidence / Actual artifact / Existence–validity / Match / Claim scope / Gap and required action）是继 Skill 十标题（L227–229）与 `evidence-and-claim.md` L280–291 之后的第三套命名（Gate B N7 延伸）。要么用词对齐 Skill，要么加一句"these are lenses, not headings; output headings are exactly the Skill's"。
- **N7** `research-lead.md` L72 "Keep total response under ~80 lines" 与 `next-research-move.md` 每候选五字段 × 3–5 候选的工作文件：prompt L234–236 已把返回与工作文件分开，B2 落地后再核一遍即可。
- **N8** `failure-diagnosis.md` L235–236 允许 engineering / environment 类用一行 "why not a sweep"，而不是"一段、即使显然"。
- **N9** "technical failure → `failed` + `not-assessed` → 无 Negative Discovery"在 4 个 prompt 中各复述一遍（Q9 末段）。B1 时可把 `evidence-verification.md` L112–116 与 `failure-diagnosis.md` L184 / L242–243 收成一句指针，保留 `result-diagnosis.md` L145–159 作为 prompt 侧唯一完整应用（它是 Outcome candidate 的直接使用者）。
- **N10** `literature-synthesis.md` L83–88 重抄四个 depth-label 字串属可接受（与写 Outcome 令牌同性质，且未抄 §E 映射表）；保持现状即可，勿再加映射。
- **N11** Gate B 的 Skill 级 N1–N8 本轮未处理（Skills 不在范围）。三处已在 prompt 侧被部分补偿：`idea-evaluation.md` L28 列了 RESOURCES（B-N3）；`evidence-verification.md` L233–235 "re-verifying … update this file"（B-N4b）；`experiment-review.md` L58–59 与 `evidence-verification.md` L244–245 "not a Reviewer Verdict"（B-N6 部分；仍未明说"不计入 Reviewer 独立性"）。Wave D 仍应回到 Skill 文件修正。
- **N12** 提交卫生沿用 Gate A N9 / Gate B §6：`graphify-out/` 未跟踪且无 `.gitignore`；`gate-reviews/.gitkeep` 与实文件并存。

---

## 6. 给 Wave D（路由接线）的非阻塞提醒

本 gate 不评估 Wave D 计划，只记录接线时会碰到的既有事实：

- 8 个新 prompt 目前在 Skills / `AGENTS.md` / `README.md` / 角色文件中**零引用**；`idea-evaluation` 与 `evidence-verification` 两个 Skill 也未指向各自的 prompt（prompt 侧已单向声明 "Hand this prompt with the … Skill"）。接线时必须保持 gate 的 trigger-only 语义：不要把 `idea-evaluation.md` / `evidence-verification.md` 并入 `experiment-design → experiment-execution → result-analysis` 默认链（Gate B §6 与 `skill-evolution.md` L198–201 的 Reject 条件）。
- B2 的优先级规则最终应同步进四个角色文件（`research-lead.md` L57 "exactly"、`result-analyst.md` L70、`experiment-agent.md` L70、`literature-scout.md` L63），并把 `research-lead.md` L61–63 候选行格式（"— est. cost/info"）与 prompt 的五个定性字段对齐；这属于角色文件改动，超出 Wave C。
- `README.md` "Canonical Skills（10）" → 12（Gate B §6 延续）。
- 若 Wave D 落 protection fixture，本轮 Q10 的三处软点（N3 / N4 / N8）正是应写成 protection case 的场景："light literature-scout 不挂 deep prompt"、"显式 exploratory EXP 不填 14 个设计标题"、"import error 不写 sweep 论证段"。

---

## 7. 本审核未覆盖

- 未运行任何行为 fixture（`cases/`、`prompt-regression/` 为空）；Q1–Q10 均为静态判读。
- 未复核 Layer 2 六件与两个新 Skill 的内容本身（Gate A / A-re / B 已 `APPROVE`）；本轮只核对 prompts 对它们的 § 引用准确性与重抄程度。
- n-gram 统计为词级、去标点、去链接目标的近似口径，用于定位而非精确计量；B1 复审建议沿用同一脚本口径以便前后可比。
- 未评估 Wave D–H 计划。
