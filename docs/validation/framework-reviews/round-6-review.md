# Gate B Round 6 — 独立终审（V0.1.1 Wave C 证据 vs README 四行）

- **角色：** 独立 Reviewer R6。相对开发 Worker（Grok）为 **different-family**（本审核由 Claude 执行）+ **fresh-context**（无开发聊天历史，仅读磁盘与 `/tmp` clone 原始日志）。
- **审核时点：** 2026-09-03
- **对象：** workspace `/Users/herxanadu/Documents/story-research-workspace` 当前磁盘。HEAD = `9f8a315`。工作树相对 HEAD 仅 `README.md`、`adapters/README.md` 两文件被改（`git diff HEAD --stat`：2 files, +11/−11）；未跟踪的新增只有 `docs/validation/harness-smoke/uninitialized-*.md`、`docs/validation/handoff-tests/`、`docs/validation/framework-reviews/v0.1.1-audit.md`。根 `.research/` 相对 HEAD **无任何改动**。
- **只读：** 除本文件外未改任何路径；未 git commit。`/tmp/sw-v011-c1|c2|c3` 与 `/tmp/sw-v011-c2-logs/` 仅读取用于交叉核验，未写。
- **判定规则：** 过声称 / 漏写 / CRITICAL（根污染）按任务书；Test F/G/J 未做不判失败（V0.2）。

---

## 1. README 四行 vs 证据（对照 `README.md` L62–65）

| # | README 原句（L62–65） | 证据路径 | 证据实际支持的范围 | 判定 |
|---|----------------------|----------|--------------------|------|
| 1 | `UNINITIALIZED cold-start portability: validated on Codex, Claude Code, Cursor, and DeepSeek Harness.` | `docs/validation/harness-smoke/uninitialized-README.md` L9–15；`uninitialized-codex.md` L26–32、L320–327；`uninitialized-claude-code.md` L26–33、L83–92；`uninitialized-dsh.md` L25–32、L80–89；`uninitialized-cursor.md` L26–33、L115–123 | 四个 CLI 在独立 clone `/tmp/sw-v011-c1`（HEAD `9f8a315`，根 UNINITIALIZED）上**只读**冷启动：退出码全 0；四项核对（识别 UNINITIALIZED / 请求材料 / 未编造 / 未写文件）全 4/4；运行前后 porcelain 均空。epoch 复核：Codex 861→903（42s）、Claude 914→993（79s）、DSH 1002→1032（30s）、Cursor 1040→1063（23s），前后不重叠，串行成立。本人复核 `/tmp/sw-v011-c1` 现仍 HEAD `9f8a315`、porcelain 0 行。 | **恰好支撑。** 句子只声称 cold-start（只读），未声称 initialized；四个名字与四份产物一一对应。 |
| 2 | `Initialized write/handoff portability: validated on Codex → Claude Code.` | `docs/validation/handoff-tests/init-codex.md` L9、L82、L196–206；`docs/validation/handoff-tests/codex-to-claude.md` L5、L11–16、L52、L140–166、L188–192、L259–272 | **Codex 写：** C2 在 `/tmp/sw-v011-c2` 把根八文件 materialize 为 `ACTIVE` / `IN_PROGRESS`，EXPERIMENTS/REVIEWS 仍空；C4 步 1 Codex 写 EXP-001（planned / not-assessed）并 commit A `8976b83`。**交接：** Claude 只读冷启动读到 EXP-001、porcelain 空、HEAD 不动。**Claude 写：** fresh Reviewer 只写 `reviews/EXP-001/method-review-r1.md`；Claude Main 整合 REVIEWS/EXPERIMENTS/STATE 并 commit B `00152fe`。本人复核 clone `git log`：`00152fe` → `8976b83` → `9f8a315`，无第三 commit；`git log --diff-filter=A` 证明 review 文件首次出现于 `00152fe`。 | **恰好支撑。** 只声称 Codex → Claude Code 单向；未声称反向、未声称 Cursor/DSH 写入（这两项在 L71 明列为 V0.2 债务「四 Harness 全写闭环」）。 |
| 3 | `Research-loop portability: partially validated; full multi-harness execution remains V0.2 work.` | 同上 C2/C4 产物；`README.md` L71 债务行 | 跨 Harness 实际走过的环节只有：初始化 → `experiment-design` → 冷启动 → method review → Main 整合。**未**跨 Harness 走过 `experiment-execution`、`result-analysis`、`story-maintenance`（C4 明确不跑代码，Outcome 保持 `not-assessed`）。 | **恰好支撑。** 「partially」是对上述半环的诚实措辞；「full multi-harness execution remains V0.2」与 L71 一致。此行较 Gate A 时未改动，Wave C 只是为它补上了实际证据。 |
| 4 | `OpenCode: documentation-only, not tested.` | `uninitialized-README.md` L15（`command -v opencode` 无结果，未测）；`adapters/README.md` L17、L22 | 未安装、未运行。 | **恰好支撑。** |

**过声称：无。四行均未超出产物。**

### 1.1 `adapters/README.md` Last tested

- L5：`Last tested: 2026-09-03 (UNINITIALIZED cold-start on four harnesses)`，链接 `docs/validation/harness-smoke/uninitialized-README.md`（文件存在）；同句说明 V0.1 MOCK-root smoke 仍在同目录无前缀文件下。
- L13–16 矩阵四行 Last tested `2026-09-03`，Evidence 分别指向 `uninitialized-codex.md` / `uninitialized-claude-code.md` / `uninitialized-cursor.md` / `uninitialized-dsh.md`（四文件均存在，本人逐一 `test -f`）。L17 OpenCode `not tested` / `documentation-only`。
- 指向的是 **cold-start** 产物，未借此声称 initialized 或 handoff。Round 5 §3 第 6 项（Wave C 尚未跑就提前改 Last tested）**已销项**：现在句子与证据同日、同范围。

---

## 2. 根 `.research/` 空状态（亲自读盘，非凭记忆）

| 文件 | 磁盘事实 | 合法空状态 |
|------|----------|------------|
| `PROJECT.md` | L3 `**Project Status:** UNINITIALIZED`；其余各节 `_Not established yet._` / `_None yet._` | Y |
| `STORY.md` | L5–26 六段皆 `_Not established yet._`；全文无 `EXP-` | Y |
| `STATE.md` | L5 Focus=初始化；L9 Active=`none`；L17 Next=`Run workspace-resume initialization.`；L21 Blockers 指向 UNINITIALIZED；L33 `NOT_INITIALIZED` | Y |
| `EXPERIMENTS.md` | L5–6 仅表头（含 Outcome 列）；L8 `_No experiments yet._`；无 `## EXP-` 段 | Y |
| `REVIEWS.md` | L5 `_No reviews yet._` | Y |
| `DISCOVERY.md` | 七节皆 `_None yet._` | Y |
| `LITERATURE.md` | L3 `_No literature has been incorporated yet._` | Y |
| `RESOURCES.md` | 四节 `_None yet._`；无本机路径、无 `/tmp/sw-v011-*` | Y |
| `reviews/`、`work/` | 各仅 `.gitkeep`；无 `EXP-001/` 目录 | Y |

`rg -n "ACTIVE|EXP-|IN_PROGRESS" .research` 唯一命中为 `EXPERIMENTS.md:5` 表头 `EXP-ID`（列名，非实验 ID）。`git status --short` 不含 `.research/`，即根与 commit `9f8a315` 完全一致。

**Wave C 的 ACTIVE / EXP-001 / method-review-r1 只存在于 `/tmp/sw-v011-c2`（以及 C3 的 `/tmp/sw-v011-c3` MOCK 拷贝），未回流源仓库。无 CRITICAL。**

---

## 3. C2 / C4 权限链（对照 `codex-to-claude.md` 与 `/tmp/sw-v011-c2-logs/` 原始文件）

| 步 | 角色 | 权限 | 写了什么 | HEAD / porcelain（原始日志） | 合规 |
|----|------|------|----------|-----------------------------|------|
| C2 | Codex Main（初始化） | `codex exec --sandbox workspace-write --add-dir <toy>` | PROJECT/STORY/STATE/DISCOVERY/RESOURCES 五文件；EXPERIMENTS/REVIEWS/LITERATURE 未动 | `c2-pre-head` = `9f8a315`；`c2-post-porcelain` = 五文件 `M`；**commit 未成**（sandbox 拒建 `.git/index.lock`） | Y。协议只要求「建议 commit，可做」（`workspace-resume` init 步 5），未 commit 不违规。 |
| C4-1 | Codex Main（experiment-design） | `--dangerously-bypass-approvals-and-sandbox`（stderr `sandbox: danger-full-access`） | EXP-001 Index+section（planned / not-assessed / Review: none）、STATE、RESOURCES；commit A `8976b83` | `c4-codex-pre-porcelain` = C2 遗留五文件；`c4-codex-post-porcelain` 空 | Y。commit A 混入 C2 未提交 diff，产物 L7、L86 已披露；不影响 Reviewer/canonical 规则。 |
| C4-2 | Claude Main 冷启动 | `claude -p --allowedTools Read,Glob,Grep`（无 Write/Edit/Bash） | 无 | `c4-claude-cold-porcelain` 空；HEAD 仍 `8976b83` | Y |
| C4-3 | Claude **Reviewer**（fresh `-p`） | `--permission-mode acceptEdits --allowedTools Read,Glob,Grep,Write,Edit`（**无 Bash**）；prompt 明令只写 `reviews/EXP-001/method-review-r1.md`，禁改八 canonical，禁 git | 仅 `.research/reviews/EXP-001/method-review-r1.md` | `c4-reviewer-pre-head` = `c4-main-pre-head` = `8976b83`（Reviewer 未 commit）；`c4-main-pre-porcelain` = `?? .research/reviews/EXP-001/`（仅 untracked review 目录，canonical 零 diff） | **Y，未越权。** 符合 `AGENTS.md` L69「Subagent 只写 `.research/work/` 或 `.research/reviews/<EXP-ID>/`」与 `reviewer.md` L102。 |
| C4-4 | Claude **Main** 整合 | `--permission-mode bypassPermissions` + Bash | REVIEWS.md（`r1 — REVISE`、Provenance synthesis + Source reviews）、EXPERIMENTS.md Review 字段（Status/Outcome 未改）、STATE.md；commit B `00152fe` 含 Reviewer 文件 | `git show --stat 00152fe`：4 files；`--diff-filter=A` 首见 review 文件于 B | Y。canonical 只在 Main 的 commit B。 |

Reviewer 文件头（磁盘）：`Model relation: different-family`、`Context relation: fresh-context`、`Provenance: raw`、`Review round: 1`；正文首 heading `## Verdict` = `REVISE`（属 `reviewer.md` L136 枚举），五个规定 heading 齐。相对 designer（Codex / gpt-5.6-luna）的两维独立性成立。

**C4 规则「Reviewer 只写 review artifact；canonical 由 Main 整合」：通过。** 附注：Reviewer 的单文件限制靠 prompt 约束 + 事后 porcelain/HEAD 核验，不是工具级路径隔离（`acceptEdits` + Write/Edit 技术上可写 cwd 任意路径）。这与框架「instruction-only」定位一致，且本次证据显示已遵守；记为观察，不扣分。

### 3.1 C3（Claude symlink 深链）

`claude-deeplink.md` L7 PASS；L55–60 四条解析路径均在 `/private/tmp/sw-v011-c3/.agents/{references,templates}/`，`.claude/references/` 不存在（本人复核 `ls` 亦报不存在）；clone 内 porcelain 前后相同。L81：不需要把 Skill 相对链接改为 `../../../.agents/`。**C3 PASS → 无需改 Skill 链接**，本轮不再提链接修改。plan 模式在 clone 外 `~/.claude/plans/` 落了一份报告，产物 L49 已披露，属宿主副作用，不影响 clone 与源仓库。

---

## 4. Round 5 / v0.1.1-audit 交叉核对

- `round-5-review.md` §3 第 6 项（adapters 提前写 Wave C）已被真实证据填实（§1.1）。§3 第 1 项（示例 EXPERIMENTS 无 Outcome）与第 2 项（r2 缺 `## Verdict`）在 `9f8a315` 已修（示例 Index 含 Outcome 列、section `**Outcome:** supports`；四份 r1/r2 均有 `## Verdict`）。第 8 项（示例 `PROJECT.md` 无 Project Status 字段）仍在，Round 5 已判非阻塞，不属 Gate B 范围。
- `v0.1.1-audit.md` L20–25 对 Wave C 的四条描述（C1 全 exit 0 无写入；C2 ACTIVE 且 EXPERIMENTS 空；C3 路径在 `.agents/`；C4 Reviewer 只写 r1、Main 整合）逐条与产物一致，未超出。L31「可打 `v0.1.1`，根保持 UNINITIALIZED」与 §2 一致。

---

## 5. 判定

### Gate B：**通过**

- README L62–65 四行 **零过声称**：行 1 只到 cold-start（四份 4/4 产物）；行 2 只到 Codex → Claude Code 单向（C2 + C4，commit A/B 与 HEAD 链可复核）；行 3 保守写 partially 且把执行环留给 V0.2；行 4 未测即未测。
- `adapters/README.md` Last tested 指向 `uninitialized-*` 产物，范围与日期均与证据同步。
- 根 `.research/` 与 `9f8a315` 完全一致，UNINITIALIZED / NOT_INITIALIZED / 零 EXP / `reviews/` `work/` 仅 gitkeep。**无 CRITICAL。**
- C4 Reviewer 未越权、canonical 只在 Main commit B；C3 PASS 不需改链接。

### 阻塞项

无。

### 过声称行

无。

### CRITICAL

无。

### 漏写 / 非阻塞观察（不阻塞 Gate B，供 Main 择机整合）

1. **Codex `workspace-write` sandbox 不能 git commit（漏写）。** C2 与 C4 均记录 `workspace-write` 拒建 `.git/index.lock`，C4 步 1 被迫升到 `--dangerously-bypass-approvals-and-sandbox`。`adapters/codex.md` 与 `adapters/README.md` §Structural limitations 目前均无此条（`rg sandbox|index.lock adapters/` 零命中）。建议加一行：Codex 在 `workspace-write` 下无法写 `.git/`，workspace commit 需 full-access 或由外层执行。不影响 README 四行的正确性。
2. **README 行 2、3 无直接证据指针（非阻塞）。** 行 1 有 `adapters/README.md` 指向 `uninitialized-*`；行 2、3 依赖 Quick Start 的总入口 `docs/validation/`。可在行 2 末追加 `docs/validation/handoff-tests/codex-to-claude.md`，行 3 注明「已走：初始化 → 设计 → method review → 整合；未走：执行 → 结果分析」。
3. **Quick Start 步 2/5 的隐含范围（非阻塞）。** 步 2 列四个 Harness、步 5 说 Agent 会 materialize 为 ACTIVE，而 initialized 写入只在 Codex 实测。因 L63 已明确限定为 Codex → Claude Code，且 L71 列为债务，不构成误导；可选择在步 5 加「（初始化写入目前仅在 Codex 实测，见跨 Harness）」。
4. **commit A 混入 C2 初始化 diff（已披露，非阻塞）。** 若 V0.2 重跑，建议 C2 直接用可写 git 的 sandbox 先落 commit 0，使 A 只含设计。
5. **`/tmp/sw-v011-c2` 内 `EXPERIMENTS.md` Next 字段与 `STATE.md` Recommended Next 不一致（产物已披露，非阻塞）。** 仅在 clone 内，与源仓库无关。
