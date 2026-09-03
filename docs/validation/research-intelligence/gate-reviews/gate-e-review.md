# Gate E Review — V0.2 Research Intelligence 发版前独立审核

- **角色：** 独立 Gate E Reviewer。相对 V0.2 全部 Wave（A–H）开发者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘 + git 历史）。
- **审核时点：** 2026-09-04 05:50（UTC+8）
- **对象：** workspace `/Users/herxanadu/research-story-speaker`，分支 `v0.2-research-intelligence`，HEAD = `32917595753ecc33fa5f8794e197c9eba10eeb5e`（"V0.2 Wave H: skill-evolution dogfood"）。单一 worktree。
- **工作树状态：** `git status --short` 仅两项未跟踪：`docs/validation/research-intelligence/gate-reviews/v0.2-maintenance-audit.md`（开发侧 `framework-maintenance` standard 审计，05:42 写出，见 §3 E-N5）与 `graphify-out/`（工具残留，Gate A N9 沿用）。**无任何已跟踪文件被修改。**
- **只写：** 本文件。未 git commit / tag / push；未改 Skills、prompts、references、subagents、templates、`.research/`、`AGENTS.md`、`README.md`、adapters。
- **Gate 判定词汇：** `APPROVE_V0_2 | TARGETED_REVISION | MAJOR_REVISION` 由 Gate E 任务书规定，是框架层 gate 结论，**不是** `reviewer.md` §Verdict 词汇，不是 Idea-gate 四动作，不是 `evidence-and-claim.md` §F 报告标签，不新增任何 Protocol 枚举。
- **评分口径：** 不按篇幅打分。区分三件事：(i) **架构**是否守住 FROZEN CORE 与选择性门；(ii) **指令层**是否真的加入了 judgment operators；(iii) **行为证据**是否支持「更会做科学判断」这一 V0.2 头条主张。三者分开给结论，不互相借光。

---

## 0. 阅读范围与方法

**逐字读完：** Layer 2 六件（`scientific-reasoning.md`、`idea-and-mechanism.md`、`experiment-thinking.md`、`evidence-and-claim.md`、`deep-literature-mode.md`、`skill-evolution.md`，合计 1,732 行）；两个新 Skill；`research-loop`、`result-analysis`、`experiment-design`、`experiment-execution`、`framework-maintenance`、`workspace-resume` SKILL；`reviewer.md`、`research-lead.md`、`literature-scout.md`；prompts `subagent-handoff.md`、`next-research-move.md`、`failure-diagnosis.md`、`result-diagnosis.md`、`idea-evaluation.md`、`evidence-verification.md`；`docs/design/research-intelligence-sources.md`；`BASELINE.md`；Gate A/A′/B/C/C′ 五份审核；`gate-d.md`；Wave G 三份报告；Wave H `REPORT.md`；Case 01 / 03 / 10 README；`v0.2-maintenance-audit.md`（未跟踪）；`/tmp/rss-wave-g-prompts/test1-idea-evaluation.txt`、`test2-evidence-verification.txt`（Wave G 实际喂给 agent 的 prompt 原文，仅存于 `/tmp`）。

**按标题 + 关键段落抽读：** `experiment-proposal.md`、`story-audit.md`、`literature-synthesis.md`、`method-review.md`、`result-review.md`、`experiment-review.md`、其余 Skill / subagent 的 Wave D/E diff（`git diff v0.1.1 HEAD`）。

**机械核对（全部用 `git` / `rg` / `find` 直接跑，不依赖开发侧报告）：** tag 指向、根 `.research/` 与 `v0.1.1` 的 diff、Protocol 四件与八模板的 byte-diff、`reviewer.md` 增量、目录计数、脚本扫描、各层行数增长、规则复制密度、Idea-gate 结果的落点追踪（`work/` → canonical 的链路）。

**未做：** 未运行任何 harness；未写 `.research/`；未改任何 fixture；未复跑 Wave G/H。

---

## 1. 机械确认（任务书点名项 + Q12）

| 项 | 结果 | 证据 |
|---|---|---|
| `v0.1.1` tag 未移动 | **✓** | `git rev-parse v0.1.1^{commit}` = `762deb4c9db896acb5c00066b8e6dc5a63732cfa`（annotated tag object `90a41d5d`）；`v0.1` 同样在位 |
| 根 `.research/` 仍 UNINITIALIZED | **✓** | `git diff --stat v0.1.1 HEAD -- .research/` **为空**；`PROJECT.md` `Project Status: UNINITIALIZED`；`STATE.md` `Story Status: NOT_INITIALIZED`、33 行；八个 `*.md` + `work/.gitkeep` + `reviews/.gitkeep`，无第九个 canonical 文件 |
| 8 canonical 文件 | **✓** | `.agents/templates/` 恰 8 个模板，与根 `.research/` 八文件同名；无 `RUBRIC.md` / `CLAIMS.md` / `HYPOTHESES.md` |
| 一个 `research-loop` | **✓** | `.agents/skills/research-loop/` 唯一调度 Skill；`framework-maintenance` 自述 "Not a 13th Skill"，不进科研 loop |
| 5 subagents | **✓** | `experiment-agent`、`literature-scout`、`research-lead`、`result-analyst`、`reviewer`，与 v0.1.1 同名同数 |
| 恰 2 个新科学 Skill，共 12 | **✓** | `ls .agents/skills` = 12 目录 = v0.1.1 的 10 + `idea-evaluation` + `evidence-verification`；`AGENTS.md` 路由表 12 行一一对应 |
| 无框架运行时脚本 | **✓** | `find .agents .claude .codex .cursor -name '*.sh' -o -name '*.py' -o -name '*.js' -o -name '*.ts' -o -name Makefile` 为空；`.agents` 下无可执行位文件。Wave G 的 `run-harness.sh` / `test3-deeplink.py` 只在 `/tmp`，不在仓库 |
| FROZEN CORE byte-identical | **✓** | `git diff --stat v0.1.1 HEAD -- experiment-record.md git-linking.md state-files.md story-loop.md .agents/templates/` **为空**（171 / 135 / 180 / 126 行不变） |
| `reviewer.md` 契约 | **✓** | `git diff v0.1.1 HEAD -- .agents/subagents/reviewer.md` = **+20 / −0**，唯一新增段 "Task loads (progressive)"；Verdict 词表、五段 body、独立性、文件命名零改动（新增段自述 "This file remains the Protocol owner for Verdict vocabulary, the five body headings, independence, and review file naming"） |
| `AGENTS.md` 尺寸 | **✓** | 76 → 78 行（+2：两条新 Skill 路由行 + 一句「Layer 2 / prompts 随 Skill 按需加载，冷启动不必通读」） |

**各层行数增长（`git show v0.1.1:` vs HEAD）：**

| 层 | v0.1.1 | HEAD | Δ |
|---|---|---|---|
| Protocol 四件（`experiment-record` / `git-linking` / `state-files` / `story-loop`） | 612 | 612 | **0** |
| `.agents/templates/`（8） | — | — | **0**（byte-identical） |
| `.agents/skills/`（10 → 12） | 1,121 | 1,655 | +534 |
| `.agents/prompts/`（4 → 12） | 255 | 2,353 | **+2,098（×9.2）** |
| `.agents/subagents/`（5） | 528 | 752 | +224 |
| `.agents/references/research-intelligence/`（0 → 6） | 0 | 1,732 | +1,732 |
| `AGENTS.md` | 76 | 78 | +2 |
| **指令层合计** | **1,980** | **6,570** | **×3.3** |

结论：增长全部落在 Layer 2 与 Layer 3（prompts），Protocol 与 canonical 状态形状零变化。这是 V0.2 设计承诺的形状。**但 ×3.3 的指令量本身不是「更会推理」的证据**，见 Q1。

---

## 2. 十二问逐答

### Q1 — agent 真的更会做科学推理了，还是只是 prompt 更长？

**结论：指令层——是真的加入了 judgment operators，不是注水；行为层——按 V0.2 自己的 `evidence-and-claim.md` §F 口径，只能标 `insufficient evidence`。**

指令层证据（抽三处足以说明不是套话）：

- `failure-diagnosis.md`：9 个失败类别各带 "tells"（可观察征兆）+ 8 个 move 推荐 + 显式 decision logic；`bounded debug` 默认 1–3 轮后**停**并由 Main 写 STATE blocker。v0.1.1 的 `experiment-execution` 只有「失败 → `failed` + `not-assessed`」一条规则，没有「工程失败 vs 科学失败」的分类器。
- `result-diagnosis.md` L70–71 "Analyze in this order. Do not reorder. Do not start at Story"，十步顺序 Integrity → effect → variance → baseline fairness → mechanism attribution → heterogeneity → rival → Outcome → Story → next；L145–157 "Hard mappings" 把技术失败 / 完成但不可用 / 先前证据失效三种情形分别映射到 Protocol 已有词（`failed`+`not-assessed` / `invalid` / Invalidated Findings），并三次强调 **不产生 Negative Discovery**；L173 "Parameter sweeps are not a discriminating action unless the Question is itself a threshold"。这些是 v0.1.1 的九个中文问题所没有的算子。
- `experiment-design/SKILL.md` L63–78：设计字段从 6 个增加到 13 个（Unit of analysis / Confounders / Controls / Interpretation matrix / Must-Nice-Cut …），但 L80–81 "Map into the existing EXP section. Do **not** invent new canonical files, index columns, or Protocol enums" —— 算子进 Skill、形状不进 Protocol。

行为层证据：

- `gate-d.md` 是诚实的 **dry-read**（L1 标题、L4 "No Codex/Claude CLI"、L162 "Instruction dry-read does not mark them REGRESSED"、L167 "Wave G owns live Codex/Claude CLI traces; this Gate D file is instruction-level only"）。十个 case 的「无回归」是**读指令文本**得出的，不是观察行为得出的。
- **Wave G 是唯一的 live 行为证据，但它测的不是判断力。** 喂给 agent 的 prompt（`/tmp/rss-wave-g-prompts/`）把评分规则写进了任务：
  - Test 1："Recommended Action must be exactly one of REVISE or PARK … **ADVANCE is a miss.**"；fixture 候选描述直接给出 "Honest difference vs WES: diagram boxes renamed … **no new information source** … same threshold decision rule"。
  - Test 2："Artifact existence / F1 0.91 vs 0.41 **must NOT be treated as** the honest-baseline / Core Idea criterion being satisfied"；"Report label per §F … **typically `does not address` or `insufficient evidence`**"；MOCK 尾部直接写 "Integrity of this job can hold … **The baseline criterion still fails.**"
  - 于是两份报告里判 PASS 的判断类行（`wave-g-codex.md` L89 fatal flaw、L175 honest baseline `does not address`；`wave-g-claude.md` L68 mechanism distinction、L146）是 agent **复述被告知的答案**，不是自己得出的。真正被 Wave G 证明的是：Codex / Claude Code 能通过 symlink 找到 Skill + prompt + Layer 2（Test 3 deep-link 充分）、能产出规定 heading、能守住「canonical eight 不写、不建 EXP、不改 Outcome」的写纪律。这三项**有价值且成立**，但不是 Q1 的证据。
  - V0.2 自己的 Layer 2 给这个现象起了名字：`skill-evolution.md` L131–132 "Using them to retune is **leaking the test into the prompt**"。Wave G 报告没有披露这一点，两份报告的 "宿主限制" 段只谈 CLI 参数。
- Wave H 的 baseline 运行（VAEG 伪装 costume → REVISE，REPORT L124、L153）是目前**最接近**「未被喂答案」的判断证据，因为 Wave H fixture 把作者 pitch 写成对抗性（"laundering costume"）而不是自我揭露。但 Wave H 是同模型自评（REPORT L22 "Operator: this model (Cursor Grok 4.6)"），且只有一个 case。

因此 Q1 的诚实答案是：**prompt 不只是更长，算子是真的；但「agent 实际推理更好」尚无未受污染的 live 证据。** 见 §3 E-B1。

### Q2 — 门太多了吗？

**结论：门的数量没问题（新增两个，均触发式）；「词表」的数量是负担。**

- 新门只有两个：Idea-gate、Evidence-gate。`AGENTS.md` L48 / L53 路由行是场景限定（「新 Core Idea / 换路线 / 高代价实验」、「结果拟进 Story Evidence / 高风险结果」），不是每 EXP 默认。`research-loop` L84 "Selective — **not** a default chain. Ordinary exploratory EXP stays light."；L95–97 明写普通探索链 = design → execution → result-analysis，**without** 四类重物；L156–159 Deviation 禁止 "force every EXP through `idea-evaluation` → `evidence-verification` → reviewer"。
- Review 门是 v0.1.1 已有；deep-literature 是 optional mode；bounded debug 是**停止规则**不是门；`scout/focus/confirm` 是内部选路，禁止写 STATE。
- 真正的负担在别处：V0.2 引入约 **10 套小词表 / ≈59 个新 token**（Idea-gate 4、§F 报告标签 6、§G 措辞强度 7、失败类别 9 + move 8、route stage 3、lit mode 2、`framework-maintenance` mode 4、citation depth 4、evidence rungs 8、Decision Frontier 4），每套都要靴带一句「not Protocol / not Outcome / not Verdict」。`rg` 统计：此类免责句 **68 行 / 28 个文件**。这不是门多，是 agent 要同时记住的「不能写进哪儿」的禁令多。见 Q4、E-N3。

### Q3 — 自主性受损了吗？

**结论：文本层没有；两个门实际上把「自我否决」的能力交给了 agent 而不是用户。一个小缺口会让 Main 在 PARK 之后不知所措（见 Q5）。**

- `AGENTS.md` L28–32 Autonomy 段与 v0.1.1 逐字相同："不要求每步询问用户。Skills 是 strong guidance，不是强制状态机。"
- `workspace-resume` 新增 Decision Frontier（L47）：只问缺失项，"`UNINITIALIZED` is a Decision Frontier"（L84）——这是**减少**询问。
- `research-loop` L120–121 "skipping steps is allowed"；`idea-evaluation` L88 "PARK or ABANDON is progress" —— 门的产物是 agent 自己的决定，不是向用户请示。
- `evidence-verification` 禁止 auto-Outcome / auto-Story，这是对**子任务写权限**的约束（与 v0.1.1 「Subagent 只写 work/」同一原则），不是用户门。
- bounded debug "Default 1–3 … then stop and have Main record a blocker in STATE" 是防死循环的自主停机，"Default" 允许偏离。
- 残留风险：Idea-gate 给出 PARK/ABANDON 后，Skill 只说 "Main Agent integrates"，没说整合到哪个 canonical 字段（Q5）。一个谨慎的 Main 在此处最可能的行为是回头问用户「这个想法要放哪」，这是自主性被**缺口**而非被**门**削弱。

### Q4 — Research Intelligence 规则有重复吗？

**结论：算子级重复——低（Gate C B1 修复后守住）；元规则级重复——中高，且不在 `framework-maintenance` 现有 checklist 的检查范围内。**

算子级（复核）：Outcome 表只在 `experiment-record.md`；Verdict 表只在 `reviewer.md`；Idea-gate 四动作只在 `idea-and-mechanism.md` §H；§F 标签只在 `evidence-and-claim.md`；§G 措辞只在 `scientific-reasoning.md`。其它地方一律 cite。开发侧审计 #1 / #11 与本审核 `rg` 结果一致。

元规则级（本审核 `rg` 计数）：

| 被复制的规则 | 份数 | 备注 |
|---|---|---|
| 「not Protocol / not Outcome / not Verdict / do not recopy / cite only」免责句 | 68 行 / 28 文件 | `evidence-verification.md` 一个 prompt 内 7 处 |
| 「exploratory stays light / stop, no file / not a boot set / must not auto-load」保护句 | 36 行 / 18 文件 | 保护是好事，但同一句在 Skill、prompt、subagent、Layer 2 四层各说一遍 |
| 「task prompt wins on artifact shape」 | 6 份（`subagent-handoff.md` owner + 4 role 文件 + `next-research-move.md`） | Gate C B2 允许 "optionally a sentence in each role prompt"，已成六份 |
| 「Do not load every file under `research-intelligence/`」 | 7 份（4 role + reviewer + handoff + research-loop）+ `AGENTS.md` L57 | 同上 |
| **数值参数「1–3 iterations」** | 5 行 / 3 文件（`experiment-execution` ×2、`failure-diagnosis` ×2、`experiment-agent` ×1） | **没有指定 owner**；改一处不改其余即漂移。见 E-N2 |
| 「technical failure → `failed` + `not-assessed` → 不进 Negative Discovery」 | 6 文件 | Protocol owner 在 `experiment-record.md` L75；其余为应用，各自 cite，可接受 |

`framework-maintenance` checklist #1 "Duplicate rules" 只检查 Protocol 表（Outcome / Verdict / Story 六段 / 更新链），元规则复制不在其范围，所以开发侧审计全 PASS 与本表并不矛盾。见 E-N3。

### Q5 — 有隐藏的第二状态平面吗？

**结论：没有新建平面，但 V0.2 在既有的 `.research/work/` 上放了一种**新型决策产物**（Idea-gate 结果）而没有给它 canonical 落点。这是「文件即记忆」的一个真实缺口，需要一段话修补。**

守住的部分：

- `.research/work/` 与 `.research/reviews/` 在 v0.1.1 已存在（`.gitkeep` + `subagent-handoff.md` L29–32 四行 owner 表），不是 V0.2 新建。
- 明文禁止新 canonical 文件：`evidence-verification/SKILL.md` L103–105 "Do not create `RUBRIC.md`, `CLAIMS.md`, or other canonical state files"；`idea-evaluation/SKILL.md` L108 "Must not add canonical state files"。
- 明文禁止内部词进 STATE：`research-loop` L71、L157–158（`scout/focus/confirm`）；`result-diagnosis.md` L140–143（§G 措辞不进 Outcome）；`evidence-verification` §F 标签不进 Outcome。
- fixtures 放 `docs/validation/`，`skill-evolution.md` L260 禁止放 `.research/`。

缺口（追踪链路）：

1. `idea-evaluation/SKILL.md` L101–110 Updates 表**只有一行**：`.research/work/idea-evaluation-<slug>.md`。随后 "Main Agent integrates" —— 整合到哪儿，未说。
2. `idea-evaluation.md` prompt L198–210 Handoff 段：只写了 ADVANCE 之后可去 `experiment-design`；**REVISE / PARK / ABANDON 的落点为空**。
3. `research-loop` L84–91 只把 Idea-gate 当路由；L116 "After evidence, follow `state-files.md` §更新顺序" —— 而 §更新顺序是为**实验证据**写的，一个被 PARK 的想法不是 EXP（无 EXP-ID）、不是 Discovery（无实验）、不是 Story Evidence。
4. `workspace-resume` 全文**不读 `work/`**（`rg 'work/'` 零命中）；`research-memory` 只在 L101 "May leave an agent/workflow-friction note in `.research/work/`"，不回收 `work/` 中的决定。

后果：AEA 被 PARK 的理由只活在 `work/idea-evaluation-aea.md`。下一个 session 冷启动读 PROJECT / STORY / STATE 看不到它；`STATE.md` 即便被 Main 写了一句，按设计几十行、会被覆盖。于是要么下个 session 重新提出 AEA（重复劳动），要么 `work/` 逐渐变成事实上的决策日志 —— 这正是本问所指的「隐藏第二平面」。相对 v0.1.1 **不是回归**（v0.1.1 根本没有显式 Idea 决定），但是 V0.2 自己新引入的产物没有闭环。修补是一段话，不动 Protocol：见 E-B2。

### Q6 — 普通探索还轻吗？（Case 10）

**结论：指令层——轻，且是多层加固的轻；行为层——**零 live 运行**，只有 dry-read。**

指令层证据齐全：`research-loop` L95–97、L156–159；`idea-evaluation` "Cheap exploratory / sanity EXP … must not auto-load this Skill"，prompt L214 "Trigger does not match → stop with no file"；`evidence-verification` "ordinary exploratory → stop; no file"；`experiment-design` Deviation L122–123 "Explicitly exploratory EXP: short Question + honest baseline is enough; do not invent a Protocol flag"；`result-analysis` Deviation L116–117 "Trivial exploratory runs: merge with execution in one session"；`result-diagnosis.md` L34–37 / L266–267（仅在被 dispatch 时适用，且 "do not inflate into a gate"）；`AGENTS.md` L57 冷启动不通读 Layer 2；`result-analyst` When-to-use 仍是「普通探索不强制」。`gate-d.md` L152 dry-read 结论 "PROTECTION invariant held"。

未覆盖：Case 10 是唯一的 protection fixture，Wave G 没有跑它；Wave H 也只把它当 dry protection。而 Case 10 恰恰是「加门会不会把一切变重」这个头条风险的**唯一行为探针**，且是十个 case 里最便宜的一个。另有一处措辞歧义：`result-analysis` L52–55 "Answer analysis questions — Work through explicitly (`result-diagnosis.md` …)" 可被逐字执行的 harness 读成「每个 EXP 都走十段 heading」，虽然 Deviation 段已放行合并。见 E-B3、E-N10。

### Q7 — Reviewer 被过度扩张了吗？（Verdict 契约必须不变）

**结论：没有。**

`reviewer.md` diff = +20 行、一个段落，仅新增 progressive task-load 说明与 Layer 2 按需清单；Verdict 词表、五段 body、独立性、`*-review-r<N>.md` 命名零改动。三份 review prompt（`method-review` 63→99、`result-review` 64→94、`experiment-review` 60→81）与 `experiment-review` SKILL（102→111）的增量是 Layer 2 lens 引用与 "cite reviewer.md §Verdict — do not recopy" 类句子（Gate C 已逐条核对；本审核抽查 `result-review.md` L94 仅引用 `experiment-record.md` §Outcome 值）。`research-loop` L122–124 对 `ATTENTION_REQUIRED` 的引用仍指向 `reviewer.md` §Verdict。Idea-gate 四动作与 §F 标签在所有出现处都被声明 "not a Verdict"。

### Q8 — `idea-evaluation` 太保守了吗？

**结论：按设计不保守；按行为无法判断（ADVANCE 情形没有 live 运行）。**

- 四个动作里 ADVANCE 是常规出口；Case 02（真实新信息源 PRRW）的 dry-read 得 ADVANCE（`gate-d.md` L60）。
- 触发面窄（Core Idea / 换路线 / 机制替换 / 高代价 successor），"Not for" 排除便宜探索。
- 偏保守的措辞只有一句 `SKILL.md` L88 "PARK or ABANDON is progress"，它的作用是消除「不 ADVANCE 就是失败」的心理，不是提高 ADVANCE 门槛。
- `gate-d.md` L60 自己点名了要盯的失败模式："PARK/REVISE *only* because the name is new, despite a real information-flow change — that would be a V0.2 fixture miss"。这一模式**只能用 Case 02 的 live 运行**排除，目前没有。
- Wave H：baseline 对 VAEG（costume）给 REVISE，方向正确；候选补丁无增益 → reject。它证明的是「不会因为补丁变得更保守」，不证明「对真想法不保守」。

建议（非阻塞）：把 Case 02 作为 tag 后第一个 held-out live case，或并入 E-B1 的去泄漏复跑。

### Q9 — `evidence-verification` 官僚吗？

**结论：不官僚，是**按赌注定价**的一页纸；只要触发面守住（已守住）就不会变成日常税。**

- 每条 criterion 10 个字段，每个字段对应 §A–§F 的一个算子（existence / integrity / match / satisfaction 分离；"Excluded is mandatory"）。去掉任一字段，Case 03 的失败模式（metric existence = criterion satisfied）就会重新出现。
- 触发条件：Story Evidence 候选 / surprising strong / main baseline / Story-core change（`AGENTS.md` L53；Skill When-to-use）；普通探索 "stop; no file"。一个 Story Evidence 候选通常 3–5 条 criteria ≈ 一页。Story Evidence 是仓库里最持久的主张，一页纸是合理成本。
- 它**不能**写 Outcome / Story / Reviewer 文件，所以它不能阻塞任何事，只能给 Main 提供 `does not address` 之类的标签。官僚机构的特征是能否决而不负责；这个 Skill 恰好反过来。

### Q10 — deep literature 有高成本触发器吗？

**结论：有「赌注型」触发器，没有「预算型」上限。可接受，记 N。**

`literature-research` 默认 light；deep 仅在 novelty / 新核心机制 / 会改 Story 的冲突 / 领域惯例瓶颈时进入，并链接 `deep-literature-mode.md`；后者自述 optional、expensive，要求先冻结 RQ、多角度检索、closest-work matrix、citation-depth 标注。`scientific-reasoning.md` §F 明说 exploratory 不自动触发三门。缺的是显式预算行（来源数 / 时长 / 停止条件）—— 目前由 Main 与 RESOURCES 约束。建议在 `literature-synthesis.md` Task fields 加一行 budget（非阻塞，E-N9）。

### Q11 — skill evolution 会自我强化坏行为吗？

**结论：机制是 fail-closed 的，Wave H 证明了它会拒绝无增益补丁；但两个结构性缺口让它**尚不能**防止「模型朝自己的偏好优化」。**

守住的：`skill-evolution.md` §A 触发必须先复现一个真实 miss；一次一个原子改动；failure / protection / held-out 三件 fixture；held-out 冻结后才评分（L131–132、L203）；"never auto-overwrite canonical Skills"，最多 "deserves review"。Wave H REPORT：§A 不成立（L29–41）、scored token 相同（L129）→ **reject**（L3、L147、L246），canonical 未动。这是正确的行为。

缺口：

1. **评分者独立性未要求。** Wave H 的作者、执行者、评分者是同一模型（REPORT L22）。`skill-evolution.md` 没有 "scorer ≠ author / different family" 的要求。同模型自评最容易通过的补丁，正是把 prompt 改得更像它自己的偏好。
2. **fixture 本身可以是喂答案的。** Case 01 README L143–146 在**输入**里写 "Honest difference vs WES … no new information source"。一个候选补丁若在这种 fixture 上 PASS，几乎不说明判断力；反之 Wave H 自建的 VAEG fixture（作者 pitch 写成对抗性）才是正确写法。`skill-evolution.md` L131–132 只禁止「用 held-out 调参」，没禁止「评分规则出现在 agent 可见输入里」。

两点都可用一两句话补进 `skill-evolution.md` §D/§E（E-N7、E-N8），不阻塞 tag。

### Q12 — 8 canonical / 一个 research-loop / 5 subagents / 恰 2 新 Skill / 无脚本？

**全部 ✓**，见 §1 表。补充：12 prompts = 4 升级 + 8 新增；Layer 2 六件；`.claude/skills/` 12 个 symlink 指向 `.agents/skills/`（Wave G Test 3 与开发侧审计 #7 均验证；本审核未改动）。

---

## 3. 发现

严重度用 Gate 惯例：**E-B** = 本 Gate 要求在 tag 前处理（每项都是有界小改）；**E-N** = 非阻塞，可入 release notes 或 v0.2.1。

### Blocking（targeted）

**E-B1 — Wave G 的判断类 PASS 不是证据：评分规则泄漏进 agent prompt。**
Owner：`docs/validation/research-intelligence/prompt-regression/`（文档层，不是框架层）。
证据：§2 Q1 引文；`/tmp/rss-wave-g-prompts/test1-idea-evaluation.txt`、`test2-evidence-verification.txt`；两份 Wave G 报告无披露；prompt 原文仅存 `/tmp`，仓库内不可复核（`wave-g-codex.md` L44 自述）。
要求（二选一，优先 a）：
- (a) 用**去泄漏** prompt 复跑 Test 1、Test 2 各一次（≥1 个 harness）：删除 "ADVANCE is a miss"、"must be exactly one of REVISE or PARK"、"Honest difference vs WES" 四条自我揭露（改为作者口吻的 pitch，可参照 Wave H VAEG 写法）、"must NOT be treated as … satisfied"、"typically `does not address` or `insufficient evidence`"、"The baseline criterion still fails"；**保留** MOCK 状态、磁盘 artifacts、trigger、写纪律硬约束、输出 heading 与 stdout 行格式。把实际 prompt 原文归档到 `docs/validation/research-intelligence/prompt-regression/wave-g-prompts/`（docs，不是 `.research/`，不是框架）。结果**如实记录**：命中 → Q1 行为证据成立；未命中 → 按 Wave H 同一口径写成 fixture miss / known limitation，**不阻塞 tag**，但 README 与 release notes 必须相应措辞。
- (b) 不复跑，但把两份 Wave G 报告与 README 的相应主张改写为「验证的是 harness 可移植性 + 输出形状 + 写纪律；科学判断行为未经未泄漏 live 测试」。
本审核认为 (a) 成本低于半天，且是 V0.2 头条主张唯一可能的支撑，强烈建议 (a)。

**E-B2 — Idea-gate 结果没有 canonical 落点（Q5）。**
Owner：`.agents/skills/idea-evaluation/SKILL.md` §Updates 或 §Default flow 末尾；可选在 `research-loop` §6 加一句 cite。
要求：一段话，形如 —— ADVANCE → `experiment-design`（已有）；REVISE → Main 在 `STATE.md` Recommended Next Action 写一行（短期）；PARK / ABANDON → Main 在 `DISCOVERY.md` §Current Scientific Understanding（或 STORY Boundary / Open Gaps，视其是否改变边界）写**一行**结论并 cite `work/idea-evaluation-<slug>.md`，使 `workspace-resume` 冷启动可见。不新建文件、不新增枚举、不改 `state-files.md`。

**E-B3 — Case 10（protection）无 live 运行。**
Owner：`docs/validation/research-intelligence/prompt-regression/`。
要求：任一 harness 跑一次 Case 10，prompt 只含 MOCK + trigger + 写纪律硬约束（**不**提任何 Skill 名、不提 Layer 2）；按 Case 10 README L133–147 §Expected behavior 评分。PASS 标准：无 `idea-evaluation` / `evidence-verification` 工作文件、无 reviewer / `result-analyst` 派发、未整读 `research-intelligence/`、EXPERIMENTS 得到一个 §Outcome 值 token、Story 无数字。可与 E-B1(a) 同批执行。

### Non-blocking

- **E-N1** `research-loop` L91 高风险证据行 "`evidence-verification` → `experiment-review`" 未写 `result-analysis`；`gate-d.md` L164 已标为 temptation。加一个从句 "after `result-analysis`"。建议与 E-B2 同一次编辑。
- **E-N2** 数值参数「1–3 iterations」5 行 / 3 文件无 owner。建议 `failure-diagnosis.md` 为 owner，其余两处改为 cite。
- **E-N3** 元规则复制：68 行免责句 / 28 文件，36 行保护句 / 18 文件，"task prompt wins" 6 份，"do not load every file" 7 份。建议 `framework-maintenance` checklist 增加一行「meta-rule copies」并设阈值；v0.2.1 收敛。
- **E-N4** `skill-evolution.md` L11–12 / L239 仍是将来时 "V0.2 will add exactly 2"（开发侧审计已记 MINOR）。tag 后改现在时。
- **E-N5** 仓库无 `.gitignore`；`graphify-out/` 与 `v0.2-maintenance-audit.md` 均未跟踪。commit 9 **不得** `git add -A`；要么加 `.gitignore`（`graphify-out/`），要么删目录。`v0.2-maintenance-audit.md` 是否入库由 Lead 决定（入库则它是 commit 9 evidence 之一）。
- **E-N6** 版本串：`AGENTS.md` L3、`README.md` L3 "Framework base: v0.1.1"；`README.md` L64 / L71 债务清单仍列 "Test F/G/J、Subagent 狗食落盘、四 Harness 全写闭环、OpenCode 实测"。commit 9 更新，并按 E-B1 结果**如实**写行为证据边界。
- **E-N7** Case 01 fixture 输入含自我揭露（L143–146）。后续 skill-evolution 轮次应把候选 pitch 改为对抗性口吻，评分规则只留在 §Expected。
- **E-N8** `skill-evolution.md` §D/§E 缺「评分者 ≠ 候选作者（或 different family）」与「agent 可见输入不得含评分规则」两句。
- **E-N9** `deep-literature-mode.md` / `literature-synthesis.md` 缺显式预算行（来源数 / 时长 / 停止条件）。
- **E-N10** `result-analysis` L52–55 措辞可被读成「每 EXP 走 `result-diagnosis.md` 十段」；建议改为「in-session 用下列九问；dispatch 或高风险时用 `result-diagnosis.md`」。
- **E-N11** 两份 Wave G 报告缺「本测试测什么 / 不测什么」一行；若选 E-B1(b) 则此项即为修法。
- **E-N12** OpenCode 仍 documentation-only / 未实测（v0.1.1 已知债务，沿用）。

---

## 4. Verdict

**`TARGETED_REVISION`**

理由：架构（FROZEN CORE、8/1/5/12/0、选择性门、Reviewer 契约、自主性文本）全部守住，Layer 2 与 prompts 加入的是真实算子而非注水，`skill-evolution` 在 Wave H 表现 fail-closed —— 这些都不需要重做。但 V0.2 的头条主张「Research Intelligence」目前只有**指令层**证据；唯一的 live 证据（Wave G）把评分规则写进了 agent prompt，按 V0.2 自己的 `evidence-and-claim.md` §A/§E 与 `skill-evolution.md` L131–132 的标准不能算证据，而报告把它标成了 PASS 且未披露。同时 Idea-gate 结果没有 canonical 落点，与「文件即记忆」直接冲突；protection case 零 live 运行。三项都是有界小改（预计合计半天内），不涉及 Layer 2 六件、Protocol 四件、`reviewer.md`、templates、门的增删或新 Skill。

**退出条件（全部满足即可进入 Gate E re-review；re-review 只做 diff 核对，不重读全仓）：**

1. E-B1 (a) 或 (b) 完成；若 (a)，prompt 原文归档且报告如实记录（miss 不阻塞，但必须写成 limitation）。
2. E-B2 一段落点规则落入 `idea-evaluation/SKILL.md`（可选 `research-loop` §6 一句 cite）。
3. E-B3 Case 10 一次 live 运行 + 报告。

**明确不要求：** 不改 Layer 2 六件；不改 Protocol 四件与八模板；不改 `reviewer.md`；不增减门；不加第 13 个 Skill；不复跑全部十个 case；不测 OpenCode；不要求 E-B1(a) 必须 PASS。

---

## 5. Release notes（tag 前 / commit 9 携带，均非阻塞）

1. `AGENTS.md` L3、`README.md` L3 版本串 → v0.2（按任务书，在 commit 9 完成）。
2. `README.md` 债务清单刷新；新增一行「行为证据边界」：Gate D dry-read（10 cases，instruction-level）+ Wave G live（harness 可移植性 / 输出形状 / 写纪律）+ E-B1 复跑结果（如做）+ Wave H reject；Case 02 为首个 tag 后 held-out。
3. 加 `.gitignore`（至少 `graphify-out/`）；决定 `v0.2-maintenance-audit.md` 是否入库。
4. `skill-evolution.md` 时态改现在时（E-N4）。
5. 「1–3 iterations」指定 owner（E-N2）；`research-loop` 高风险行补 `result-analysis`（E-N1）—— 若与 E-B2 同批编辑则一并带入。
6. v0.2.1 backlog：E-N3 元规则收敛 + checklist 行；E-N7 / E-N8 `skill-evolution.md` 评分者独立性与 fixture 卫生；E-N9 预算行；E-N10 `result-analysis` 措辞；E-N12 OpenCode。

---

## 6. 短摘要

FROZEN CORE byte-identical；`v0.1.1` → `762deb4c` 未动；根 `.research/` 与 v0.1.1 零 diff、仍 UNINITIALIZED；8 canonical / 1 research-loop / 5 subagents / 12 Skills（恰 +2）/ 0 脚本；`reviewer.md` 仅 +20 行 progressive-load 段，Verdict 契约不变；普通探索链在指令层多重加固；自主性文本未损；`skill-evolution` 在 Wave H 正确 reject。指令层确实加入了真实的 judgment operators（失败分类器、十步诊断顺序、per-criterion 证据算子、设计字段映射），不是注水；指令层从 1,980 行增至 6,570 行，全部落在 Layer 2/3。**但**「agent 更会推理」尚无未泄漏的 live 证据：Wave G 把评分规则写进了 agent prompt（"ADVANCE is a miss"、"typically does not address"、"The baseline criterion still fails"），报告标 PASS 而未披露；Idea-gate 的 PARK/ABANDON 没有 canonical 落点，`workspace-resume` 看不见；Case 10 protection 零 live 运行。三项有界修补（去泄漏复跑或如实改写 + 一段落点规则 + 一次 Case 10 live）后可 tag。**Verdict：`TARGETED_REVISION`。**
