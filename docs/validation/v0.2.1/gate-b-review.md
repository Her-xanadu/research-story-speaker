# V0.2.1 Gate B Review — micro-hardening + compact-retune (Waves 0, A–G)

- **角色：** 独立 V0.2.1 Gate B Reviewer。相对 Wave A–G / retune 实施者为 **different-family**（本审核由 Claude 执行；实施侧为 Grok / Codex `gpt-5.6-luna` / Claude Code CLI 上的 `deepseek-v4-pro`）+ **fresh-context**（无实施聊天历史；只读磁盘 + `/usr/bin/git`）。
- **审核时点：** 2026-09-04 15:12–15:30（UTC+8）。
- **对象漂移（如实记录）：** 任务书发出时 `origin/v0.2.1-micro-hardening` = `727e31b`，`origin/v0.2.1-compact-retune` = `f41d9ea` **未合并**。审核进行中（15:14:51）实施侧推送了合并提交 `38543fe`（parents `727e31b` + `f41d9ea`，无冲突标记；merge-time 仅额外改了 `README.md` +10/−4 与 `wave-g/README.md`，均为如实写入 MISS）。**本审核锁定在 `38543fe`**（= 合并后 `origin/v0.2.1-micro-hardening` = worktree `/private/tmp/rss-v021-wave0` HEAD），diff 基线 `v0.2^{}` = `577bb76`。`727e31b..38543fe` 的 diff 与 `e612f8e..f41d9ea`（retune 分支独有）逐文件一致，因此 retune 分支不再单独审，作为 `38543fe` 的一部分审。
- **工作树布局：** 主 checkout `/Users/herxanadu/research-story-speaker` 在 `v0.2-research-intelligence`（= `577bb76`），干净。被审分支 checkout 在 worktree `/private/tmp/rss-v021-wave0`（`38543fe`，porcelain 为空，无 `MERGE_HEAD`）。
- **只写：** 本文件（`docs/validation/v0.2.1/gate-b-review.md`，落在 wave0 worktree，**未** `git add` / commit / tag / push）。未改 Skills、prompts、Layer 2、Protocol、templates、`.research/`、`AGENTS.md`、`README.md`。
- **范围：** 静态审读 `v0.2..38543fe` 全部 64 文件 diff（+3472 / −321）+ Wave G 9 份 live 报告 + G3/G5 prompt 与产物；不重跑 harness。Live 数字全部取自报告原文，不做二次推算。
- **Gate 判定词汇：** `APPROVE_V0_2_1 | TARGETED_REVISION | MAJOR_REVISION`，为本任务书规定的框架层 gate 结论，**不是** `reviewer.md` §Verdict、不是 Idea-gate 四动作、不是 `evidence-and-claim.md` §F 报告标签。

---

## 0. 提交链（磁盘证据，`v0.2^{}..38543fe`）

```text
6ab7ed5  Wave 0 baseline
80d8180 / 1934c3a / 5c4f56a / 6ef093b / 8592f69   Wave A / B / C / D / F   → 5 merges
6b44c72  Wave E: meta-rule deduplication
e612f8e  Gate A: APPROVE (static)                       ← Wave G / retune 共同基线
51d99f9  Wave G: G3 Case 02 + G5 skill-evolution independence
889ce04  Wave G: Codex live (G1 / G2 / G4)
5222eaa  Wave G: Claude Code live (G1 / G2 / G4)
051e865 / 3a47290 / 727e31b   三次 merge 进集成分支
ee515fe  compact path: stop default-loading full prompts        (retune 1 Skill)
f57f34b  Wave G: Case 10 compact retune evidence (Codex 38,009)
a81f0ca  Wave A: compact skill stop-fold                        (retune 2 Skill)
f41d9ea  Wave G: Case 10 compact retune2 evidence (Codex 40,321; Claude 44,765)
38543fe  Merge compact-retune → micro-hardening (+ README 如实写 MISS)
```

---

## 1. 冻结检查（全部于 `38543fe`）

| 项 | 结果 | 证据 |
|---|---|---|
| `v0.1^{}` | **✓** | `8db3b301f5bce878d6c2ee4a61bcb234c9609c3d`；tag object `96340ad8…`；本地 = `git ls-remote --tags origin` |
| `v0.1.1^{}` | **✓** | `762deb4c9db896acb5c00066b8e6dc5a63732cfa`；tag object `90a41d5d…`；本地 = 远端 |
| `v0.2^{}` | **✓** | `577bb76f4cc7a43ae4c64a1c35456d4d53e746c1`；tag object `0bd2d2c6…`；本地 = 远端 |
| `origin/master` == `v0.2^{}` | **✓（预期）** | `577bb76`；未 FF 到 v0.2.1，这是发版前的正确状态，不作阻塞 |
| canonical state = 8 | **✓** | `.research/` 8 个 `*.md` + `reviews/` + `work/`；`git diff v0.2 38543fe -- .research/` 为空（仍 UNINITIALIZED） |
| research-loop = 1 | **✓** | 唯一 `.agents/skills/research-loop/SKILL.md` |
| subagents = 5 | **✓** | experiment-agent / literature-scout / research-lead / result-analyst / reviewer |
| skills = 12 | **✓** | `.agents/skills/*/SKILL.md` = 12；`.claude/skills/` 12 个 symlink 全部解析到存在的 `SKILL.md` |
| RI references = 6 | **✓** | 6 个 `.md`，目录内无其它对象 |
| framework scripts = 0 | **✓** | 全仓无 `100755`；`.agents/` 内无非 `.md`/`.gitkeep`/symlink；仓内 `.json/.yaml` 仅为 `docs/validation/**/artifacts/` 夹具 |
| Protocol 四件 byte-identical | **✓** | `state-files.md` / `experiment-record.md` / `story-loop.md` / `git-linking.md` 对 `v0.2` diff 为空；`reviewer.md` 仅 Gate A 已审的 +1 行 progressive-load cite，§Verdict 未动 |
| 模板 | **✓** | 仅 `REVIEWS.template.md` +1 行（Gate A 已审）；其余 7 模板未动 |
| RI / prompts / subagents / templates 自 Gate A 起未动 | **✓** | `git diff --stat e612f8e 38543fe -- .agents/references .agents/prompts .agents/subagents .agents/templates` 为空 —— **retune 只改了 5 个 SKILL.md + `AGENTS.md`** |
| `adapters/`、`CLAUDE.md`、`.gitignore` | **✓** | 不在 diff 内 |
| 历史证据未重写 | **✓** | `cases/`、`prompt-regression/`、`gate-reviews/`、`source-audit/`、`BASELINE.md`、`framework-reviews/`、`handoff-tests/`、`harness-smoke/` 对 `v0.2` diff 为空 |
| 相对链接 | **✓** | 框架层 + live-cases + `v0.2.1/` + `docs/design/` 共 331 个相对链接，0 断链（Gate A 382 → 331：retune 把 Skills 内多数 `[x](path)` 改为裸 `` `x` ``，见 N2） |
| 合并提交干净 | **✓** | 树内无 `<<<<<<<`/`>>>>>>>`；`--cc` 仅显示 merge-time 的 README 两处如实写 MISS |

---

## 2. Plan §43 十九问逐答

### 架构

| # | 问 | 答 | 证据 |
|---|---|---|---|
| 1 | 8/1/5/12/6 still? | **是**（+0 脚本、12 symlink） | §1 |
| 2 | hidden state source? | **无**。`compact` / `full` 在 Protocol 四件、8 模板、根 `.research/`、5 subagents 内 0 命中；prompts / RI 仅有 v0.2 既有的无关用法（"one compact block per criterion"）。三个 compact Skill 各自声明 "Never write them into STATE, EXPERIMENTS, Status, or Outcome"。`AGENTS.md` 的 compact 路由条件读的是既有 STATE `Recommended Next Action`，不引入新字段。Live：4 次 Case 10 运行（G1×2、retune×3）报告均写明 `compact`/`full` 未进 STATE / EXPERIMENTS / Outcome | `git grep -w compact` |
| 3 | 13th Skill? | **无**。12 个；`framework-maintenance` checklist #10 / #14 与 §Anti-patterns 三处仍写 "not a 13th Skill" | §1 |
| 4 | second orchestrator? | **无**。`research-loop` 仍是唯一 "Highest-level orchestrator. Schedules only"。`AGENTS.md` L26–32 / L53 新增的 compact 路由是**入口路由表的一行**（读已决定的 STATE 下一步 → 派 Skill），不决定科研下一步、不循环；不构成第二编排器。但它把三个 Skill 的 "do not open" 清单整段抄进入口文件，属 #14 重复（见 T4） | `AGENTS.md` L26–34, L53 |

### 效率

| # | 问 | 答 | 证据 |
|---|---|---|---|
| 5 | ordinary exploratory lighter than V0.2? | **是，但未达软目标。** 文件集：retune 三次 live 均**未**加载 `workspace-resume`、`research-loop`、`failure-diagnosis.md`、`experiment-proposal.md`、`result-diagnosis.md`、Protocol 三件、RI 目录（G1 pre-retune 时 Codex 曾加载 `workspace-resume` + 全文 `failure-diagnosis.md` + Protocol 三件；Claude 曾加载 Protocol 两件 + `evidence-and-claim.md` + 根 `README.md`）。可比 token：Codex 54,664 → 38,009 / 40,321（−30% / −26%）；Claude uncached input 50,170 → 44,765（−11%；与 Codex stderr 不是同一计量）。软目标 ≤30k 或 −40% **两 harness 均 MISS** | §3 |
| 6 | just moved text but still load all? | **一半一半，报告已如实区分。** (a) "不默认加载周边文件" 是**真实**变化，live 已证；(b) SKILL.md 内的 stop-fold 是**无效**变化——3/3 retune 运行都整文件读（Codex `sed -n '1,240p'`/`'1,280p'`；Claude `Read` 无 offset/limit），报告原文 "The stop line was in the file; the agent did not stop"；(c) 且三个 compact SKILL.md **本身变大**：experiment-design 131→226 行（7,179→10,903 B，+52%）、result-analysis 122→196 行（6,809→9,386 B，+38%）、experiment-execution 137→165 行（7,481→8,639 B，+15%）。整文件读的字节比 v0.2 多，这是 retune2 略高于 retune1 的直接原因，也是软目标 MISS 的根因 | §3.2, §4 |
| 7 | compact lose Outcome/integrity? | **否。** compact `result-analysis` 保留 Integrity（"is the artifact usable (crash, missing metrics, obvious leak)?"）、Outcome、Persist Protocol（两模式共用，含 `failed`+`not-assessed` 不产生 Negative、`invalid` 非 Negative、Invalidated Findings）；compact 不写 DISCOVERY、不进 Story Evidence；无可用产物 → `not-assessed`；prediction 不成立 → "unexpected result" → full 触发。Live：5 次 Case 10 运行 0.51 均未进 Story / DISCOVERY。**措辞缺口**：compact Outcome 条只点名 `supports` / `not-assessed`，"prediction 未成立" 依赖读者去对下方触发清单（见 T2） | `result-analysis` L23–52, L126–150 |

### 科学判断

| # | 问 | 答 | 证据 |
|---|---|---|---|
| 8 | cosmetic idea still caught? | **是（live ×2）。** G2 Case 01 unleaked：Codex `REVISE`、Claude(DeepSeek) `REVISE`；Mechanism Distinction 均点名 frozen λ 0.5→0.3 + rename；§G deletion test fail；50-seed grid 拒绝；未抄 §H glossary；canonical 八文件未动。`idea-evaluation` Skill / prompt / RI 自 `e612f8e` 起未变，结论可携带到 `38543fe` | `g2-case01*.md` |
| 9 | real mechanism ADVANCE? | **是（live ×1，Codex）。** G3 Case 02 held-out：`ADVANCE`；Distinction 落在 information 轴（port-rarity rank vs packet-size entropy）；最小测试点名 mechanism-off（窗内 shuffle ranks）+ rare-burst；无 EXP-ID。本审核复核 agent-visible prompt：`input.md` 117 行非空行 100% 逐行包含 + `prrw-spec.md` + 输出格式（"no prescribed token"），禁用短语 0 命中 → **unleaked** | `g3-case02-codex.md`, `prompts/g3-case02-agent-visible.txt` |
| 10 | weak-baseline not Story Evidence? | **是（live ×2）。** G4 Case 03：honest-baseline 与 mechanism 两准则均 `does not address`（`tuned=false`、`raw_4d_counts_not_entropy`、`honest_tuned_detector: null`、`mechanism_off: null`）；local weak-comparison `supports` 显式限定 "not Story Evidence / Outcome"；STORY SHA 不变；无 RUBRIC/CLAIMS；§F 未抄。`evidence-verification` 自 `e612f8e` 起未变 | `g4-case03*.md` |
| 11 | technical failure still `failed`+`not-assessed`? | **静态是；live 有一处需说清。** 规则在位：`experiment-record.md` L75、`experiment-execution` L92 / L157、`result-analysis` L140、`failure-diagnosis.md`、`result-diagnosis.md`、`result-review.md`。Live：G1 pre-retune 两 harness 对 "code entry missing" 都写了 `failed`+`not-assessed`（真实产生过）。**retune 之后**同一夹具改为 `completed`+`supports`——因为 retune 把 "entry missing **且** 有已供给的 smoke log" 明确定义为**非**技术失败（用 log 作产物），而真实 crash / hang / unusable metrics 仍走 full → `failed`+`not-assessed`。这是**范围收窄**而非规则删除；Case 04（真实 crash）在 V0.2.1 未 live，仅 V0.2 dry-read。措辞上把 "supplied log" 的来源要求写清即可（见 T3） | 两份 G1 vs 三份 retune 报告 |
| 12 | invalid trusted evidence → Invalidated Findings? | **静态是（允许）。** `result-analysis` Persist Protocol "Previously trusted evidence later shown unusable → DISCOVERY **Invalidated Findings**, not Negative Discovery … full-diagnosis trigger; cite `evidence-and-claim.md` §G"；`evidence-and-claim.md` L268、`result-diagnosis.md` L153/L208、`state-files.md` L78、`DISCOVERY.template.md` L21 均在位。无 live Case 07 | `git grep "Invalidated"` |

### 维护

| # | 问 | 答 | 证据 |
|---|---|---|---|
| 13 | meta-rule dup down? | **Wave E 范围内下降；retune 引入新一族，整体未降。** `load every file under` 6→1 文件；`task prompt wins` 6→3；`Outcome … only` 3→1（Gate A 已测，`38543fe` 不变）。`do not (re)copy` 55 行/28 文件 → 53/26。**新增**：`do not open … on compact` 族 0 → **19 行 / 5 文件**（experiment-design 6、experiment-execution 5、result-analysis 4、AGENTS.md 3、workspace-resume 1）；`experiment-design` 内 "Do not auto-upgrade" 六项出现两次（L39、L100–108）；Outcome 六 token 枚举在 owner 之外重新出现 1 处（T1）。指令总量：skills 77,396 → 85,977（Gate A）→ **88,371 B**（+14% vs v0.2）；prompts / RI / subagents 自 Gate A 不变 | §4 |
| 14 | deep-lit budget bounded? | **是（Gate A 已审，未再动）。** `deep-literature-mode.md` §G "Search budget (soft)" + "One extension"；Pass 1 / Pass 2 / extension 为 work-file 判断注记，"not a STATE field"；`literature-synthesis.md` / `literature-research` 接线未变 | `git diff e612f8e 38543fe -- .agents/references` 为空 |
| 15 | skill-evolution scorer independent? | **是（live G5）。** author = Wave H 历史作者（Grok）；executor = Wave H 既有 `failure/` + `protection/` 产物；scorer = Claude CLI 新 session 上的 `deepseek-v4-pro`，`Model relation: different-family`、`Context relation: fresh-context`；评分包无 `grader.md` / 无 Wave H `REPORT.md` / 无 expected PASS；scorer 输出 `RECOMMENDATION: reject`（§A trigger 不成立 + §E 第一合取失败），与 Wave H 同向但**独立得出**；candidate 未 merge（`.agents/` diff 为空，canonical Skill 哈希不变）。`skill-evolution.md` §C/§D/§F 与 checklist #13 自 Gate A 未变 | `g5-*.md`, `artifacts/g5-scorer-stdout.txt` |
| 16 | fixture/grader isolated? | **是。** `live-cases/caseNN/{input.md, artifacts/, grader.md}` 三分；本审核对 4 个 `input.md` + 7 个 artifacts 复扫六条禁用短语 + `grader`：**0 命中**。Wave G 每次 run 前在 clone 删除 `grader.md` / `cases/` / `prompt-regression/` / `gate-reviews/` / `gate-a-review.md`（Codex 侧再 isolation-commit），报告逐项记录 prompt SHA 与 grader SHA（本审核核对 case01/03/10 `input.md` SHA-256 前缀与报告一致；case02 用单独 prompt 文件，见 Q9）、Host-memory check、Claude file-read trace。残余：Claude G1 pre-retune 读了根 `README.md`（报告已标 "scoring-visible"）；Codex 侧 clone 历史中 `git show e612f8e:…/grader.md` 仍可达（报告已标；jsonl 无打开）；Gate A N4 提问者层面自曝未改（历史夹具不重写，正确） | live-cases 树 + 9 份报告 |

### 发版

| # | 问 | 答 | 证据 |
|---|---|---|---|
| 17 | README only claims verified work? | **是（无夸大；有一处过时的少报）。** `README.md` L74–78 如实写 "G1 compact soft target **MISS** on both harnesses; compact retune2 still **MISS** … agents still ingest full SKILL.md. Do **not** claim compact token success. G2/G4 PASS; G3 ADVANCE PASS; G5 reject (no deploy)"；L62–63 把 harness 矩阵限定为 V0.2 tag 证据；L90 债务行保留 "compact token 软目标仍 MISS"。**过时**：L71–72 "Gate A and Gate B have **not** passed" —— Gate A `APPROVE` 已落盘于 `e612f8e`（`docs/validation/v0.2.1/gate-a-review.md`）；此为少报不是多报，发版步骤改（T5） | `README.md` |
| 18 | master vs tag | `origin/master` = `577bb76` = `v0.2^{}`，**尚未** v0.2.1 —— 按任务书为预期，不阻塞 | §1 |
| 19 | historical tags unmoved? | **是。** 三 tag 的 peeled commit 与 tag object 本地 = 远端 = `baseline.md` 记录值 | §1 |

---

## 3. Compact token 诚实度（不盖章）

### 3.1 Live 数字（报告原文；不同 harness 计量不可混）

| Run | Skill 提交 | 计量 | 值 | vs 54,664 | ≤30k | −40% | 判定 |
|---|---|---|---:|---:|:-:|:-:|---|
| V0.2 Case 10 Codex | `577bb76` | stderr `tokens used`（0.152.1） | 54,664 | — | N | — | 基线 |
| Codex G1 | `e612f8e` | non-cached in + out + reasoning（0.153） | 62,368 | +14% | N | N | **MISS** |
| Claude G1 | `e612f8e` | uncached `input_tokens` | 50,170 | −8% | N | N | **MISS** |
| Codex retune1 | `ee515fe` | 同 Codex G1 | 38,009 | −30% | N | N | **MISS** |
| Codex retune2 | `a81f0ca` | 同 Codex G1 | 40,321 | −26% | N | N | **MISS** |
| Claude retune | `a81f0ca` | uncached `input_tokens` | 44,765 | −18% | N | N | **MISS** |

五份报告与 `wave-g/README.md`、根 `README.md` 三层都写 "Do not claim compact success"；Codex 报告都单列 "gross cache-inclusive input 不可与 54k 比"，Claude 报告都写 "cache_read 不可与 Codex 计量比"。**未发现任何把 −30% 或 38k 包装成达标的句子。** 本审核接受这些为 **limitation**，不是 MAJOR 触发。

### 3.2 根因核对（静态可复现）

- 报告归因 "agents read entire SKILL.md despite stop-fold" —— 与 §4 尺寸一致：三个 compact SKILL.md 分别位于 L56 / L37 / L58 放 stop line，但文件总长 226 / 165 / 196 行；Codex `sed -n '1,240p'` 与 Claude 全文 `Read` 在两个 harness 上都会越过 stop line。
- 指令式 fold（"Stop. Do not read the rest"）在两族模型上 0/3 生效。**任务书明确不要求**拆 SKILL.md 成新 Skill 或第 13 Skill 来冲 30k；本审核也不提此建议。可在不新增文件的前提下做的是**删重**（T4）：把 stop line 以下与以上重复的 compact 内容压成一行指针，使整文件读的字节回落——这是有界措辞，不是架构变更，也不保证达标。
- 报告 "Do not retune fixtures to chase 30k" 得到遵守：`live-cases/case10/input.md` SHA-256 `f88e4fd7…` 五次运行不变。

### 3.3 保护与判断（未回归）

| 项 | 结果 |
|---|---|
| Case 10 protection（无 Idea/Evidence/Reviewer/result-analyst 工作文件；0.51 不进 Story） | 5/5 运行 **held** |
| G2 costume → `REVISE` | 2/2 |
| G3 real mechanism → `ADVANCE` | 1/1 |
| G4 weak baseline → `does not address`，STORY 不变 | 2/2 |
| G5 independent scorer → `reject`，无 deploy | 1/1 |

---

## 4. Retune（`e612f8e..38543fe` 的 Skill / AGENTS 部分）静态审读

只改 `experiment-design` / `experiment-execution` / `result-analysis` / `research-loop` / `workspace-resume` 五个 SKILL.md 与 `AGENTS.md`；prompts / RI / Protocol / templates / subagents **零改动**。

**保留正确的部分（不需改）：** compact 五项映射到既有 EXP 字段；Persist Protocol 两模式共用；`failed`+`not-assessed` / `invalid` / Invalidated Findings 三条 DISCOVERY 规则原位；"Do not assign a scientific Outcome in execution"；compact 不写 DISCOVERY/STORY；full 触发清单完整；`research-loop` 仍是 exploratory routing owner（checklist #14 点名）；`workspace-resume` 收窄到 UNINITIALIZED / 下一步不清，不影响冷启动安全（`AGENTS.md` Start Here 仍要求 PROJECT → STORY → STATE）。

**需有界修正的部分：**

| # | 位置（`38543fe`） | 观察 | 为什么要改 | 有界修法 |
|---|---|---|---|---|
| **T1** | `result-analysis/SKILL.md` L81–84 | "Set **Outcome** from the closed set `not-assessed` / `supports` / `contradicts` / `null` / `inconclusive` / `invalid` (owner: `experiment-record.md` …)" | 这是 Outcome **完整枚举**在 owner 之外的复本——Gate A §2 第一行 SSOT 检查（"单行枚举 … 在 owner 之外 0 命中"）在 `e612f8e` 为 0，`38543fe` 为 1（换行书写，单行 regex 漏检）。`experiment-record.md` L77 原文 "其它文件引用本节，不要再抄这张表"。compact sanity 只需 `supports` / `not-assessed` 两个 token，且 L38–40 已点名 | 删去六 token 列表，保留 "Set **Outcome** per `experiment-record.md` §Outcome 值 (do not copy that table)"；一句话 |
| **T2** | `result-analysis/SKILL.md` L38–40 | "else `not-assessed` only when there is no usable artifact at all" | 只覆盖 "prediction 成立" 与 "无产物" 两种情形；"产物可用但 prediction **未**成立"（例如预测 chance-like 却得 0.95、或预测有限却 NaN）需读者自行对到 L47–50 的 "unexpected result" 触发。可能被读成 "exit 0 ⇒ supports" | 该条末尾补一短语："if the prediction did **not** hold → unexpected result → full diagnosis" |
| **T3** | `experiment-execution/SKILL.md` L23（"a supplied **MOCK** / `artifacts/` log"）；`result-analysis/SKILL.md` L33–34（"(finite metric; predicted **chance-like F1**)"）、L44（"**Chance-like F1** that the design predicted …"） | 夹具词汇（MOCK、F1、chance-like、`artifacts/` 目录名）写进 canonical Skill；`v0.2` 与 `e612f8e` 的 `.agents/skills/` 内 MOCK / F1 / chance-like 均为 0 命中。这是 Skill 向 Case 10 期望行为收敛的痕迹；原则本身（"已供给的、能回答已设计 sanity Question 的产物可用；entry missing 不是 debug 触发"）可辩护，但词汇应通用。另外 "supplied log" 未要求写明来源，真实项目里来源不明的日志不应被记为该 EXP 的 run | 三行改为通用措辞（"a supplied artifact that answers the already-designed sanity prediction"；去掉 MOCK / F1 / chance-like）；execution compact persist 加一短语："record the artifact's path and provenance in Runs / Results; provenance unknown → `not-assessed` + STATE blocker" |
| **T4** | `AGENTS.md` L26–34、L44、L53；`experiment-design` L24 / L76 / L112 / L123 / L136 / L197 与 L39 + L100–108；`experiment-execution` L20 / L59 / L67 / L105 / L133；`result-analysis` L25 / L107 / L149 / L165 | "do not open X on compact" 同一治理句族 0 → 19 行 / 5 文件；`AGENTS.md` 整段复述三个 Skill 各自的 do-not-open 清单；`experiment-design` "Do not auto-upgrade" 六项两处；stop line 以下的 "Compact flow" 仍保留 7 步而非指针。checklist #14 ">3 files → owner + cite"，owner 为 `research-loop`（exploratory routing）。副作用：这些重复正好落在被整文件读的三个文件里 | (a) `AGENTS.md` L26–32 压成一行路由："ACTIVE 且 STATE 已指明普通 sanity → 直接走 compact `experiment-design` / `result-analysis`，不经 `workspace-resume` / `research-loop`；各 Skill 自己声明 compact 不读什么"，删除逐文件清单与 L34 / L44 两处 Protocol 旁注；(b) 三个 Skill 内 "Files to read" 表保留一份 do-not-open 行，正文内的行内 "(do not open … on compact)" 旁注删除；(c) `experiment-design` L100–108 与下方 "Compact flow" 7 步改为 "See Compact (default) above" 一行。全部为删句，不新增文件、不动 Protocol |
| **T5** | `README.md` L71–72；`live-cases/README.md` L3–4、L75–82 | "Gate A and Gate B have **not** passed" 过时（Gate A `APPROVE` 在 `e612f8e`）；live-cases README "Wave G live runs: not filled" 与全空的 Live runs 表过时（G1–G5 已跑，证据在 `docs/validation/v0.2.1/wave-g/`） | 发版步骤：README 改为 "Gate A APPROVE (`e612f8e`); Gate B `TARGETED_REVISION` (this file) → 修订后状态"；live-cases README Live runs 段改为指向 `../../v0.2.1/wave-g/README.md` 的一行，表格删除或填 4 行摘要（不改 `input.md` / `grader.md` / artifacts） |

**非阻塞观察（可选，不计入 TARGETED 清单）：**

| # | 位置 | 观察 | 建议 |
|---|---|---|---|
| N1 | Gate A N1–N3、N6、N8 | 均未在 Wave G / retune 处理（`experiment-review` Next-sync 触发仍为 5 选 3；`Memo:` 指针未登记；compact 查重最小集未点名；D 段 "scope in STATE" 邻接句；sources "Decision frontier" 行 "Wave D" 未加 "V0.2"） | 与 T1–T5 同批处理或顺延到 v0.2.2，不阻塞 |
| N2 | 五个 retune Skill | 多数 `[file](../../path)` 改为裸 `` `file` ``，Skills 内相对链接从 382 降到 331；full 模式下读者需自行定位 `.agents/prompts/` / `.agents/references/`（`AGENTS.md` L67 与其它 Skill 仍给出目录，可定位） | 保持亦可；若 `framework-maintenance` 的链接检查以 markdown 链接为对象，此变化让该检查对这五个文件变空，写一句说明 |
| N3 | `research-loop` L95–96 | "do **not** load this Skill" 写在该 Skill 内部（读到时已加载） | 无害；可删 |
| N4 | Claude Code 侧 | 三份 Claude 报告都写明 CLI 默认模型为 `deepseek-v4-pro[1m]`，不是 Anthropic 权重；"Claude Code live" 在 README 应保留这个限定（`wave-g/README.md` 已写，根 README 未写） | T5 一并加半句 "(Claude Code CLI on DeepSeek weights)" |
| N5 | G3 | 仅 Codex 一 harness（任务书允许）；G5 held-out Case 02 为 hunk dry-score 非 live（报告已声明） | 记录即可 |

---

## 5. Blocking items

**无。** 按任务书三条阻塞判据逐条核对：

| 判据 | 结果 |
|---|---|
| README / release text **claims** compact token success | **否** —— 三层文本均写 MISS + "Do not claim"（§2 Q17、§3.1） |
| architecture broken | **否** —— 8 / 1 / 5 / 12 / 6 / 0；Protocol 四件 byte-identical；无隐藏状态；无第 13 Skill；无第二编排器（§1、§2 Q1–4） |
| judgment regressions（costume ADVANCE / real mechanism always-reject / weak baseline into Story） | **否** —— G2 `REVISE` ×2、G3 `ADVANCE`、G4 `does not address` ×2 且 STORY 不变；`idea-evaluation` / `evidence-verification` 及其 prompt / RI 自 Gate A 起零改动（§2 Q8–10、§3.3） |

软目标 MISS + 诚实报告 = **limitation**，不升级为 MAJOR。

---

## 6. Verdict

**`TARGETED_REVISION`**

理由：三条阻塞判据全部未触发（§5），架构、Protocol、历史 tag、`origin/master`、SSOT 主体、科学判断门与 skill-evolution 独立性均经静态 + live 双重核实；compact 软目标 MISS 在报告、`wave-g/README.md`、根 `README.md` 三层如实记录且未被包装。**未达 `APPROVE_V0_2_1`** 的原因全部是有界措辞（§4 T1–T5）：Outcome 六 token 枚举在 owner 外重新出现（Gate A 为 0 的 SSOT 项回退 1 处）；compact Outcome 条缺 "prediction 未成立" 分支；夹具词汇（MOCK / chance-like F1 / `artifacts/`）进入 canonical Skill 且 supplied-log 未要求来源；"do not open" 治理句 0 → 19 行 / 5 文件与 `AGENTS.md` 整段复述违反 checklist #14 并直接抬高被整文件读的字节；README / live-cases README 两处状态过时（少报方向）。五项均为删句或一句话补语，不新增文件、不动 Protocol、不拆 Skill、不改夹具。

**复审方式：** 修订后 diff-only 复核 T1–T5 所列行；不要求重跑 live（T1–T5 不改判断算子）。若实施侧愿意在 T4 之后再跑一次 Case 10 只为记录字节变化，结果无论达标与否都按 §3 同样口径写；**不以达标为 v0.2.1 发版条件**。

**发版前提（复审通过后）：** `origin/master` FF 到 v0.2.1 提交、打 annotated tag `v0.2.1`，`v0.1` / `v0.1.1` / `v0.2` 三 tag 不动、不 force push；README `Framework base` 去掉 `-in-progress`，§当前状态改为 T5 措辞并保留 G1 MISS 原句。
