# V0.2.1 Gate B Re-review — T1–T5 diff-only 复核

- **角色：** 独立 V0.2.1 Gate B Re-reviewer。相对 T1–T5 实施者为 **different-family**（本复核由 Claude 执行）+ **fresh-context**（无实施聊天历史、无前次 Gate B 审核会话上下文；只读磁盘 + `/usr/bin/git`）。
- **复核时点：** 2026-09-04 15:35–15:40（UTC+8）。
- **对象：** `origin/v0.2.1-micro-hardening` = **`6a74afe253d065061750d56c354ed586e3f475e6`**（`V0.2.1 Gate B: targeted wording T1–T5`，author oasjkow，2026-09-04 15:33:50 +0800）。父提交 `37ca336`（`V0.2.1 Gate B: terminal independent review`，仅新增 `gate-b-review.md`），其父 `38543fe`（前次 Gate B 审核锁定对象）。`git ls-remote origin refs/heads/v0.2.1-micro-hardening` = `6a74afe`，与 worktree HEAD 一致；`git merge-base --is-ancestor` 成立。
- **工作树：** `/private/tmp/rss-v021-wave0`（分支 `v0.2.1-micro-hardening`，HEAD `6a74afe`，porcelain 为空，无 `MERGE_HEAD`）。主 checkout `/Users/herxanadu/research-story-speaker` 仍在 `v0.2-research-intelligence` = `577bb76`。
- **范围：** 严格 **diff-only**：`git diff 37ca336 6a74afe`（= `38543fe..6a74afe` 去掉 `gate-b-review.md` 的新增），6 文件 +65 / −106。逐条对照 `gate-b-review.md` §4 T1–T5 的行位与修法。**未**重跑任何 live harness；**未**重读全仓；live 数字不重算、不重新解释。
- **只写：** 本文件（`docs/validation/v0.2.1/gate-b-rereview.md`，落在 wave0 worktree，**未** `git add` / commit / tag / push）。未改 Skills、prompts、Layer 2、Protocol、templates、`.research/`、`AGENTS.md`、`README.md`。
- **Gate 判定词汇：** `APPROVE_V0_2_1 | TARGETED_REVISION | MAJOR_REVISION`（任务书规定的框架层 gate 结论，不是 `reviewer.md` §Verdict、不是 Idea-gate 四动作、不是 `evidence-and-claim.md` §F 标签）。
- **前次结论（输入）：** `gate-b-review.md` = `TARGETED_REVISION`，T1–T5 全部为有界措辞，**无** architecture / judgment 阻塞项。本复核只判断 T1–T5 是否闭合、冻结是否完好，不重开前次已核实的项。

---

## 0. 对象与 diff 边界

```text
38543fe  Merge compact-retune → micro-hardening        ← 前次 Gate B 锁定对象
37ca336  Gate B: terminal independent review            (+ docs/validation/v0.2.1/gate-b-review.md, 188 行)
6a74afe  Gate B: targeted wording T1–T5                 ← 本复核对象
```

`git diff --name-status 38543fe 6a74afe`：

| 状态 | 文件 | 归属 |
|---|---|---|
| M | `.agents/skills/result-analysis/SKILL.md` | T1 / T2 / T3 / T4 |
| M | `.agents/skills/experiment-execution/SKILL.md` | T3 / T4 |
| M | `.agents/skills/experiment-design/SKILL.md` | T4 |
| M | `AGENTS.md` | T4 |
| M | `README.md` | T5 |
| M | `docs/validation/research-intelligence/live-cases/README.md` | T5 |
| A | `docs/validation/v0.2.1/gate-b-review.md` | 前次审核落盘 |

**没有其它文件变动。** `git diff --stat 38543fe 6a74afe -- .agents/references .agents/prompts .agents/subagents .agents/templates .research adapters CLAUDE.md .gitignore .claude .agents/skills/research-loop .agents/skills/workspace-resume docs/validation/v0.2.1/wave-g docs/validation/v0.2.1/gate-a-review.md docs/validation/v0.2.1/baseline.md docs/validation/research-intelligence/{cases,prompt-regression,gate-reviews,source-audit,live-cases/case01,live-cases/case02,live-cases/case03,live-cases/case10}` 为空。即：**判断算子（`idea-evaluation` / `evidence-verification` 及其 prompt / RI）、Protocol、模板、subagents、夹具、历史证据、Wave G 报告自 `38543fe` 起零改动**，前次审核 §2 Q8–Q12、§3.3 的 live 结论可直接携带。

---

## 1. 冻结检查（全部于 `6a74afe`）

| 项 | 结果 | 证据 |
|---|---|---|
| canonical state = 8 | **✓** | `git ls-tree 6a74afe -- .research/`：8 个 `*.md` + `reviews/` + `work/`；`git diff --stat 577bb76 6a74afe -- .research/` 为空（仍 UNINITIALIZED） |
| research-loop = 1 | **✓** | 唯一 `.agents/skills/research-loop/SKILL.md`；自 `38543fe` 未动 |
| subagents = 5 | **✓** | experiment-agent / literature-scout / research-lead / result-analyst / reviewer；自 `38543fe` 未动 |
| skills = 12 | **✓** | `.agents/skills/*/SKILL.md` = 12；`.claude/skills/` 12 个 symlink 全部解析到存在的 `SKILL.md` |
| RI references = 6 | **✓** | 6 个 `.md`，目录内无其它对象；自 `38543fe` 未动 |
| framework scripts = 0 | **✓** | `git ls-tree -r 6a74afe` 中 `100755` = 0；`.agents/` 内非 `.md` / `.gitkeep` / symlink = 0 |
| Protocol 四件 byte-identical | **✓** | `state-files.md` / `experiment-record.md` / `story-loop.md` / `git-linking.md` 对 `v0.2` diff 为空；`reviewer.md` 对 `v0.2` 仍只有 Gate A 已审的 +2/−1，自 `38543fe` 未动 |
| `v0.1^{}` | **✓** | `8db3b301f5bce878d6c2ee4a61bcb234c9609c3d`；tag object `96340ad8…`；本地 = `git ls-remote origin` = `baseline.md` L25 |
| `v0.1.1^{}` | **✓** | `762deb4c9db896acb5c00066b8e6dc5a63732cfa`；tag object `90a41d5d…`；本地 = 远端 = `baseline.md` L24 |
| `v0.2^{}` | **✓** | `577bb76f4cc7a43ae4c64a1c35456d4d53e746c1`；tag object `0bd2d2c6…`；本地 = 远端 = `baseline.md` L23 |
| `origin/master` | **✓（预期）** | `git ls-remote origin refs/heads/master` = `577bb76` = `v0.2^{}`；尚未 FF 到 v0.2.1，发版前的正确状态 |
| 合并 / 冲突 | **✓** | 树内 `<<<<<<<` / `=======` / `>>>>>>>` 行 = 0；`6a74afe` 为单亲提交 |
| 相对链接 | **✓** | 6 个变动文件内 13 个相对链接全部解析（含 README → `gate-a-review.md` / `gate-b-review.md` / `wave-g/`；live-cases README → `../../v0.2.1/wave-g/` 与其 `README.md`） |
| 工作树 = HEAD | **✓** | `git diff --quiet HEAD -- <6 文件>` 无差异；porcelain 为空 |

**冻结完好。**

---

## 2. T1–T5 逐条闭合

行号均为 `6a74afe` 上的行号；"前" 为 `38543fe`。

### T1 — Outcome 六 token 枚举离开 `result-analysis`，只引用 `experiment-record.md`

| | |
|---|---|
| 前次要求 | 删去六 token 列表，保留 "Set **Outcome** per `experiment-record.md` §Outcome 值 (do not copy that table)"，一句话 |
| diff | `result-analysis/SKILL.md` L84–85：`Set **Outcome** from the closed set \`not-assessed\` / \`supports\` / \`contradicts\` / \`null\` / \`inconclusive\` / \`invalid\` (owner: … do **not** open that file on compact if you already know the token)` → `Set **Outcome** per \`experiment-record.md\` §Outcome 值 (do not copy that table).` |
| 复核 | (a) 多行容忍搜索（六 token 依序出现、间隔 ≤200 字符）跨 `.agents/` + `AGENTS.md` + `README.md` + `CLAUDE.md`：**仅命中 owner** `.agents/references/experiment-record.md`（§Outcome 值在 L71，anchor 存在）。前次 Gate A 为 0 命中 / Gate B 为 1 的 SSOT 项**回到 0**。(b) `closed set` 在 `.agents/skills/` 0 命中。(c) `.agents/skills/` 内余下 `contradicts` / `inconclusive` 命中为语境用法（`result-analysis` L141 DISCOVERY 规则 "`Status=completed` 且 `Outcome=contradicts` 或 `null`"、L192 anti-pattern；`workspace-resume` L132 动词），不是枚举。`failure-diagnosis.md` L76–81 / L195–199 是九类失败分类（measurement / hypothesis contradiction / … / inconclusive），不是 Outcome 枚举，且 prompts 自 Gate A 未动。 |
| 结论 | **闭合** |

### T2 — compact Outcome：产物可用 + prediction 未成立 ≠ compact-`supports`

| | |
|---|---|
| 前次要求 | L38–40 该条末尾补一短语："if the prediction did **not** hold → unexpected result → full diagnosis" |
| diff | `result-analysis/SKILL.md` L38–43（stop line 之上，Compact (default) 内）：`… else \`not-assessed\` only when there is no usable artifact at all. If the artifact is usable but the prediction did **not** hold, that is **not** compact-\`supports\` — unexpected result → full diagnosis, then the Protocol Outcome that path would use (do not invent a token).` |
| 复核 | (a) 三种情形现已全覆盖：prediction 成立 → `supports`；无可用产物 → `not-assessed`；产物可用但 prediction 未成立 → **不是** compact-`supports`，走 unexpected result → full。(b) 与同节 L56–59 的 full 触发清单一致（"unexpected result" 是第一项）。(c) "then the Protocol Outcome that path would use (do not invent a token)" 不列 token，与 T1 一致，把 token 选择留给 owner。(d) 补语的对偶句 L47–48 同步改为通用（见 T3）："A result that matches the already-designed sanity prediction is **not** an unexpected-result full-diagnosis trigger"——两句互补、不矛盾。(e) 不新增 Protocol 值、不新增字段。 |
| 结论 | **闭合** |

### T3 — canonical Skills 无 MOCK / chance-like F1；supplied log 要求来源

| | |
|---|---|
| 前次要求 | 三行改为通用措辞（去掉 MOCK / F1 / chance-like / `artifacts/`）；execution compact persist 加一短语："record the artifact's path and provenance in Runs / Results; provenance unknown → `not-assessed` + STATE blocker" |
| diff | `experiment-execution/SKILL.md` L23–26：`a supplied MOCK / \`artifacts/\` log` → `an operator-supplied log or pre-existing result file`。L28–34 compact persist 新增：`Record provenance in Runs / Results: path, and that it was not this-run compute. Provenance unknown → keep Outcome \`not-assessed\` and put a STATE blocker.`；L84、L88、L99 的 "supplied log" 统一为 "operator-supplied log"。`result-analysis/SKILL.md` L32–36：`A supplied in-prompt or \`artifacts/\` log that matches the already-designed smoke Question (finite metric; predicted chance-like F1)` → `An operator-supplied log or pre-existing result file that answers the already-designed sanity prediction (finite metric)`；L47–48：`Chance-like F1 that the design predicted is **not** …` → `A result that matches the already-designed sanity prediction is **not** …`；L53–54 Required reads 同步。 |
| 复核 | (a) `git grep -E "MOCK|\bF1\b|chance-like|artifacts/" 6a74afe -- .agents/skills`：**0 命中**（`38543fe` 为 3）。整个 `.agents/` 仅余 `references/research-intelligence/skill-evolution.md:135`（"input.md — MOCK state, trigger, artifacts, write discipline"）——这是 RI 文件对评测包 input/grader 拆分的 owner 描述，v0.2 既有、自 Gate A 未动，不在 T3 范围（T3 限定 canonical Skills）。(b) 来源要求三要素齐备：路径、"not this-run compute"、provenance unknown → `not-assessed` + STATE blocker，与前次建议短语逐项对应。(c) 原则未变：已供给且能回答已设计 sanity prediction 的产物可用；entry missing 不是 debug 触发；真实 crash / hang / unusable metrics 仍走 full → `failed` + `not-assessed`（L36–38、L106–108）。 |
| 结论 | **闭合**（附带观察 N1，非阻塞，见 §3） |

### T4 — compact do-not-open 清单不再抄进 `AGENTS.md`；auto-upgrade 不再重复

| | |
|---|---|
| 前次要求 | (a) `AGENTS.md` L26–32 压成一行路由，删逐文件清单与 L34 / L44 Protocol 旁注；(b) 三个 Skill 各保留一份 do-not-open，删正文行内 "(do not open … on compact)" 旁注；(c) `experiment-design` L100–108 "Do not auto-upgrade" 六项与 "Compact flow" 7 步改为指针。全部为删句，不新增文件、不动 Protocol |
| diff | **`AGENTS.md`**：L26–28 = `ACTIVE 且 STATE 已指明普通 sanity / exploratory EXP 时：直接走 compact \`experiment-design\` / \`result-analysis\`，不经 \`workspace-resume\` / \`research-loop\`。各 Skill 自己声明 compact 不读什么。`；原 L26–32 的逐文件 "不要默认加载 `result-diagnosis.md` / `failure-diagnosis.md` / RI … Protocol 文件不是 compact 必读 … 不必读 `README.md` / `docs/validation/`" 全删；L30（原 L34）与 L40（原 L44）两处 "compact sanity 不要默认打开 / 不必打开该 Protocol 文件" 旁注删除。88 → 84 行，4,863 → 4,377 B。**`experiment-design`**：L76 "(do not open on compact)" → "full design only"；原 L100–108 六项列表 → L100 `Do not auto-upgrade: see **Compact (default)**`；原 L112 "do **not** open that file on compact" 删；原 L119–142 "Compact flow" 7 步整段删，仅留 L107 `See **Compact (default)** above the stop line.`；L163 表格行 → `See **Compact (default)**`；L184 → `(see **Compact (default)**)`。**`experiment-execution`**：原 L59 / L67 / L105 三处行内旁注删；L135 表格行 → `See **Compact / ordinary sanity (default)**`。**`result-analysis`**：原 L107 "Do not open those files unless a trigger above matches" 删；原 L149 "(do **not** open that file on compact)" 删；L164 表格行 → `See **Compact (default)**`。 |
| 复核 | (a) `do (**)not(**) (re)open` 族（`.agents/skills` + `AGENTS.md`，英文正则）：**16 行 / 4 文件 → 7 行 / 4 文件**。余下 7 = 三个 compact Skill 各一份 owner 清单（`experiment-design` L24–26、`experiment-execution` L20–21、`result-analysis` L25–28，均在 stop line 之上）+ 三处表格指针 + `workspace-resume` L142 "Do not open a questionnaire"（与文件加载无关，v0.2.1 Wave 既有）。`AGENTS.md` 3 → **0**；中文旁注 "不要默认加载 / compact sanity 不必打开 / 不要默认打开"：**0**。(b) "Do not auto-upgrade" 六项：`experiment-design` **仅 L39–40 一份**，L100 与 L184 为指针（1 文件 / 3 行，其中 1 列表 + 2 指针）。(c) 实施侧把 owner 清单留在 Compact (default) 节、表格行改指针（前次建议 (b) 是反向：留表格、删正文）。两者都满足 "每 Skill 一份"，且把清单留在 stop line **之上**更符合 compact 读者的阅读顺序——接受。(d) 字节：`experiment-design` 226 → **192** 行 / 10,903 → **9,141** B（−16%）；`result-analysis` 196 → 195 / 9,386 → 9,214 B（−2%）；`experiment-execution` 165 → 167 / 8,639 → 8,618 B（−0.2%；+2 行来自 T3 provenance 必需句）。Skills 总量 88,371 → **86,416** B（−2.2%；仍比 v0.2 的 77,396 高 +11.7%）。前次审核已明确 T4 是删重而非 token 达标手段，soft target 仍 MISS 且**不是发版条件**；此处只记录字节确实回落。(e) `AGENTS.md` 新路由行仍是 "读已决定的 STATE 下一步 → 派 Skill" 的入口路由，不决定科研下一步、不循环；Start Here 冷启动顺序 PROJECT → STORY → STATE 未动；Skill Routing 表 L49 的 compact 行保留（见 N2）。无第二编排器、无隐藏状态（`compact` / `full` 仍声明 "Never write them into STATE, EXPERIMENTS, Status, or Outcome"：`experiment-design` L19–20、`experiment-execution` L15–16、`result-analysis` L20–21）。 |
| 结论 | **闭合** |

### T5 — README / live-cases 状态不过时；仍无 compact-token-success 声明；无假 Gate B APPROVE

| | |
|---|---|
| 前次要求 | README 改为 "Gate A APPROVE (`e612f8e`); Gate B TARGETED_REVISION (this file) → 修订后状态"；live-cases README Live runs 段改为指向 `../../v0.2.1/wave-g/README.md` 的一行，删空表（不改 `input.md` / `grader.md` / artifacts） |
| diff | **`README.md`** L71–75：`Gate A and Gate B have **not** passed` → `Gate A **APPROVE** (\`e612f8e\`; [gate-a-review.md]). Gate B is **TARGETED_REVISION** in progress ([gate-b-review.md]). Do **not** claim Gate B APPROVE.`；L93 债务行 `Gate A/B` → `Gate B TARGETED_REVISION`。L77–81 原句**保留**："G1 compact soft target **MISS** on both harnesses; compact retune2 still **MISS** … agents still ingest full SKILL.md. Do **not** claim compact token success. G2/G4 **PASS**; G3 **ADVANCE PASS**; G5 **reject** (no deploy)"。**`live-cases/README.md`** L3–5：`Wave G fills runs. This Wave ships the tree only` → `Wave G live runs **are** filled — reports in [../../v0.2.1/wave-g/]. Do **not** claim compact token success or Gate B APPROVE.`；L54 `leave these fields blank until a run exists` → `filled in [../../v0.2.1/wave-g/] reports`；L73–76 "Live runs" 段：删 `not filled` 代码块与 4 行全空表，改为一行指针到 `../../v0.2.1/wave-g/README.md` (G1–G5) + "Do **not** claim compact token success"。 |
| 复核 | (a) **无假 APPROVE**：`git grep -i approve 6a74afe -- README.md AGENTS.md live-cases/README.md wave-g/README.md` 命中仅 README L71（Gate A，属实）、L75 与 live-cases L5（均为 "Do not claim Gate B APPROVE"）。(b) **无 compact-token-success 声明**：README L77–80、live-cases L5 / L76、`wave-g/README.md` L7–8 / L14 / L20–22 / L38–39 三层仍写 MISS + "do not claim"；`wave-g/README.md` 自 `38543fe` 未动。(c) **不再过时**：Gate A APPROVE 已写明并链接；live-cases 不再说 "not filled" / "do not run live here"；两个 README 的 13 个相对链接全部解析。(d) 不改夹具：`case01/02/03/10` 对 `38543fe` diff 为空。(e) README L62–63 仍把 harness 矩阵限定为 V0.2 tag 证据；L85 边界句保留。 |
| 结论 | **闭合**（"Gate B TARGETED_REVISION in progress" 在本文件落盘后由发版提交更新为终态，见 §4 发版前提） |

---

## 3. 非阻塞观察（不计入判定）

| # | 位置 | 观察 | 处置 |
|---|---|---|---|
| N1 | `experiment-design` L42–44 → `result-analysis` L32–36 | T3 的 provenance 要求按前次建议落在 `experiment-execution` compact persist。但 already-planned sanity 且无需跑码时，`experiment-design` 直接路由到 compact `result-analysis`（"else compact `result-analysis` of supplied artifacts"），**不经** `experiment-execution`；`result-analysis` 的 Integrity 条（"is the artifact usable (crash, missing metrics, obvious leak)?"）未点名 provenance。风险有界：compact `supports` 明确不写 DISCOVERY、不进 Story Evidence（L50–52）；v0.2 本无 provenance 规则，非回归。这是**本复核新提出**的观察，不在 T3 原文范围，不作为 leftover | 顺延 **v0.2.2**：Integrity 条补半句 "provenance unknown → not usable → `not-assessed` + STATE blocker"。**发版提交不要加**，以保持被批准对象的 Skill 文本与 `6a74afe` 一致 |
| N2 | `AGENTS.md` L26–28 与 L49 | Start Here 的 compact 路由行与 Skill Routing 表的 compact 行都写了 "走 compact … 不要 `workspace-resume` / `research-loop`"。两者都是路由指针、都不含 do-not-open 清单；`38543fe` 已如此，T4 (a) 只针对 L26–32。轻微重复 | 可选：v0.2.2 二选一 |
| N3 | `README.md` L77 | 前次 N4："both harnesses" 未加 "(Claude Code CLI on DeepSeek weights)" 限定；`wave-g/README.md` 与三份 Claude 报告已写明。前次已列为非阻塞 | 发版提交可顺手加半句，不影响判定 |
| N4 | Gate A N1–N3 / N6 / N8、Gate B N2 / N3 / N5 | 均未在 T1–T5 提交处理（本就不在 TARGETED 清单） | 顺延 v0.2.2 |
| N5 | compact token | 本复核未重跑 Case 10（任务书不要求；T1–T5 不改判断算子）。T4 后三文件字节回落 −16% / −2% / −0.2%，是否影响 live token 未测；无论结果都不改变本判定，README 的 MISS 原句必须保留到有新 live 证据 | 记录即可 |

---

## 4. Verdict

**`APPROVE_V0_2_1`**

理由：`gate-b-review.md` §4 的 T1–T5 五项在 `6a74afe` 上逐条闭合（§2），修法均为删句 / 一句话补语，与前次要求逐项对应，且实施仅触及前次点名的 6 个文件；判断算子、Protocol 四件、模板、subagents、RI、夹具、历史证据、Wave G 报告自 `38543fe` 起零改动，前次审核已核实的 §2 Q1–Q19、§3.3 保护与判断结论可直接携带；冻结 8 / 1 / 5 / 12 / 6 / 0 完好，`v0.1` / `v0.1.1` / `v0.2` 三 tag 本地 = 远端 = `baseline.md`，`origin/master` = `577bb76` 为预期；三层文本仍如实写 compact soft target MISS、无 compact-token-success 声明、无假 Gate B APPROVE。SSOT：Outcome 六 token 枚举回到 owner 之外 0 命中；`.agents/skills/` 内夹具词汇 0 命中；`do not open` 族 16 → 7 行且 `AGENTS.md` 归 0；"Do not auto-upgrade" 单份。§3 观察全部非阻塞、不构成 leftover。

**发版前提（实施侧执行，本复核不再介入）：**

1. 发版提交建立在 `6a74afe` 之上，**只允许**包含：本文件 `docs/validation/v0.2.1/gate-b-rereview.md`；`README.md` L3 与 `AGENTS.md` L3 的 `Framework base: v0.2.1-in-progress` → `v0.2.1`；`README.md` L71–75 的 Gate B 状态改为 "Gate B **APPROVE_V0_2_1** ([`gate-b-rereview.md`](docs/validation/v0.2.1/gate-b-rereview.md))" 并去掉 "in progress" / "Do not claim Gate B APPROVE"；L93 债务行去掉 "Gate B TARGETED_REVISION"（**保留** "compact token 软目标仍 MISS"）；可选 N3 半句。
2. `.agents/**`、`.research/**`、`.claude/**`、`adapters/**`、`CLAUDE.md` 与 `6a74afe` **byte-identical**；若发版提交动了这些路径中的任一文件，即为新对象，需另行 diff-only 复核。
3. 在该发版提交上打 annotated tag `v0.2.1`；`origin/master` **fast-forward** 到该提交；`v0.1` / `v0.1.1` / `v0.2` 不动；不 force push。
4. README L77–80 的 G1 / retune2 **MISS** 原句与 "Do **not** claim compact token success" 在 tag 上必须仍在。
