# Wave C3 — Claude Code symlink 深链

- **日期:** 2026-09-03
- **源仓库 HEAD:** `9f8a315`（`Gate A candidate-v0.1.1: init mode, Outcome/Verdict SSOT, portability fields`）
- **Clone:** `/tmp/sw-v011-c3`（从源仓库独立 clone；macOS `/tmp` → `/private/tmp`）
- **CLI:** `/Users/herxanadu/.local/bin/claude`（`2.1.220 (Claude Code)`）
- **模式:** 只读 — `--permission-mode plan` + `--allowedTools Read,Glob,Grep` + `-p`
- **判定:** **PASS**

通过标准：四个文件真实读到，且解析路径落在 `.agents/`（不是 `.claude/references/`）。

## 预备命令

```bash
rm -rf /tmp/sw-v011-c3
git clone /Users/herxanadu/Documents/story-research-workspace /tmp/sw-v011-c3
rm -rf /tmp/sw-v011-c3/.research
cp -a /tmp/sw-v011-c3/examples/mock-flow-detection/.research /tmp/sw-v011-c3/.research
```

Clone HEAD = `9f8a315`。MOCK 拷贝后根 `.research/` 覆盖 UNINITIALIZED（porcelain 仅 `.research/` 变更）。Skill symlink：`.claude/skills/experiment-design -> ../../.agents/skills/experiment-design`。`.claude/references/` 不存在。

## 实际执行的完整命令行

```bash
cd /tmp/sw-v011-c3
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude --permission-mode plan --output-format text \
  --allowedTools Read,Glob,Grep -p -- \
  "Use experiment-design. Actually open and quote the first heading of these files after following the Skill's markdown links: (1) .agents/references/experiment-record.md (2) .agents/references/git-linking.md (3) .agents/references/story-loop.md (4) one template the Skill links, e.g. EXPERIMENTS.template.md. Design the next experiment conceptually without modifying files. Report the resolved filesystem paths you used for each. If a link resolved under .claude/ instead of .agents/, say FAIL."
```

只读参数：`-p`（非交互）+ `--permission-mode plan` + `--allowedTools Read,Glob,Grep` + `--output-format text`。`--` 终结选项，prompt 为位置参数。

## 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788366873 |
| 结束 epoch | 1788367107 |
| 耗时 | 234s |
| 运行前 porcelain | 仅 MOCK 覆盖的 `.research/`（8 文件 `M` + `reviews/.gitkeep` `D` + `work/.gitkeep` `M`） |
| 运行后 porcelain | 与运行前相同 |
| clone 内是否越权写文件 | 否（无需 `git checkout` 还原） |
| stdout 行数 | 39 |
| stderr 行数 | 0 |

clone 外副作用：plan 模式把报告写到用户家目录 `/Users/herxanadu/.claude/plans/use-experiment-design-actually-open-linked-avalanche.md`（不在 clone 内，未改 Skill 链接）。

## 解析路径表

磁盘实读标题由测试员对照 clone 内文件首行核对，与 Claude stdout 一致。路径为 `realpath`（`/tmp` → `/private/tmp`）。

| # | 目标 | Claude 报告的解析路径 | 首行标题 | 落在 `.agents/`？ |
|---|------|----------------------|----------|-------------------|
| 1 | `experiment-record.md` | `/private/tmp/sw-v011-c3/.agents/references/experiment-record.md` | `# Experiment Record Reference` | **Y** |
| 2 | `git-linking.md` | `/private/tmp/sw-v011-c3/.agents/references/git-linking.md` | `# Git Linking Reference` | **Y** |
| 3 | `story-loop.md` | `/private/tmp/sw-v011-c3/.agents/references/story-loop.md` | `# Story Loop Reference` | **Y** |
| 4 | 模板（`EXPERIMENTS.template.md`） | `/private/tmp/sw-v011-c3/.agents/templates/EXPERIMENTS.template.md` | `# Experiments` | **Y** |

无任何路径落在 `.claude/references/` 或其它 `.claude/` 内容目录。

**项 (4) 备注（不改判 PASS）：** `experiment-design` 的 `SKILL.md` 不直接链到模板；相对链接只有 `../../references/{experiment-record,git-linking,story-loop,state-files}.md`。Claude 按 prompt 示例打开了 `.agents/templates/EXPERIMENTS.template.md`（模板头回链 `experiment-record.md`）。深链相对路径仍解析到 `.agents/`，不是 `.claude/`。未改 Skill 链接。

Claude 对 (1)–(3) 的解析说明：harness 经 symlink `.claude/skills/experiment-design` 进入物理文件 `.agents/skills/experiment-design/SKILL.md`；`../../references/<file>` 相对物理目标解析为 `.agents/references/`。

## 核对表

| # | 标准 | 判定 | 依据 |
|---|------|------|------|
| 1 | 真实读到 `experiment-record.md` | **Y** | 首行 `# Experiment Record Reference` 与磁盘一致 |
| 2 | 真实读到 `git-linking.md` | **Y** | 首行 `# Git Linking Reference` 与磁盘一致 |
| 3 | 真实读到 `story-loop.md` | **Y** | 首行 `# Story Loop Reference` 与磁盘一致 |
| 4 | 真实读到一个 Skill 相关模板 | **Y** | 首行 `# Experiments`；路径 `.agents/templates/EXPERIMENTS.template.md` |
| 5 | 四条路径均在 `.agents/` | **Y** | 见上表；无 `.claude/references/` |
| 6 | 若解析到 `.claude/` 则 FAIL | **N/A** | 未发生 |
| 7 | clone 内无写入 | **Y** | porcelain 前后相同 |
| 8 | 未改 Skill 链接 | **Y** | 未编辑 `.agents/` / `.claude/skills/` |

**C3 通过（PASS）。** 不需要把 Skill 相对链接改为 `../../../.agents/`。

## 原始 stdout

```text
`ExitPlanMode` isn't available in this session, so I'll report directly.

## Path resolution report

`experiment-design` lives physically at `.agents/skills/experiment-design/SKILL.md`, reached from the harness via a **thin symlink**:

```text
.claude/skills/experiment-design -> ../../.agents/skills/experiment-design
```

The skill's links are `../../references/<file>`. Resolved against the symlink's physical target, `../../` = `.agents/`, so all links land in `.agents/references/`. `.claude/references/` **does not exist** — the links can only resolve under `.agents/`. No FAIL.

| # | Resolved filesystem path | First heading | Under `.claude/`? |
|---|---|---|---|
| 1 | `/private/tmp/sw-v011-c3/.agents/references/experiment-record.md` | `# Experiment Record Reference` | No |
| 2 | `/private/tmp/sw-v011-c3/.agents/references/git-linking.md` | `# Git Linking Reference` | No |
| 3 | `/private/tmp/sw-v011-c3/.agents/references/story-loop.md` | `# Story Loop Reference` | No |
| 4 | `/private/tmp/sw-v011-c3/.agents/templates/EXPERIMENTS.template.md` | `# Experiments` | No |

**Result: no FAIL** — all four resolve under `.agents/`.

One nuance for item (4): the `experiment-design` SKILL.md does **not** directly link a template. Its links point only at the three references above plus `state-files.md`. The template I opened is the one matching the skill's output (`.research/EXPERIMENTS.md`); its own header points back to `.agents/references/experiment-record.md`, the reference the skill *does* link. (Sibling skills `literature-research` and `experiment-review` *do* link `../../templates/*.template.md` directly.)

## Next experiment (conceptual, no files changed)

The highest-judgment gap is **STORY Open Gap #1**: does the 3-feature detector hold up on a *real* CICIDS subset against a *valid* strong baseline? EXP-001's `supports` is explicitly MOCK-only, and its toy Isolation Forest (F1=0.5455) is recorded as "not a valid strong control" in DISCOVERY Negative Discoveries. So:

**EXP-002 — Three-Feature vs Strong Full-Feature Baseline on Real CICIDS Subset**

- **Question:** F1 gap between 3-flow-feature LR vs an effective full-feature strong baseline on a real multi-family CICIDS subset — within the 5% completion threshold?
- **Comparisons:** 3-feature LR (candidate) · Random Forest/GBM on full features (primary strong baseline) · tuned Isolation Forest on full features (secondary control) · optional 3-feature IF (isolates feature-count vs model-choice).
- **Expected outcomes:** *support* → Core Idea validated on real data, proceed to attack-family ablation (Gap #3) + normalization (Gap #2); *refute* → narrow Boundary and trigger `experiment-review` before a Core-Idea edit; *narrow* → gap concentrated in specific families feeds EXP-003.
- **Codebase:** `flow-detector` (RESOURCES) at `../../../story-research-code`.
- **Compute:** local CPU; stratified 80/20 split, seeds 42/601/602; precision/recall/F1 + FPR + latency + memory.
- **Traceability:** planned Entry `experiments/EXP-002/run.py`, Results `results/EXP-002/`; baseline Git = existing commit `b0621e2e…` (no invented commits).

**Flagged blocker:** `RESOURCES.md` registers only the synthetic `CICIDS2017-MOCK-Subset` — no real subset exists yet, so execution must first register the real subset and add a strong-baseline + loader to the codebase.

The full report + design is written to the plan file at `/Users/herxanadu/.claude/plans/use-experiment-design-actually-open-linked-avalanche.md`. No research files were modified.
```

## 原始 stderr

```text
(empty)
```
