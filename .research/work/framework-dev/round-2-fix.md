# Gate 2 Round 2 — Fixer

范围：仅 `.agents/skills/*/SKILL.md`、`.agents/subagents/*.md`、`.agents/prompts/*.md`；本报告新建于 `.research/work/framework-dev/round-2-fix.md`。
未改：`AGENTS.md`、`README.md`、`CLAUDE.md`、`.agents/references/`、`.agents/templates/`、`adapters/`、`.claude/`、以及 `.research/` 下除本文件外的任何文件。无 git add/commit/push。无脚本文件。

---

## 必须修复

- **C1** — 五个 Skill + `subagent-handoff.md`：删除四段箭头代码块，改为链接 `[state-files.md](…) §更新顺序`。`result-analysis` / `story-maintenance` 另写本 Skill 负责 EXPERIMENTS+DISCOVERY / STORY 环。
- **M1** — `research-loop/SKILL.md`：删除 gap 优先级四条与反重复扫描清单，改为链接 `story-loop.md` §Gap 优先级 / §反重复；停滞段保留「信号 → §停滞处理」。118 行（原 132；M2/N9 增句后未再为行数拆文件）。
- **M2** — `research-loop/SKILL.md` 路由表 Experiment 行 Delegate 改为 `` `experiment-design` → `experiment-execution` → `result-analysis` ``；并行时 `experiment-agent` / `result-analyst`，handoff 用 `subagent-handoff.md`。未展开三 Skill 流程。
- **M3** — `reviewer.md` 新增 `## Independence policy`（唯一定义：模型家族 → 不同模型 → 外部 Reviewer MCP；最低 `independence: same-model-fresh-context`；禁同会话自审；无 subagent 时新会话跑 prompt 仍须记录限制）。`experiment-review` Skill 只链接该节。
- **M4** — `experiment-review/SKILL.md`：删除五段输出列表，改为「输出结构以 active prompt 为准，最低五段见 `reviewer.md` Required output headings」。顺手让 `method-review.md` / `result-review.md` 使用同一 headings，仅保留各自填空问题。
- **M5** — `prompts/experiment-review.md`：删除 When to review 与 Orchestration 长步骤，改为一句「何时审、如何派见 experiment-review Skill」；保留 decision matrix、REVIEWS.md synthesis template、「subagent 不写 REVIEWS.md」。
- **M6** — `literature-research/SKILL.md`：删除 LITERATURE 八字段枚举，改为链接 `LITERATURE.template.md` 与 `state-files.md` §LITERATURE.md；保留五透镜映射到 Relation 字段一句。
- **M7** — `framework-maintenance/SKILL.md`：删除 Canonical owner 表、adapter 六问正文、尺寸数字；改为链接 `state-files.md` §单一事实来源（§20）/ §尺寸建议 与 `adapters/`。Phase 11 八项检查表改为「对照 reference 是否漂移」。120 行。
- **M8** — `experiment-execution/SKILL.md`：删除路径恢复五步枚举与绑定字段逗号列表；改为绑定/恢复只遵循 `git-linking.md` §每个正式 Experiment 的最小绑定 / §路径恢复五步法；本 Skill 跑前冻结 commit、跑后把路径写入 EXPERIMENTS。

## 建议项

- **N1** — `research-loop`：`Do not use a fixed state machine`。
- **N2** — `research-loop`：步骤 6 改为「若仍自主推进，回到 gap 判断；允许跳步」。
- **N3** — `experiment-execution`：删除 STORY `trivial factual corrections`，STORY 一律不碰。`research-memory` Updates 加「压缩/搬移细节，不改 Problem/Core Idea 的科学主张」。
- **N4** — `research-loop`：删除「运行成功 ≠ 科学成功」。`result-analysis` / `experiment-execution` 保留。
- **N5** — `research-loop` / `workspace-resume` / `research-memory`：不再画六段箭头链，改为「六段见 STORY.md / story-maintenance」。`story-maintenance` 作为 owner 保留箭头链。
- **N6** — `experiment-review` Skill Traceability 代码块改为一句链接 `git-linking.md` §完整追溯链。
- **N7** — `workspace-resume`：「chat history 不是记忆」改为见 `AGENTS.md` Research Memory 与 `state-files.md`。
- **N8** — `experiment-execution`：`Main Agent (this skill) still owns EXPERIMENTS/STATE updates after the subagent returns`。
- **N9** — `research-loop` invoke 与 Deviation：独立判断下一步时派 `research-lead`（读 STORY/STATE/DISCOVERY，写 `.research/work/`）。

未编号但按任务 Canonical 锚点一并做：`method-review.md` / `result-review.md` 五段 heading 以 `reviewer.md` 为准。`experiment-design` 与其余 4 个 subagent 无 R2 项，未改。

## 未能处理 / 范围外

- 无必须项未能处理。
- `research-loop` 118 行（目标约 100）：删除 SSOT 复述后因 M2 路由句与 N9 `research-lead` 仍略超 100，仍在 ~100–130 指引内，未拆文件。
- 工作区同时出现其他会话改动（`.agents/references/`、`.agents/templates/`、`.research/` 状态文件、`round-3-review.md`）。Fixer 未触碰；提交由 Lead 统一处理。

---

## 自检

### C1 四段箭头

```text
rg -n --hidden "EXPERIMENTS.*→.*DISCOVERY.*→.*STORY|EXPERIMENTS.*->.*DISCOVERY.*->.*STORY" \
  .agents/skills .agents/subagents .agents/prompts
```

命中 **0**（rg exit 1）。允许的链接句只写 `§更新顺序`，不再画出四段箭头。

### 行数

| 文件 | 行数 |
|------|------|
| `.agents/skills/experiment-design/SKILL.md` | 104 |
| `.agents/skills/experiment-execution/SKILL.md` | 102 |
| `.agents/skills/experiment-review/SKILL.md` | 106 |
| `.agents/skills/framework-maintenance/SKILL.md` | 120 |
| `.agents/skills/literature-research/SKILL.md` | 107 |
| `.agents/skills/research-loop/SKILL.md` | 118 |
| `.agents/skills/research-memory/SKILL.md` | 115 |
| `.agents/skills/result-analysis/SKILL.md` | 102 |
| `.agents/skills/story-maintenance/SKILL.md` | 109 |
| `.agents/skills/workspace-resume/SKILL.md` | 110 |
| `.agents/subagents/experiment-agent.md` | 93 |
| `.agents/subagents/literature-scout.md` | 92 |
| `.agents/subagents/research-lead.md` | 79 |
| `.agents/subagents/result-analyst.md` | 92 |
| `.agents/subagents/reviewer.md` | 123 |
| `.agents/prompts/experiment-review.md` | 57 |
| `.agents/prompts/method-review.md` | 59 |
| `.agents/prompts/result-review.md` | 60 |
| `.agents/prompts/subagent-handoff.md` | 66 |

### Frontmatter 与六要素

10 个 `SKILL.md`：`name` 均等于目录名；均含 `When to use` / `Goal` / `Default flow` / `Reads` / `Updates` / `Deviation allowed`（各 6 个 `##` 标题）。frontmatter `description` 未改。

### M2 路由表

```text
rg -n "experiment-design|experiment-execution|result-analysis" .agents/skills/research-loop/SKILL.md
```

```text
64:| Untested mechanism, empirical answer | Experiment | `experiment-design` → `experiment-execution` → `result-analysis` |
```

### 相对链接与小节标题

Markdown 相对链接解析：**0 missing**。核验过的 `§` 目标均存在：

- `state-files.md`：`## 更新顺序`、`### LITERATURE.md`、`### STORY.md`、`### 单一事实来源（§20）`、`## 尺寸建议`、`## 反重复规则`、`## Story 完成条件`
- `story-loop.md`：`## Gap 优先级`、`## 反重复`、`## 停滞处理`
- `git-linking.md`：`## 每个正式 Experiment 的最小绑定`、`## 路径恢复五步法`、`## 推荐代码布局（§15）`、`## 完整追溯链`
- `reviewer.md`：`## Independence policy`、`## Required output`（内含五段 heading）
- `LITERATURE.template.md`、`REVIEWS.template.md`、`AGENTS.md` 文件存在

### git status（本 Fixer 范围内）

```text
 M .agents/prompts/experiment-review.md
 M .agents/prompts/method-review.md
 M .agents/prompts/result-review.md
 M .agents/prompts/subagent-handoff.md
 M .agents/skills/experiment-execution/SKILL.md
 M .agents/skills/experiment-review/SKILL.md
 M .agents/skills/framework-maintenance/SKILL.md
 M .agents/skills/literature-research/SKILL.md
 M .agents/skills/research-loop/SKILL.md
 M .agents/skills/research-memory/SKILL.md
 M .agents/skills/result-analysis/SKILL.md
 M .agents/skills/story-maintenance/SKILL.md
 M .agents/skills/workspace-resume/SKILL.md
 M .agents/subagents/reviewer.md
?? .research/work/framework-dev/round-2-fix.md
```

全工作区另有并发改动（非本 Fixer）：`.agents/references/`、`.agents/templates/`、若干 `.research/` 状态/review 文件、`round-3-review.md`。
