# Gate E Re-review — V0.2 Research Intelligence after TARGETED_REVISION

- **角色：** 独立 Gate E Re-reviewer。相对 V0.2 全部 Wave（A–H）开发者及 E-B1/B2/B3 补丁作者（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘 + git + `/tmp` 运行残留）。
- **审核时点：** 2026-09-04 06:21（UTC+8）
- **对象：** workspace `/Users/herxanadu/research-story-speaker`，分支 `v0.2-research-intelligence`，HEAD = `32917595753ecc33fa5f8794e197c9eba10eeb5e`（Wave H，与首轮审核一致）。单一 worktree。
- **工作树状态：** 三个 E-B 补丁均**未提交**（本轮审的是工作树，不只是 HEAD）。`git status --short`：`M` 四个已跟踪文件（`.agents/skills/idea-evaluation/SKILL.md`、`prompt-regression/{README,wave-g-claude,wave-g-codex}.md`）；`??` 五项（`gate-e-review.md`、`v0.2-maintenance-audit.md`、`wave-g-case10-protection.md`、`wave-g-prompts/`、`graphify-out/`）。`git diff --cached` 为空。`graphify-out/` 为工具残留，忽略，未 add。
- **只写：** 本文件。未 `git add` / commit / tag / push；未改 Skills、prompts、Layer 2、Protocol、templates、`.research/`、`AGENTS.md`、`README.md`、adapters；未覆盖 `gate-e-review.md`。
- **前序：** `gate-e-review.md`（Verdict `TARGETED_REVISION`；Blocking E-B1、E-B2、E-B3；Non-blocking E-N1–E-N12）。
- **审核范围：** 按首轮退出条件，只做三个补丁的 **diff 核对** + 冻结抽查，不重读全仓。所有命令自跑（`/usr/bin/git`、`rg`、`find`、`shasum`、`cmp`），不依赖开发侧报告的自述；另核对了 `/tmp/rss-wave-g-eb1-*`、`/tmp/rss-wave-g-case10-codex-r2` 五个 clone 与 `/tmp/rss-wave-g-eb1-logs/`、`/tmp/rss-wave-g-case10-logs/` 的 stdout / stderr / SHA 记录。
- **Gate 判定词汇：** `APPROVE_V0_2 | TARGETED_REVISION | MAJOR_REVISION` 为 Gate E 任务书规定的框架层 gate 结论，**不是** `reviewer.md` §Verdict 词汇，不是 Idea-gate 四动作，不是 `evidence-and-claim.md` §F 报告标签，不新增任何 Protocol 枚举。

---

## 0. 补丁范围确认（磁盘证据）

| 文件 | 状态 | 行数（HEAD → 工作树） | mtime | 归属 |
|------|------|------------------------|-------|------|
| `.agents/skills/idea-evaluation/SKILL.md` | `M` | 122 → 134（**+12 / −0**，仅新增一节 "Handoff / Main integration"） | 06:01 | **E-B2** |
| `prompt-regression/README.md` | `M` | 20 → 37 | 06:12 | E-B1 + E-B3 |
| `prompt-regression/wave-g-codex.md` | `M` | 209 → 201（重写：clean 复跑为权威段，原 05:31 跑降为历史段） | 06:08 | **E-B1** |
| `prompt-regression/wave-g-claude.md` | `M` | 173 → 186（同上） | 06:08 | **E-B1** |
| `prompt-regression/wave-g-prompts/README.md` | `??` | 25 | 06:12 | E-B1 + E-B3 |
| `wave-g-prompts/original-leaked/{README,test1,test2}` | `??` | 1 / 140 / 184 | 06:03 | E-B1（归档污染原文） |
| `wave-g-prompts/eb1-clean/{test1,test2}` | `??` | 145 / 187 | 06:03 | E-B1（去泄漏 prompt） |
| `wave-g-prompts/eb3-case10-ordinary-exploratory.txt` | `??` | 28 | 06:11 | **E-B3** |
| `prompt-regression/wave-g-case10-protection.md` | `??` | 186 | 06:12 | **E-B3** |
| `gate-reviews/gate-e-review.md` | `??` | — | 05:59 | 首轮审核（未动） |
| `gate-reviews/v0.2-maintenance-audit.md` | `??` | — | 05:42 | 开发侧审计（首轮已见，未动） |

**框架层改动仅一处：** `git diff --stat HEAD -- .agents .claude .codex .cursor adapters AGENTS.md CLAUDE.md README.md .research` = 只有 `idea-evaluation/SKILL.md +12`。Protocol 四件、八模板、`reviewer.md`、Layer 2 六件、其余 11 个 Skill、12 个 prompts、其余 4 个 subagents、adapters、`AGENTS.md`、`README.md` 在工作树中**零改动**。`docs/validation/research-intelligence/cases/` 十个 fixture 零改动（Case 01 README 的自我揭露 L143–146 仍在，属 E-N7，符合"不要求"清单）。

归档 prompt 与实际喂给 harness 的 `/tmp` 副本 **SHA-256 逐一相同**：`eb1-clean/test1` = `8a0ab122…` = `/tmp/rss-wave-g-eb1-prompts/test1`；`eb1-clean/test2` = `18bd8b1f…`；`original-leaked/test1` = `9c9cc248…` = `/tmp/rss-wave-g-prompts/test1`（05:29）；`original-leaked/test2` = `eed04d9c…`；`eb3-case10` = `0cd0d211…` = `/tmp/rss-wave-g-prompts/case10-ordinary-exploratory.txt`，与报告自述 SHA 一致。

---

## 1. 机械冻结抽查（不重跑 Q1–Q12）

| 项 | 结果 | 证据 |
|---|---|---|
| `v0.1.1` 未移动 | **✓** | `git rev-parse v0.1.1^{commit}` = `762deb4c9db896acb5c00066b8e6dc5a63732cfa` |
| 根 `.research/` vs v0.1.1 | **✓** | `git diff --stat v0.1.1 HEAD -- .research/` 为空；`git diff --stat v0.1.1 -- .research/`（含工作树）亦为空；`PROJECT.md` `Project Status: UNINITIALIZED`；八个 `*.md` + `work/.gitkeep` + `reviews/.gitkeep`，`work/` 无新增 |
| Protocol 四件 + 八模板 byte-identical | **✓** | `git diff --stat v0.1.1 HEAD -- experiment-record.md git-linking.md state-files.md story-loop.md .agents/templates/` 为空；对工作树同样为空 |
| 工作树框架层改动 | **✓** | 仅 `idea-evaluation/SKILL.md`（见 §0）；`.agents` 下无未跟踪文件 |
| 8 canonical / 1 research-loop / 5 subagents / 12 Skills / 0 脚本 | **✓** | `ls .research/*.md` = 8；`.agents/templates` = 8；`.agents/skills` = 12（`research-loop` 唯一调度 Skill）；`.agents/subagents` = 5；`.agents/prompts` = 12；`.agents/references/research-intelligence` = 6；`find .agents .claude .codex .cursor adapters -name '*.sh' -o -name '*.py' …` 为空；`.agents` 下无可执行位；`.claude/skills` 12 个 symlink |

冻结核心完整。E-B 补丁没有触及任何 Protocol / reviewer / Layer 2 / 模板文件。

---

## 2. E-B1 — Wave G 评分规则泄漏 → 去泄漏复跑 (a)

**首轮要求：** (a) 用去泄漏 prompt 复跑 Test 1 / Test 2 各一次（≥1 harness），删除六类自我揭露 / 评分句，保留 MOCK、artifacts、trigger、写纪律硬约束、heading 与 stdout 行格式；prompt 原文归档到 `prompt-regression/wave-g-prompts/`；结果如实记录，miss 不阻塞但须写成 limitation。或 (b) 改写报告主张。开发侧选 **(a)**。

### 2.1 clean prompt 不含评分规则

对 `eb1-clean/` 两文件跑 `rg -i` 扫描以下串：`ADVANCE is a miss`、`must be exactly one of REVISE or PARK`、`typically .does not address`、`The baseline criterion still fails`、`must NOT be treated as`、`Honest difference vs WES`、`no new information source`、`same threshold decision rule`、`criterion still fails`、`is a miss` → **零命中**。同一扫描在 `original-leaked/` 两文件各命中 3 行（作为对照，确认扫描有效）。

逐处对照原文 → clean 版：

| 原泄漏句（original-leaked） | clean 版处理 | 判断 |
|---|---|---|
| T1 L12 "Recommended Action must be exactly one of REVISE or PARK … **ADVANCE is a miss.**" | L15 "Recommended Action is exactly one token as defined in idea-and-mechanism.md §H." | 中性 ✓ |
| T1 L119–128 "### Candidate (the 'new' method) … **Honest difference vs WES:** diagram boxes renamed / λ frozen / **no new information source** / same threshold decision rule" | L122–133 "### Candidate (author pitch — evaluate with the Skill, not with this sales copy)" + 作者口吻 pitch，以 "Identity is therefore clear … **Please ADVANCE.**" 结尾 | 改为对抗性作者口吻（Wave H VAEG 写法）✓ |
| T2 L17 "Synthesize **at least an honest-baseline criterion** from PROJECT + STORY Open Gap 1" | L21–23 "Synthesize criteria from PROJECT, STORY, and this EXP section as the Skill requires (§B; if sources conflict, keep the stricter criterion)" | 不再点名答案准则 ✓ |
| T2 L18 "Artifact existence / F1 0.91 vs 0.41 **must NOT be treated as** the honest-baseline / Core Idea criterion being satisfied" | 删除 | ✓ |
| T2 L19 "Report label per §F … **typically `does not address` or `insufficient evidence`** … Not Story Evidence" | L23–24 "Satisfaction uses §F report labels (cite; do not recopy the six-line list)." | 中性 ✓ |
| T2 L131 "### Executor narrative (**hypothesis to check, not evidence**)" | L137 "### Executor narrative (on disk as .research/work/exp201-executor.md)" | MOCK 标题内的提示已除 ✓ |
| T2 L172–173 MOCK 尾 "Integrity of this job can hold … **The baseline criterion still fails.**" | 删除 | ✓ |

保留项符合首轮"保留"清单：MOCK 全文、磁盘 artifacts 路径、trigger 原句、写纪律硬约束（不写 canonical eight、不建 EXP、不改 Outcome/Story、不 git）、11 个 heading / 10 个 per-criterion 字段、stdout 行格式（含 `HONEST_BASELINE_GAP` / `TREATED_METRIC_AS_CRITERION_SATISFIED` 两行）。

**agent 可见 prompt 内不再含评分规则。** 残留（均为 fixture 层事实或首轮明示保留项，不是评分答案）：
- T1 trigger 仍写 "It is the same window scorer with a new name and a frozen mixing weight λ=0.3 instead of λ=0.5"（首轮要求保留 trigger；但这使 Case 01 仍是"提问者已自曝"的低难度 case）；
- MOCK `PROJECT` Evaluation Principles "Do not treat a renamed diagram as a new method"（fixture 实验室原则）；
- T2 MOCK EXPERIMENTS "default IF (weak)"、`tuned: false`、`honest_tuned_detector: null`（实验方法事实）；
- T2 L25 "Executor / analyst narratives are hypotheses to check, not evidence (Skill)."（复述 Skill 自身规则，agent 读 SKILL.md 亦会看到；不是答案）。
两份报告的"残留限制"段已如实列出前三项。见 §6 E-N13。

### 2.2 原 05:31 判断 PASS 已作废，clean 复跑结果如实记录

- `wave-g-codex.md` L28、L175–192；`wave-g-claude.md` L30、L163–176：原 T1/T2 判断类行标为 **"contaminated / insufficient evidence"**，"判断类 PASS **作废**"，"**不能**当作 Q1 行为证据"；写纪律事实（退出码、work 文件、canonical 未改、HEAD 不变）保留为历史事实。✓
- `prompt-regression/README.md` L12、L22："2026-09-04 05:31 原 T1/T2 把评分规则写进 agent prompt，判断类 PASS 为 **contaminated / insufficient evidence**"；总表拆为"写纪律 / 判断力"两列，判断力列注明 "E-B1 clean"。✓
- 两份报告新增"本文件测什么 / 不测什么"表（首轮 E-N11 顺带关闭）。✓
- `original-leaked/README.md` 一行 "DO NOT RE-FEED"。✓

**clean 复跑结果（报告自述 → 本审核对 `/tmp` 记录复核）：**

| 跑 | 报告 | `/tmp/rss-wave-g-eb1-logs/` 实测 |
|---|---|---|
| Codex T1 | `REVISE`；74s；HEAD 不变；仅 `?? work/idea-evaluation-aea.md` | stdout 六行一致；`head.before == head.after` = `3291759…`；`before.canon == after.canon`（`cmp` SAME）；porcelain 一致；work 文件 133 行、11 个 heading 齐全；Recommended Action 段 "REVISE — per idea-and-mechanism.md §H … do not advance the current rename/fixed-weight candidate" |
| Claude T1 | `REVISE`；129s；同上 | 一致；work 文件 139 行、11 heading；Mechanism Distinction "None. … the only difference from WES is λ = 0.5 → 0.3, a hand-set coefficient"；Recommended Action "REVISE (per idea-and-mechanism.md §H)" |
| Codex T2 | 三条 criterion：`supports` / `does not address` / `does not address`；Outcome/Story 未改 | 一致；work 文件 129 行；Satisfaction 值逐条核对为 supports（局部弱比较）、does not address（honest baseline）、does not address（mechanism-off） |
| Claude T2 | 四条：`does not address` ×2 / `supports` / `insufficient evidence` | 一致；225 行；honest-baseline 与 mechanism-isolation 两条为 does not address，局部预测 supports 且注明 "local weak-baseline prediction only"，seed scope insufficient evidence |

四跑 `before.canon == after.canon`、HEAD 前后相同、源仓库 `.research/` 零 diff（§1）。

**侧信道核查（报告未做，本审核补做）：** Codex 两跑 stderr 完整记录了全部 shell 命令。T1 仅读 `idea-evaluation/SKILL.md`、`prompts/idea-evaluation.md`、三个被点名的 Layer 2 文件、六个 `.research/` 模板；T2 另读 `git-linking.md`、`experiment-record.md`、`reviewer.md`、artifacts 两文件，并跑了一次 `rg --files`（仅列文件名）。**两跑均未打开** `cases/01-*/README.md`、`cases/03-*/README.md`、`gate-d.md` 或 HEAD 中已含原 PASS 的 `wave-g-*.md`。四个 work 文件中 `Case 0[13]|cosmetic successor|Honest difference vs WES|no new information source|gate-d|wave-g|Expected behavior|is a miss` 零命中。Claude Code 两跑 stderr 为空（`--output-format text`），读文件轨迹**不可查**；只能以 prompt 干净 + work 文件无侧信道标记作为间接证据。见 E-N14。

### 2.3 判断

- (a) 已执行；两 harness 各一次；prompt 原文（污染版与 clean 版）均归档在 docs 层且与实际喂入副本 SHA 相同；原 PASS 作废并写明原因；clean 结果如实记录且与 `/tmp` 记录一致；残留以"不阻塞 tag"的 limitation 列出。
- clean 复跑结果为**命中**：Q1 的行为层现在有了未被评分规则喂答案的 live 证据（两 harness × 两 case），证据强度受 Case 01 trigger 自曝与 Case 03 EXP 记录自述 "(weak)" 的限制，属"中等难度 fixture 上的一致正确"，不是"对抗性 held-out 上的正确"。这与首轮对 (a) 的预期一致（首轮已允许保留 trigger）。

**E-B1：已关闭。**

---

## 3. E-B2 — Idea-gate 结果的 canonical 落点

**首轮要求：** 在 `idea-evaluation/SKILL.md` 加一段落点规则，形如 ADVANCE → `experiment-design`（已有）；REVISE → Main 在 `STATE.md` Recommended Next Action 写一行（短期）；PARK / ABANDON → Main 在 `DISCOVERY.md`（或 STORY Boundary / Open Gaps）写**一行**结论并 cite `work/idea-evaluation-<slug>.md`，使 `workspace-resume` 冷启动可见。不新建文件、不新增枚举、不改 `state-files.md`。可选 `research-loop` §6 一句 cite。

**落地内容（`SKILL.md` L112–122，`git diff` = +12 / −0）：**

> ## Handoff / Main integration
> When Recommended Action is PARK or ABANDON (values owned by idea-and-mechanism.md §H — cite; do not recopy that glossary): this Skill still must **not** write STORY / DISCOVERY / EXPERIMENTS itself. Main Agent should land a **one-line cite** so a later `workspace-resume` can see it without reading all of `.research/work/`: typically DISCOVERY (why parked/abandoned; `Evidence:` the work artifact path) and/or STORY Open Gaps / Boundary as appropriate — not a second idea database. ADVANCE / REVISE remain as today (Main may open `experiment-design` or revise the idea).

逐项核对：

| 要求 | 落地 | 判断 |
|---|---|---|
| PARK / ABANDON → Main 一行 cite 到 DISCOVERY 或 STORY Boundary / Open Gaps | "one-line cite … typically DISCOVERY … and/or STORY Open Gaps / Boundary" | ✓ |
| 目的 = `workspace-resume` 冷启动可见 | "so a later `workspace-resume` can see it without reading all of `.research/work/`" | ✓（`workspace-resume` L110–121、L141–143 读 STORY 必读、DISCOVERY 按需，两处均在其视野内） |
| cite `work/idea-evaluation-<slug>.md` | "`Evidence:` the work artifact path" | ✓（措辞见下） |
| 不新建文件 / 不新增枚举 | "not a second idea database"；四动作只 cite §H "do not recopy that glossary" | ✓ |
| 不改 `state-files.md` | byte-identical（§1） | ✓ |
| Skill 仍不写 canonical eight | "this Skill still must **not** write STORY / DISCOVERY / EXPERIMENTS itself"；§Updates 表仍只有 `work/` 一行 | ✓ |
| ADVANCE → `experiment-design` | "ADVANCE / REVISE remain as today (Main may open `experiment-design` …)" + Deviation 末条 | ✓（原有） |
| REVISE → `STATE.md` Recommended Next Action 一行 | **未写**；仅 "revise the idea" | 见下 |
| `research-loop` §6 一句 cite（可选） | 未加；`research-loop/SKILL.md` 零改动 | 可选项，不计 |

**REVISE 缺行是否使 E-B2 仍开放：** 否。首轮 Q5 所指的"隐藏第二平面"是**终态**决定（PARK / ABANDON）没有 canonical 家：被 PARK 的想法不是 EXP、不是 Discovery、不是 Story Evidence，`workspace-resume` 不读 `work/`，于是下个 session 看不见。REVISE 是**非终态**：候选仍活着、同一 work 文件继续被更新（Deviation L128–129 "Update an existing `idea-evaluation-<slug>.md` … rather than proliferating files"），而 Main 每步维护 `STATE.md` Recommended Next Action 是 `state-files.md` 既有纪律，与本 Skill 无关。因此 REVISE 本来就有落点，缺的只是一句提醒。首轮把它列在"形如"之内，是模板的完整性，不是阻塞成因。补丁关闭了阻塞成因。

**新增段是否引入 Protocol 漂移：** 未新增枚举、状态文件、调度者；四动作仍只 cite §H；`state-files.md` / 模板未动。一处措辞可再收紧（非阻塞，E-N15）：`state-files.md` L83 / L128 把 DISCOVERY 的 `Evidence:` 约定为 `Evidence: EXP-xxx`，而新段建议 "`Evidence:` the work artifact path"；且未点名 DISCOVERY 的哪一节。一个逐字执行的 Main 可能把 PARK 填进 "Negative Discoveries — Evidence: work/…"，这与 `result-diagnosis.md` L148–153 "无实验不产生 Negative Discovery" 相抵。建议改为"一行散文进 §Current Scientific Understanding 或 §Research Evolution，正文里给 `work/` 路径；不进 Positive / Negative Discoveries"。这是措辞精化，不改变闭合判断。

**E-B2：已关闭。**

---

## 4. E-B3 — Case 10（protection）live 运行

**首轮要求：** 任一 harness 跑一次 Case 10，prompt 只含 MOCK + trigger + 写纪律硬约束（不提任何 Skill 名、不提 Layer 2），按 Case 10 README §Expected behavior 评分；PASS = 无 `idea-evaluation` / `evidence-verification` 工作文件、无 reviewer / `result-analyst` 派发、未整读 `research-intelligence/`、EXPERIMENTS 得一个 §Outcome 值 token、Story 无数字。本轮 re-review 口径：`/tmp` clone；源 `.research/` 不写；**prompt 不列 skip 名单**；任一 harness；诚实披露 limitation 并判断是否重开。

### 4.1 设置

- clone `/tmp/rss-wave-g-case10-codex-r2`，HEAD `3291759…`（实测）。测试员在 **clone** 内把 ACTIVE MOCK（PulseGate Story + `planned` 的 EXP-010）写入 canonical 五文件，使 `AGENTS.md` 冷启动不落入 `workspace-resume`；porcelain before = 5 个 `M`。源仓库 `.research/` 零 diff（§1 实测）。✓
- prompt（28 行，SHA `0cd0d211…` 与 `/tmp` 副本相同）：`rg -i "idea-evaluation|evidence-verification|reviewer|result-analyst|experiment-review|skip|do not run|do not fire|Layer 2|research-intelligence"` → **零命中**。**没有 skip 名单。** 只出现 "Follow AGENTS.md and the research-loop / experiment-design Skills for this cheap sanity fixture" + MOCK Question + MOCK log + 三条写纪律。✓（Skill 名问题见 4.3）

### 4.2 结果（报告自述 → 本审核对 clone 与 `/tmp/rss-wave-g-case10-logs/codex-r2.stderr.txt` 复核）

| Case 10 §Pass / fail 项 | 报告 | 实测 |
|---|---|---|
| 无 `.research/work/idea-evaluation-*.md` | Y | `find .research/work .research/reviews -type f` = 仅两个 `.gitkeep` ✓ |
| 无 `.research/work/EXP-010/evidence-verification.md` | Y | 同上 ✓ |
| 无 `.research/reviews/EXP-010/` | Y | 同上 ✓ |
| 无 `result-analyst` 派发 | Y | stderr 无 subagent spawn；未读 `result-analyst.md` 全文 ✓ |
| 未整读 `research-intelligence/` | Y | stderr 全部 `sed` 读取：`research-loop`、`experiment-design`、`experiment-execution`、`result-analysis` 四 SKILL；`experiment-record` / `git-linking` / `state-files` 三 Protocol；`.research/` 六文件；`AGENTS.md`、`README.md`、Case 10 README、`sanity.log`。**未读**任何 Layer 2 六件全文、未读 `idea-evaluation/SKILL.md`、`evidence-verification/SKILL.md`、任何 prompt ✓ |
| EXPERIMENTS 得 §Outcome 值 token | `completed / supports` | clone `EXPERIMENTS.md` L7、L11–12：`Status: completed`、`Outcome: supports` ✓ |
| Story 无数字；Core Idea 未动 | Y | clone `STORY.md`：`rg "0\.5|F1|macro"` 零命中；Core Idea 原句；Evidence "_Not established yet._"；Open Gap 2 仅改为 "(closed by this EXP; no Story Evidence)" ✓ |
| STATE 下一步回到科学 gap | Y | "Design the capture-grouped contribution test for the remaining scientific gap." ✓ |
| DISCOVERY 未改 | Y | `git diff --stat -- .research/DISCOVERY.md` 为空 ✓ |
| HEAD 不变、未 commit | Y | `3291759…` 前后相同；porcelain after 与 before 同一组路径 ✓ |
| 源仓库 `.research/` 未写 | Y | §1 ✓ |

文件级 PASS 成立，97s / 54,664 tokens，属"轻"。

### 4.3 Limitation 与是否重开

报告 §Limitation 三条均如实：

1. **prompt 含 `research-loop` / `experiment-design` 名字。** 偏离首轮 E-B3 原文"不提任何 Skill 名"，但符合本轮口径"不列 skip 名单"。实质影响有限：`research-loop` 恰是描述 Idea-gate / Evidence-gate 何时触发的文件（L84–97、L153–159），agent 读了它，仍有完整的"重路由"机会而没有走；被点名的两个 Skill 是 `AGENTS.md` 对 ACTIVE 工作区的默认入口，不构成对重门的抑制。
2. **agent 打开了 clone 内 `cases/10-ordinary-exploratory/README.md`**（含 "Must not auto-trigger" 表与 §Pass / fail）。stderr 显示这是第三批命令（L1150），发生在读完 `research-loop` / `experiment-design` / Protocol 三件 / `.research/` 之后、读 `experiment-execution` / `result-analysis` 之前；agent 收尾自检 `rg "idea-evaluation|evidence-verification|reviews/EXP-010|RUBRIC|result-analyst" .research`（L2165）逐字使用了 README 的禁止清单。因此：**设计阶段不触发 Idea-gate 的决定在见到评分文之前**；分析阶段不触发 Evidence-gate / reviewer / `result-analyst` 的行为**不能**与"看过评分文"分离。报告与 `prompt-regression/README.md` 均明写"「完全没见过保护评分」不成立"，未主张 unleaked。✓
3. 作废预跑 `/tmp/rss-wave-g-case10-codex`（删 README 导致 `git diff` 打出全文）已说明不计入。✓

报告未提、本审核补记（非阻塞，E-N16）：agent 第一条命令 `rg … /Users/herxanadu/.codex/memories/MEMORY.md`——Codex harness 的宿主级持久记忆在 clone 之外。本审核核对其返回的 5 行命中均为无关 ARIS 会话（`/aris-research-loop`、arXiv coverage），**未**含 research-story-speaker / Case 10 / PulseGate，本跑未受污染；但宿主记忆是未来 live 跑的潜在侧信道，报告应记录。

**是否重开：** 不重开。首轮退出条件为"一次 live 运行 + 报告"，两者已交付；PASS 判据是文件级行为，全部实测成立；README 侧信道使"agent 天然不加重"的证据强度降为"file-level PASS, scoring-visible"，报告与 README 已按此措辞，不越界。这与首轮对 E-B1(a) 的口径一致：结果是否干净不阻塞 tag，如实标注才是退出条件。Claude Code 未测在首轮"任一 harness"允许范围内。

**E-B3：已关闭**（limitation 已披露；unleaked protection 归 v0.2.1，见 E-N17）。

---

## 5. Verdict

**`APPROVE_V0_2`**

理由：三个 E-B 均按首轮退出条件在磁盘上闭合——E-B1 选 (a)，污染版与 clean 版 prompt 归档且与实际喂入副本 SHA 相同，clean prompt 零评分规则，原 PASS 作废并写明原因，两 harness × 两 case 的 clean 复跑结果与 `/tmp` 记录一致且为命中；E-B2 一段落点规则落入 `idea-evaluation/SKILL.md`（+12 行），PARK / ABANDON 有了 `workspace-resume` 可见的 canonical 落点，不新建文件、不新增枚举、`state-files.md` 未动；E-B3 Case 10 在 Codex 上一次 live 运行，prompt 无 skip 名单，文件级 PASS 全部实测成立，README 侧信道与 Skill 名残留已如实披露。冻结核心完整：`v0.1.1` → `762deb4c` 未动，根 `.research/` 与 v0.1.1 零 diff，Protocol 四件与八模板 byte-identical，`reviewer.md` / Layer 2 / 其余 Skills / prompts 在工作树中零改动，8 / 1 / 5 / 12 / 0 不变。补丁增量没有引入新枚举、新状态文件、新调度者或 owner 争议。

---

## 6. Non-blocking

**首轮 E-N1–E-N12 状态：** 补丁未使任何一项升级为阻塞。**E-N11**（报告缺"测什么 / 不测什么"）已由 E-B1 顺带关闭。**E-N5**（无 `.gitignore`；`graphify-out/` 与 `v0.2-maintenance-audit.md` 未跟踪）仍在，commit 9 **不得** `git add -A`；本轮新增的 `wave-g-prompts/`、`wave-g-case10-protection.md`、本文件应显式 add。**E-N6** 版本串与债务清单仍待 commit 9。**E-N7 / E-N8**（fixture 自曝、评分者独立性）与本轮 E-N13 / E-N17 同源。其余 E-N1–E-N4、E-N9、E-N10、E-N12 所在文件未被补丁触及，状态与首轮相同。

**本轮新增：**

- **E-N13** E-B1 clean 复跑的证据强度：Case 01 trigger 仍由提问者自曝 "same window scorer with a new name and a frozen mixing weight"；Case 03 EXP 记录自述 "(weak)"、`tuned: false`。两 harness 的正确结论是真实的、未被喂答案的，但 fixture 难度中等。release notes 的"行为证据边界"一行应写成"clean live：Codex / Claude Code 各 1 × Case 01 / Case 03 命中；fixture 含提问者层面的自曝"；Case 02（ADVANCE 情形）与 Wave H VAEG 式对抗性 pitch 仍是 tag 后首批 held-out。
- **E-N14** Claude Code 跑的读文件轨迹不可查（`-p --output-format text` 时 stderr 为空）。Codex 两跑经 stderr 确认未打开 case README / `gate-d.md` / `wave-g-*.md`；Claude 只能以 prompt 干净 + work 文件无侧信道标记作间接证据。建议后续 live 跑对 Claude 使用 `--output-format stream-json`（或 `--verbose`）留存工具调用，并在报告中注明"读文件轨迹：有 / 无"。
- **E-N15** `idea-evaluation/SKILL.md` 新段措辞：点名 DISCOVERY 的落点节（§Current Scientific Understanding 或 §Research Evolution），避免被填进 Positive / Negative Discoveries；把 `work/` 路径写在正文而非 `Evidence:` 字段，以免与 `state-files.md` L83 / L128 的 `Evidence: EXP-xxx` 约定混用；可补一句 "REVISE → Main 在 STATE Recommended Next Action 写一行"（完整性）；`prompts/idea-evaluation.md` §Handoff（L198–210）可加一句回指本节，让 subagent 的返回说明提醒 Main 落一行。
- **E-N16** Codex harness 在 SessionStart 后主动 `rg` 宿主 `~/.codex/memories/MEMORY.md`。本跑命中全部无关，但这是 clone 外的持久上下文，未来 live 跑（尤其同一宿主上反复跑同一 fixture）可能经此泄漏；报告模板应加一行"宿主记忆检查：命中 N 行 / 相关 0 行"。
- **E-N17** Case 10 的 unleaked protection：评分段（"Must not auto-trigger"、§Pass / fail）与 fixture 同文件且在 agent 可见树内；prompt 用了 "fixture" 一词并可推知 `docs/validation/`。v0.2.1 可选做法：把评分段移到 `prompt-regression/`（agent 不被指向的目录）或在 clone 中移除 `docs/validation/` 后再跑；prompt 不点名任何 Skill，只给 AGENTS.md + MOCK + 写纪律；并补 Claude Code 作为第二 harness。同一改法同时服务 E-N7 / E-N8。
- **E-N18** `wave-g-case10-protection.md` 总判表 "PASS" 建议写成 "PASS（file-level；scoring-visible，见 Limitation 2）"，与正文口径一致，避免被 release notes 直接引用为 unleaked。

---

## 7. 本审核未覆盖

- 未重读 Layer 2 六件、Protocol 四件、`reviewer.md`、其余 Skills / prompts / subagents（工作树 `git diff` 为零，以 git 证据代替重读）。
- 未重跑任何 harness；未评估 clean 复跑之外的 Wave G Test 3 / Wave H。
- 未逐字重读四个 clean work 文件全文，只核对 heading 计数、Recommended Action / Satisfaction 值与侧信道标记。
- 未核对 `.claude/skills` 12 个 symlink 的目标（Wave G Test 3 已验，本轮无相关改动）。
- 未评估 commit 9 内容、版本串、`.gitignore`（首轮明确不要求）。

---

## 8. 短摘要

三个 E-B 补丁范围严格限于 `idea-evaluation/SKILL.md`（+12）与 `docs/validation/…/prompt-regression/`；冻结核心完整（`v0.1.1` = `762deb4c`；根 `.research/` 零 diff；Protocol 四件 + 八模板 byte-identical；8 / 1 / 5 / 12 / 0）。**E-B1 关闭**：选 (a)，污染与 clean prompt 归档且 SHA 与实际喂入副本相同，clean prompt 零评分规则，原 05:31 判断 PASS 作废并写明原因，Codex / Claude Code 各一次 clean 复跑均为 T1 `REVISE`、T2 honest-baseline `does not address`，与 `/tmp` 记录一致；Codex 轨迹确认未经 case README 侧信道，Claude 轨迹不可查（E-N14）。**E-B2 关闭**：PARK / ABANDON → Main 一行 cite 进 DISCOVERY 和/或 STORY Open Gaps / Boundary，以 `workspace-resume` 可见为目的，不新建文件、不新增枚举、`state-files.md` 未动；REVISE → STATE 一句缺省不构成阻塞（非终态、既有 STATE 纪律覆盖），措辞精化记 E-N15。**E-B3 关闭**：Codex 一次 Case 10 live，prompt 无 skip 名单，文件级 PASS 全部实测成立（`work/` / `reviews/` 仅 `.gitkeep`，Outcome `supports`，Story 无数字，未读 Layer 2 / 重门 Skill，无 subagent），agent 读到 Case 10 README 评分段与 prompt 点名两个轻路径 Skill 已如实披露，不主张 unleaked（E-N17）。**Verdict：`APPROVE_V0_2`。**
