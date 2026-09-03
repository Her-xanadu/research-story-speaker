# Gate A Review — V0.2 Wave A Layer 2（Research Intelligence）

- **角色：** 独立 Gate A Reviewer。相对 Wave A 开发者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘）。
- **审核时点：** 2026-09-04
- **对象：** workspace `/Users/herxanadu/research-story-speaker`，分支 `v0.2-research-intelligence`。单一 worktree（`git worktree list` 仅本目录）。HEAD = `dd7e43f72db1883209baf7cfef151ac218a17b92`（"Rename project identity to research-story-speaker"），与 `docs/validation/research-intelligence/BASELINE.md` 记录的冻结 HEAD 一致。
- **工作树状态：** `git diff` / `git diff --cached` 均为空 → **没有任何已跟踪文件被修改**（Skills、prompts、Protocol references、`.research/`、`AGENTS.md`、adapters 全部未动）。未跟踪新增仅四项：`.agents/references/research-intelligence/`（六个 Wave A 文件 + 一个 `.gitkeep`）、`docs/design/`、`docs/validation/research-intelligence/`、`graphify-out/`（工具残留，见 N9）。
- **只写：** 本文件。未 git commit；未启动 Wave B。
- **Gate 判定词汇：** 本文件的 Verdict 取值 `APPROVE | TARGETED_REVISION` 由 Gate A 任务书规定，是框架层 gate 结论，**不是** `reviewer.md` §Verdict 的 EXP review 词汇，也不新增任何 Protocol 枚举。

---

## 0. 阅读范围与方法

**Layer 1 Protocol（逐字读完）：** `experiment-record.md`、`reviewer.md`、`state-files.md`、`story-loop.md`、`git-linking.md`。

**Layer 2 Wave A（逐字读完，共 1,739 行）：** `scientific-reasoning.md`（262）、`idea-and-mechanism.md`（241）、`experiment-thinking.md`（346）、`evidence-and-claim.md`（309）、`deep-literature-mode.md`（302）、`skill-evolution.md`（279）。

**Sources / freeze：** `docs/design/research-intelligence-sources.md`、`source-audit/2026-09-04-license-recheck.md`、`BASELINE.md`。

**交叉核对（判断 ownership 与一致性所需）：** 10 个 `SKILL.md` 中的 `result-analysis`、`experiment-design`、`literature-research`、`framework-maintenance`、`research-loop`、`experiment-review`、`story-maintenance`、`research-memory`；`AGENTS.md`；`.agents/prompts/subagent-handoff.md`；`templates/LITERATURE.template.md`、`EXPERIMENTS.template.md`、`DISCOVERY.template.md`。

**上游原文（用于 Q8）：** `/Users/herxanadu/.agents/skills/idea-evaluator/SKILL.md` + `references/fatal-flaws.md`；`/Users/herxanadu/.agents/skills/deep-research/SKILL.md` + `references/hedge-calibration.md`、`citation-protocol.md`；`/Users/herxanadu/aris_repo/skills/experiment-plan/SKILL.md`。

**机械检查：** 对六个 Wave A 文件 vs 上游 `deep-research/**`、`idea-evaluator/**`、ARIS `experiment-plan/**` 全部 `.md` 做小写去标点后的 6-gram / 8-gram 共享片段扫描（脚本置于 `/tmp`，未写入仓库）；`rg` 扫描 Protocol 免责声明密度、悬空引用、四动作词汇出现位置；`find` 扫描禁止文件名。

---

## 1. 冻结核心确认（亲自读盘）

| 项 | 要求 | 实测 | 判定 |
|----|------|------|------|
| `.research/` canonical 文件 | 恰好 8 个 | `DISCOVERY / EXPERIMENTS / LITERATURE / PROJECT / RESOURCES / REVIEWS / STATE / STORY`.md = 8；另有 `reviews/.gitkeep`、`work/.gitkeep` | **PASS** |
| Skill 目录 | 恰好 10 个 | `experiment-design, experiment-execution, experiment-review, framework-maintenance, literature-research, research-loop, research-memory, result-analysis, story-maintenance, workspace-resume` = 10 | **PASS** |
| 禁止文件 | 无 HYPOTHESES / RUBRIC / CLAIMS / FINDINGS / PROGRESS.md | `find -iname` 全仓库（含 docs、.agents）零命中；顺带核对 SURVEY.md / TASK_PLAN.md 也为零 | **PASS** |
| 已跟踪文件改动 | 无 | `git diff --stat` 与 `--cached` 均空 | **PASS** |
| Layer 2 是否被现有框架文件引用 | Wave A 不应改 Skills | `rg research-intelligence|scientific-reasoning|...` 于 `AGENTS.md`、`CLAUDE.md`、`README.md`、`.agents/skills`、`.agents/prompts`、`.agents/subagents`、`.agents/references/*.md`、`adapters` 零命中 | **PASS**（Layer 2 当前不可达，符合 Wave A 范围） |
| 标签 | `v0.1.1` / `v0.1` 不动 | BASELINE.md 记录 peeled commit；本审核未触碰标签 | 未复核 peeled SHA（非本 gate 任务） |

---

## 2. 十问

### Q1. 六个文件之间是否存在 ownership 重叠？

**判定：局部存在一处实质重叠，其余为已声明、可接受的软重叠。**

1. **实质重叠 — Idea-gate 四动作词汇 `ADVANCE / REVISE / PARK / ABANDON` 在两个文件、三处各自给出释义：**
   - `idea-and-mechanism.md` §D L126–129（REVISE / PARK / ABANDON 各一条 "if…" 释义）+ L131–135（声明 "Idea-gate recommendations only"）；
   - `idea-and-mechanism.md` §H L229–234（四个动作各一条 "—" 释义，措辞与 §D 不同：PARK 在 §D 是 "timing, resources, or a novelty check is the block"，在 §H 是 "not now (resources, sequencing, novelty check pending)"）；
   - `deep-literature-mode.md` §F L238–243 再给一套文献版释义（REVISE / PARK / ADVANCE），且**只列了三个动作却在 L245 写 "Those four actions"**。
   
   框架自己对 Verdict 的纪律是"只在 `reviewer.md` 定义，其它处写 `per reviewer.md`"（`reviewer.md` §Verdict；`experiment-review/SKILL.md` L17）。Layer 2 对自己新造的词汇没有执行同一纪律。Wave B 的 idea-evaluation prompt 若要输出这个推荐，必须有唯一 owner 段。→ **B1（blocking）**。

2. **软重叠（已声明，接受）：** `scientific-reasoning.md` §C L108–119 的 rival 列表（target / simpler mechanism / data artifact / baseline weakness / optimization / capacity / seed / measurement / leakage / selection）与 `experiment-thinking.md` §C L123–134 的 confounder 列表（split / seed / init / budget / capacity / augmentation / optimizer / preprocessing / extra information / tuning）语义大半重合。两文件各自声明角色分工（前者是"完成结果的竞争解释"，后者是"设计期要均衡的旋钮"，`experiment-thinking.md` L136–137 明说 "Do not paste that rival list here"）。可接受，但属于压缩候选 → N5。

3. **无重叠的部分：** 六文件的核心 operator 各不相同——objects / claim kinds / falsifiability / 强度阶梯（scientific-reasoning）；problem anchor / 机制身份 / fatal flaw / deletion test（idea-and-mechanism）；unit of analysis / controls / prediction matrix / must-nice-cut（experiment-thinking）；四层不等式 / criterion synthesis / integrity / match / 报告标签（evidence-and-claim）；RQ freeze / 多角度 / closest-work matrix / 矛盾图 / 引用深度（deep-literature-mode）；trigger / atomic / cases / regression / no-auto-deploy（skill-evolution）。跨文件全部以 §级链接互指，未见整段复制。

### Q2. 是否形成第二套 Protocol？（第二 Outcome 表、第二 Verdict 表、新状态文件、新 orchestrator）

**判定：否。**

- **第二 Outcome 表：无。** 唯一像枚举的两处都被明确限定为非 canonical：`evidence-and-claim.md` §F L204–211 六个 *per-criterion 报告标签*（"report labels… not Experiment Outcome values" L213–215；"Do not paste a report label into `EXPERIMENTS.md` as Outcome" L217；"one EXP can mix labels across criteria" L219–220）；`scientific-reasoning.md` §G L232–240 七个 *措辞强度词*（"not Experiment Outcome values, not Reviewer Verdicts, and not Status values" L226–227；"Overlap of English words with Protocol tokens is accidental and must not be exploited" L242–243）。两者作用域（单条 criterion / 口头强度）与 Outcome（整个 EXP 的科学结论）不同，且被禁止写入 canonical 文件。词面撞车风险见 N2。
- **第二 Verdict 表：无。** 四动作 `ADVANCE/REVISE/PARK/ABANDON` 限定为 work artifact 内的 Idea-gate 推荐（`idea-and-mechanism.md` L131–135、L236–238；`deep-literature-mode.md` L245–246）。`skill-evolution.md` 的 "deserves review | reject"（L274）是框架维护推荐，明确 "not a Reviewer Verdict"（L15–16）。
- **新状态文件：无。** 六文件均在文件头禁止 `HYPOTHESES.md` / `RUBRIC.md` / `CLAIMS.md` / `FINDINGS.md` / `SURVEY.md`（`scientific-reasoning.md` L12–13；`idea-and-mechanism.md` L6–7；`evidence-and-claim.md` L8；`deep-literature-mode.md` L9–10、L285）。RQ、closest-work matrix、complexity budget、criterion 块全部落 `.research/work/`（与 `subagent-handoff.md` L15、L29–32 的 work 路径一致）。skill-evolution 的 fixtures 落 `docs/validation/research-intelligence/cases|prompt-regression/`（L114–117），这两个目录 Phase 0 已建。
- **新 orchestrator：无。** `deep-literature-mode.md` 是一个可选模式，不调度 Skill、不写 canonical（L256–259 "Routing after that is the existing loop… not a new literature state machine"）。`skill-evolution.md` 自我定位 "Not a research-loop step"（L11）、"Treating this file as a step inside `research-loop`" 列为反模式（L256）。`research-loop` 仍是唯一调度者。

### Q3. 是否重新定义了 Outcome 或 Verdict？

**判定：否。与 Protocol 的每一处交界都核对一致。**

- Outcome / Verdict 的唯一定义处引用正确：`experiment-record.md` §Outcome 值、`reviewer.md` §Verdict（六文件头部均如此写）。
- `evidence-and-claim.md` §D L159–160 "crash / missing artifact 不是 scientific negative，不写 Negative Discovery — 规则已在 `result-analysis`" ↔ `result-analysis/SKILL.md` L59（`Status=failed` 且 `Outcome=not-assessed` → 不产生 Negative Discovery）**一致**。
- `evidence-and-claim.md` §G L266–272 "先前证据被证明泄漏/误绑 → 走 Invalidated Findings 而非 Negative" ↔ `result-analysis/SKILL.md` L61（`invalid` 不当 Negative Discovery）与 `DISCOVERY.template.md` L21–23（Invalidated Findings 段）**一致**。
- `deep-literature-mode.md` §E L203–211 把四档引用深度映射到既有 `Access` 三值 `metadata-only | abstract-only | full-text-checked` ↔ `LITERATURE.template.md` L13 **逐字一致**，并明确拒绝造第四个 Access 值（L210、L285）。
- `experiment-thinking.md` §A L49–58 "Prediction matrix → Expected outcomes，无专用字段时放 Motivation 下" ↔ `experiment-design/SKILL.md` L58、L101 **一致**；未新增 index 列（L60–61）。
- 唯一措辞欠精确处（非重定义）：三个文件写 "Story's six segments live **only** in `state-files.md`"（`scientific-reasoning.md` L10；`evidence-and-claim.md` L8；`skill-evolution.md` L8），而 `state-files.md` §单一事实来源 表中 Story 语义的 owner 是 "`story-maintenance` skill + 本文件 STORY 段"（两处共同）。无害，见 N10。

### Q4. 是否有过度僵硬的 checklist，迫使每个 EXP 走每个 gate？

**判定：否。六文件都内置了显式的"轻量保护"语句，且 gate 均限定触发条件。**

证据（每文件至少一处）：
- `scientific-reasoning.md` L15–18（"Do not load on every cold start… one-line sanity rerun"）；§C L103（"not a mandatory ten-item chant"）；§F L218–219（"Exploratory work stays light. It does not automatically trigger idea-evaluation, evidence-verification, or an independent Reviewer"）；Habits L257–258。
- `idea-and-mechanism.md` L9–11（"Skip for routine seeds, small ablations, bugfixes, and simple replications"）；§B L59–61（cheap exploratory probe 不是 Core Idea replacement）。
- `experiment-thinking.md` §D L166（"Not every EXP needs all eight"）；§H L329–330（"A design that requires every control in §D on a sanity EXP is a process failure, not rigor"）；Using L335–339（"For an ordinary exploratory probe, A plus an honest baseline may be enough"）。
- `evidence-and-claim.md` L11–12（"Do not load for every exploratory sanity metric"）；§D L165–167（"light existence + this EXP / this commit check is enough; do not expand this list into a universal gate"）；Using L293–294。
- `deep-literature-mode.md` L19–23（"OPTIONAL EXPENSIVE… not default literature"）；L25–28（skip 条件）；Stop/cost L264–269；反模式 L280（"Running this mode on every Open Gap"）。
- `skill-evolution.md` §C L132–134、§E L195–198：把"普通 exploratory EXP 必须保持轻"写成 canonical protection case，并把"用开满所有 gate 来修 failure"定义为 **Reject**。

"must" 语句均限定于 *新 Core Idea*（`idea-and-mechanism.md` §A、§F "Excluded is mandatory" L177）、*confirmatory EXP*（`experiment-thinking.md` §A L37–40 有 exploratory 例外）、*高风险验证报告*（`evidence-and-claim.md` §F）、*deep 模式*（`deep-literature-mode.md` §A）或 *框架改动*（`skill-evolution.md`），没有一条对所有 EXP 生效。

限制：以上是静态阅读判断；真实是否"变重"要靠 Wave E/F 的 protection fixture 跑出来。目前 `cases/` 只有 `.gitkeep`。

### Q5. 是否对 ML / systems / network security 通用，而非单一小领域？

**判定：是，三域例证均有；`deep-literature-mode.md` 两处偏向网络安全，属平衡性小瑕疵。**

- ML：F1、representation learning 的 capacity / tuning budget、seed、augmentation、widths（`scientific-reasoning.md` L49、L124–125；`experiment-thinking.md` L128–133、L292）；医学影像 patients/slices、hospital/site（`experiment-thinking.md` L94、L101、L105）。
- Systems：latency、measurement method、workload mix、requests/services/tenants、scheduling/routing（`scientific-reasoning.md` L49、L125；`experiment-thinking.md` L102；`deep-literature-mode.md` L95）。
- Network security：detection rate、pcap/capture/session/site、identity leakage、CIC / campus / encrypted backbone、inline latency（`experiment-thinking.md` L88–92、L100、L103–106；`idea-and-mechanism.md` L117–119；`deep-literature-mode.md` L94–102、L174–176、L183–185）。
- 偏向：`deep-literature-mode.md` §B L94–103 的八行映射有七行是流量/pcap/CIC 例子；§D L174–177 的矛盾图示例只有安全场景。→ N7。

### Q6. 是否有明显错误的因果 / 统计原则？

**判定：未发现明显错误；两处措辞精度可改进。**

核对为正确的关键原则：Status ≠ Outcome、technical failure ≠ negative finding（与 Protocol 一致）；Observation ≠ Interpretation、Prediction ≠ Evidence、Correlation ≠ Mechanism（`scientific-reasoning.md` L40–44）；post-hoc slice / lucky seed 不能冒充预注册预测（§F L204–216）；pseudoreplication 与 grouping unit 进 split（`experiment-thinking.md` §B L79–81、L108–113）；confounder 要在设计期均衡而非事后修正（§C L140–147）；controls 八类定义与"选控制看 rival 不看完整性"（§D）；prediction matrix 各格解释（§E：`Full ≈ Mechanism-off > Baseline → 共享设计`，`Oracle ≫ Full → headroom 非证伪`，`Full ≈ Baseline → 本测试下无可检出贡献` 均带条件限定）；oracle 是上界不是 Evidence（L185）；ablation 优先 remove / replace / sham 而非 sweep（§G）；existence ≠ valid ≠ criterion ≠ claim（`evidence-and-claim.md` §A）；不跨异质人群/指标平均效应量（`deep-literature-mode.md` §D L182–185）；null / negative 结果同样有 rival（`scientific-reasoning.md` L133–137）。

两处精度问题（非错误）：
- `scientific-reasoning.md` L53–55 "A mechanism names the process that should still produce the effect if **the correlate** were removed or scrambled" — "the correlate" 可被读成"所主张的原因本身"（此时移除它效应应当消失）。意图应是"移除仅相关的伴随量后效应仍在；移除所主张的原因后效应消失"。→ N3。
- `scientific-reasoning.md` §E L168–185 强度阶梯把 "direct discriminating experiment" 排在 "replicated controlled evidence" 之上，而 L193–194 又说取"最弱的必要档"。判别力与重复性是两条正交轴，单次未重复的判别实验不应自动高于已重复的受控证据；建议一句话点明"两轴分别取弱"或把顶档写成 "replicated discriminating"。→ N4。

### Q7. 是否真正提升判断力，而非只增加 token？

**判定：主体是可操作的判断算子，提升明显；但有可压缩的重复，约 10–15%。**

提升判断力的具体算子（每个都是现有 Skills 没有、且不可由 Protocol 推出的问题）：
- "Which claim kind currently has the weakest direct evidence?"（`scientific-reasoning.md` L84）+ "If the weakest kind is Problem/Observation, a new module will not fix it…"（L93–96）。
- "Name the **best** rival… Skip rivals that cannot generate *this* pattern"（§C L123–127）；"What result would make me *lower* belief?"（§D L145）。
- 机制"戏服"六型表（`idea-and-mechanism.md` §C L92–99）、deletion test（§G L191）、"Excluded is mandatory"（§F L177）、fatal-first 六项（§D）。
- 四个 unit 与 "10k flows from 3 captures → experimental unit closer to 3"（`experiment-thinking.md` §B）；"Pick controls from the rival, not from a completeness fetish"（L171–179）；"Cut is a success"（L246）。
- 四层不等式（`evidence-and-claim.md` L25–30）与 mismatch 清单（§E L177–189）。
- "Mixed is a map, not a mean"（`deep-literature-mode.md` L158–159）；novelty threat 要 methods-checked（L213–215）。
- protection case 概念与 "fixing the failure by enabling every gate → Reject"（`skill-evolution.md` §E）。

token 成本侧：Layer 2 共 1,739 行，超过 Protocol 五文件（612 行）+ 十个 SKILL.md（≈1,121 行）之和的 ~1.0×；按需加载可接受。可压缩项：
- Protocol 免责声明重复：`rg` 统计含 "not Protocol / not Outcome / not Verdict / do not copy / not a new state file" 等短语的行数 = deep-literature 3、evidence-and-claim 7、experiment-thinking 3、idea-and-mechanism 4、scientific-reasoning 6、skill-evolution 3（合计 26 行，另有多段前后呼应的 "Using this file / Do not / Anti-patterns" 与文件头重复）。每文件保留"文件头一次 + 定义相似词汇处一次"即可。→ N6。
- 四动作三处释义（B1）、两份 rival/confounder 列表（N5）。

### Q8. 是否有大段复制的上游 skill 文本？（deep-research 为 CC-BY-NC-SA — 只能取 idea）

**判定：无。机械与结构两层证据均为零复制。**

- **机械：** 六文件 vs `deep-research/**/*.md`（SKILL + 7 个 references）+ `idea-evaluator/**/*.md`（SKILL + 11 个 references）：共享 **8-gram = 0**；共享 6-gram 仅 1 个，为许可证字串 "cc by nc sa 4 0"（`deep-literature-mode.md` L12）。六文件 vs ARIS `experiment-plan/SKILL.md`：6-gram / 8-gram 均为 0。
- **结构（deep-research, NC-SA）：** 上游的 MECE taxonomy、六 gate 表（Angle/Coverage/Citation/Taxonomy/Calibration/Weaving）、五级引用 verdict（VERIFIED/MINOR/MAJOR/UNVERIFIABLE/PAYWALL）、hedge ladder（Strong/Medium/Weak/No evidence + 动词表）、survey-paper 输出骨架、"[unconfirmed: …]" 标记，在 `deep-literature-mode.md` 中**全部不存在**。保留的只是抽象想法：先冻结问题、多角度、核对引用深度、找会杀死 novelty 的论文；文件头 L12–17 自行声明了这一许可证姿态并列出了所取想法。引用深度四档（metadata/abstract/full text/methods checked）是本地词汇并映射到既有 `Access` 值，不是上游五级表。
- **结构（idea-evaluator, CC-BY-4.0）：** 上游的 Higher/Faster/Stronger/Cheaper/Broader 五维打分、F1–F10 十项 fatal flaw 与 CRITICAL/MAJOR/MINOR 升级逻辑、Strong Accept / Accept with Revisions / Reject and Pivot、lifecycle-capability 匹配、paradigm-shift 四问，在 `idea-and-mechanism.md` 中**全部不存在**；L137–139 明文拒绝导入外部打分分类。仅"在投入前先做 fatal-first 审计"这一想法被重述，`docs/design/research-intelligence-sources.md` L15、L24 已给出 CC-BY 归属。
- **skill-evolution 的上游（skill-doctor / skill-upper / experience-to-skill）：** 本机 `~/.agents/skills`、`~/.claude/skills`、`~/.codex/skills`、`~/aris_repo` 下均不存在同名目录，无可复制之物；文件 L23–24 声明未复制。

### Q9. Source registry 对 Wave A 的声明是否足够完整？

**判定：许可证姿态完整（每行 "Copied text? no" 与 Q8 机械结果一致）；文档完整性有四处可补，均非阻塞。**

- (a) `deep-research` 行（L17）"Concept learned" 只列了"冻结问题、多角度、核对引用"，但 `deep-literature-mode.md` §D 的"矛盾按条件定位、不平均"与上游 SKILL.md L115–116（"contradictions are presented with condition analysis, never averaged away"）同一想法；应补记（作为 common evidence-synthesis practice 亦可，但要写明）。
- (b) L12–15、L17–21 的 "Local adaptation" 列仍写 "(`X.md` when Wave A lands)"；Wave A 已落盘，应改为实际路径 `.agents/references/research-intelligence/X.md`。
- (c) 标注 "unknown / not copied" 的行（AutoSciRub L20、skill-doctor/experience-to-skill/skill-upper L21、grilling L22）没有定位信息；应至少记 "2026-09-04 本机磁盘不存在同名 skill，仅凭概念记忆"，否则审计不可复现。
- (d) ARIS 行（L19）写 "ARIS skill files were not opened for quotation"，可补上磁盘路径 `/Users/herxanadu/aris_repo/skills/experiment-plan/SKILL.md` 与本审核的 0-overlap 结果作为旁证。
- 可选：为 `scientific-reasoning.md` §E 强度阶梯与 §G 措辞表加一行 "common scientific-writing practice（claim strength ≤ evidence strength）；非取自 deep-research hedge ladder（文本与结构均不同）"，预防后续审计误判。

### Q10. 有没有应当删除而非增加的内容？

**判定：有，且都是小项；无整段需要删除。**

1. `idea-and-mechanism.md` §D L126–129 与 `deep-literature-mode.md` §F L238–243 的四动作重复释义 → 删为指向 §H 的一句引用（B1）。
2. `skill-evolution.md` L132–134、L197–198 "Case 10 in the V0.2 suite" — 磁盘上 `docs/validation/research-intelligence/cases/` 只有 `.gitkeep`，全仓库无 "Case 10" 定义；陌生 Agent 无法从盘上解析。L11 "13th scientific Skill" 与 L237 "thirteenth anything" 与冻结的 10 个 Skill 不符（同文件 L237 前半、registry L21 都写 "eleventh/11th"）。→ B2。
3. `skill-evolution.md` L18–20（"Wave D will name `session-diagnosis`, `skill-evolution`, `regression-eval`"）、L240–242（"Wave H may dogfood…"）与 `deep-literature-mode.md` L20–21（"Wave D may name an explicit light|deep switch"）：计划期 Wave 字母写进了长期 reference。→ N1。
4. 26 行 Protocol 免责声明中的重复部分 → N6。
5. `.agents/references/research-intelligence/.gitkeep`：目录已有六个实文件，提交时删除。→ N9。
6. `graphify-out/`（2026-09-03 工具残留，早于 Wave A，未跟踪）不得随 Wave A 提交；仓库无 `.gitignore`。→ N9。

---

## 3. Verdict

**`TARGETED_REVISION`**

理由：Protocol 零分叉、零复制、科学原则无错、轻量保护到位、冻结核心完好——六文件作为 Layer 2 基础**可用**。但存在两处违反框架自身纪律（单一定义处、仅凭磁盘可重建）的具体缺陷，修复量合计约十几行，且都位于 Wave B 将要锚定的段落；应在 Wave B 把 Skills / prompts 链到这些 § 之前修掉，而不是链上之后再改。

---

## 4. Blocking（Wave B 开始前必须完成）

### B1. 四动作词汇单一 owner（`idea-and-mechanism.md` + `deep-literature-mode.md`）

- **owner 定为** `idea-and-mechanism.md` §H L229–234（保留此块为唯一释义）。
- `idea-and-mechanism.md` §D L124–129：改为 "A single fatal flaw → do not ADVANCE; pick REVISE / PARK / ABANDON per §H"，删去三条 "if…" 释义（或把 §D 中区别于 §H 的判据——"flaw is fixable"——并入 §H 的 REVISE 一行，保证只剩一处）。L131–135 的 "not Protocol" 声明可保留一句。
- `deep-literature-mode.md` §F L238–246：把代码块改为一句 "Recommend one action per `idea-and-mechanism.md` §H"；文献特有的触发条件（"no real axis → REVISE or PARK；mechanism/information difference + discriminating test → ADVANCE"）以散文写在其后，不再给动作下定义；同时修正"列三说四"的计数（L245）。
- 验收：`rg -n "ADVANCE|ABANDON|\bPARK\b" .agents/references/research-intelligence/` 中带 "—" 或 "if" 释义的行只出现在 `idea-and-mechanism.md` §H。

### B2. `skill-evolution.md` 悬空 / 错误引用

- L132–134 与 L197–198：删除 "Case 10 in the V0.2 suite" 这一磁盘上不存在的标识，改为自足描述，例如 "the *ordinary exploratory EXP stays light* protection case（fixture to be added under `docs/validation/research-intelligence/cases/`）"。若 V0.2 case 编号确需保留，必须同时在 `docs/validation/research-intelligence/cases/` 落一个可读的占位说明文件，使编号可从磁盘解析。
- L11 "Not a 13th scientific Skill" → "Not an eleventh Skill"；L237 "and it does not authorize a thirteenth anything" → 删去或改为与 10 个 Skill 一致的表述。
- 验收：`rg -n "Case 10|13th|thirteenth" .agents/references/research-intelligence/` 零命中；`ls docs/validation/research-intelligence/cases/` 若被引用则非空。

---

## 5. Non-blocking（可在 Wave B–H 任一阶段或 V0.2 发布审计时处理）

- **N1** 计划期引用出长期文件：`skill-evolution.md` L18–20、L240–242；`deep-literature-mode.md` L20–21。把 "Wave D / Wave H / `session-diagnosis` / `regression-eval`" 改为 "a future `framework-maintenance` mode" 之类不带 Wave 字母的表述，或移入 `docs/design/`。最迟 V0.2 release audit 清零。
- **N2** 词面撞车：`evidence-and-claim.md` §F `supports / contradicts / invalid evidence / insufficient evidence` 与 Outcome 的 `supports / contradicts / invalid / inconclusive`；`scientific-reasoning.md` §G `inconclusive / contradicted / invalid`。现有护栏（L213–220、L242–246）已足够明确；**建议先不改**，待 Wave E/F 的 fixture 观察是否出现"把报告标签抄进 Outcome 列"的实际失误，若出现再把报告标签加前缀（如 `criterion-supported`）。
- **N3** `scientific-reasoning.md` L53–55 机制句改为双向：移除**仅相关的伴随量**效应应仍在；移除**所主张的原因**效应应消失。
- **N4** `scientific-reasoning.md` §E L168–185：加一句 "判别力与重复性分别取弱档"，或将顶档改为 "replicated discriminating experiment"。
- **N5** `experiment-thinking.md` §C L123–137 可缩为"指向 `scientific-reasoning.md` §C + 设计期增量（equalize before the run）"，减约 10 行。
- **N6** 免责声明压缩：每文件保留文件头一次 + 定义相似词汇的 § 内一次；目标每文件减 10–15%，不删任何 operator。
- **N7** `deep-literature-mode.md` §B L94–103 加一行 systems 或 ML 映射；§D L174–177 补一个非安全域示例，使 L92 的三域声明成立。
- **N8** Registry（`docs/design/research-intelligence-sources.md`）：按 Q9 (a)–(d) 补记；此文件不属六个 Wave A 文件，由 registry owner 处理。
- **N9** 提交卫生：删除 `.agents/references/research-intelligence/.gitkeep`；`graphify-out/` 不入库（加 `.gitignore` 或删除）。
- **N10** "Story's six segments live only in `state-files.md`"（`scientific-reasoning.md` L10、`evidence-and-claim.md` L8、`skill-evolution.md` L8）改为与 `state-files.md` §单一事实来源 一致的 "`state-files.md` §STORY.md + `story-maintenance`"。

---

## 6. 本审核未覆盖

- 未运行任何行为 fixture（`cases/` 为空），Q4 / Q7 是静态判断。
- 未复核 `v0.1` / `v0.1.1` 标签 peeled SHA（BASELINE.md 已记录，非 Gate A 任务）。
- 未评估 Wave B–H 计划本身；本文件只判断六个 Layer 2 文件 + registry 是否可作为后续 Wave 的基础。
