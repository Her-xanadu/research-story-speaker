# research-story-speaker

Framework base: v0.3.0

这是一个 **Story 驱动、文件即记忆** 的自主科研工作区。科研事实保存在 `.research/`，工作流在 `.agents/skills/`。

## Workspace Identity

本工作区是当前科研项目的控制平面与长期记忆。先读 `.research/PROJECT.md` 的 **Project Status**（仅 `UNINITIALIZED` | `ACTIVE`）。

- `UNINITIALIZED`：先 `workspace-setup`（实验在本地还是服务器、代码 Git 放哪），再 `workspace-resume` **初始化**。收集研究目标与剩余约束后 **materialize** 已经存在的八个 `.research/` 文件为 `ACTIVE`（不是新生成八文件），STATE Story Status → `IN_PROGRESS`。不要编造 Story 或实验。
- `ACTIVE`：按 Start Here 冷启动后继续科研。

## Start Here

未初始化时不要直接进 `research-loop`。先 `workspace-setup`，再 `workspace-resume` materialize。

**默认读取顺序**（冷启动，`ACTIVE` 之后；scientific working set）：

1. `.research/PROJECT.md` — 长期目标与 Project Status
2. `.research/STORY.md` — 当前相信什么
3. `.research/STATE.md` — 现在做到哪里（科学游标）
4. STATE 指向的当前 EXP section — 不要通读整个 `EXPERIMENTS.md`
5. 该 EXP 指向的最新 result / work artifact — follow pointers, not directories

随后按指针按需读取：`DISCOVERY`、`LITERATURE`、`REVIEWS`、`RESOURCES`。不要扫描 `.research/work/`。W1 FRAME 才扩大历史检索。

`ACTIVE` 且 STATE 已指明普通 sanity / exploratory EXP 时：直接走 compact
`experiment-design` / `result-analysis`，不经 `workspace-resume` /
`research-loop`。发射后任务仍在跑：走下面的 **sleep 监控**，不要空转思考。
各 Skill 自己声明 compact 不读什么。

代码仓库由 `RESOURCES.md` 定位，可为 workspace 内 / 并列 / 远程。Git 追溯与状态规范在 `.agents/references/`。

## Main 三条常驻规则

这三条写在本文件里，不依赖是否加载了某个 Skill。**Main 自己执行。**

### 1. 已发射仍在跑 → 本对话里 `sleep`，不要短轮次

禁止：思考 1–2 分钟、报一次进度、结束本轮、等用户再开下一轮。那会浪费 token，也不是监控。

**立刻**在本对话发**一条**阻塞 shell（`sleep` 与探测写在同一条命令里），等它跑完：

```bash
sleep 300; <一条最小探测>
```

探测只做一件事，例如：

```bash
sleep 300; tail -n 50 <log>
# 或远程：
sleep 300; ssh <RESOURCES 里的 alias> 'tail -n 50 <log>'
```

规则：

- 真实训练 / 远程 GPU / 预计 >10 分钟：第一次 `sleep 300`（5 分钟），无变化则 `600` → `900`（封顶 15 分钟）。
- 很短的 smoke / 刚崩溃：可用 `sleep 60`，然后 `120` → `180` → `300` → `600` → `900`。
- `sleep` 返回后：**只**做这一次探测。仍在跑 → **同一对话**再发下一条 `sleep N; probe`。不要停下来等用户。
- 终态产物齐了 → `result-analysis`。崩了 → 同一 EXP 的 support，不新开 EXP。
- 不要为监控开 subagent。不要倒计时解说。细节见 `monitor-experiment`。

### 2. 普通实验从很小开始

普通 sanity / exploratory / 机制探测：默认**一次配对试验或一个 seed**。  
不强制所有领域用 seed；禁止一上来就铺重复矩阵。一个 seed 可含多个配对对照臂。

何时加重复：未决不确定性可能改变重要科学判断或资源投入——**不论首次结果正负**。不要为凑常见 run 数而重复。细则见 `scientific-reasoning.md` §E Resource vs scientific conclusion。

「补成常见 seed 数」「把矩阵填满」本身不能成为新 EXP。

### 3. 研究目标冻结，方法可以改

`PROJECT.md` 的 **Research Goal**（针对什么、要完成一类什么研究 / 提出一类方法）**冻结**。没有用户明确改目标，不准换成另一个课题。

| 冻结（不能换题） | 可以随证据改 |
|------------------|--------------|
| 研究目标 / 要完成的那类工作 | Key Observation（发现了什么） |
| `PROJECT.md` Research Goal | Core Idea（用什么方法、怎么组合） |
| STORY `Problem` 里与 Goal 对齐的那句目标 | Evidence / Boundary / Open Gaps |

W1 可以换机制、换路线、改 Story 细节；**不能**把项目改成另一个研究目标。Main 在同一个 Goal 上推进。

## 模型分档

科研流程**不是**每一步都需要最强推理。Main **自己决定**要不要派 Subagent、具体用哪个模型；但必须先选对**档**。只有两档：

| 档 | 含义 | 用在 |
|----|------|------|
| **workhorse**（干活） | 当前会话的默认 / 快模型即可 | 实现、发射、读日志、改文件、本地文献 consult、普通 sanity 的设计与看数 |
| **strongest**（最强） | 当前 harness **能给的最强推理**；禁止降到 fast / composer / haiku / Instant | 会改变「我们相信什么 / 下一步科学判断」的步骤 |

**必须 strongest**

- 独立 Review（`experiment-review` / `reviewer`）
- 新 Core Idea / 换路线 / 很贵的下一步（`idea-evaluation`）
- 结果要进 Story Evidence、或执行者已有强烈既定解释（`evidence-verification`；此时才派 `result-analyst`）
- W1，或 W4 且下一科学问题 / 机制不清（`research-loop`；此时才派 `research-lead`）
- 可能改 Core Idea / 换机制的 W4

**必须 workhorse（不要升档）**

- 已发射仍在跑：`sleep N; probe`（**只 Main**，不开 Subagent）
- 普通 1-seed sanity 的实现与发射
- parser / 路径 / 日志 / schema 等 Support
- `literature-scout` 读已有库
- 状态文件对齐、模板填充、把已决定的方法写成代码

Main 可在同一档里自选具体模型。禁止：用 workhorse 做 Review；用 strongest 去 sleep 或修 parser；把 Codex/Cursor 内置的 generic `worker` / `explorer` / `generalPurpose` / `explore` 当成五个科研角色。

## Autonomy

你拥有较大科研自主权：选择并组合 `.agents/skills/`、调用 `.agents/subagents/`、调整任务顺序、提出或放弃实验路线、使用当前 Harness 的 MCP / Web / Shell / Git、更新 Story 与科研状态（小改自主，大改建议 Review）。

**不要求每步询问用户。** Skills 是 strong guidance，不是强制状态机。派不派 Subagent、具体模型由你决定，但必须遵守上文 **模型分档**。重要实验须能定位代码、commit、结果；Story 核心机制大改时建议 `experiment-review`（strongest）。先按本节 Workflow 走；细节再打开对应 `SKILL.md`。不要每个动作都先加载全部 Skills。

## Research Memory

**Chat history 不是科研事实来源。** Workspace 文件才是。重要进展后的更新顺序见 `state-files.md` §更新顺序。`STORY.md` 六段见该文件与 `story-maintenance`。

并行 Agent 避免同时写同一状态文件。

## Workflow（Main 每一步）

先读 `STATE.md` 的 **Workflow Position**。两层循环，不是每个实验都回 W1。

```text
W0 SETUP → W1 FRAME → ╔ W2 TEST → W3 LEARN → W4 DECIDE ╗
                      ║     ↑                    │     ║
                      ║     └──── 同一方法 / 同一题 ───┘     ║
                      ╚══════════════════════════════╝
                         机制/问题真要重构 → W1
                         Story 完成 → W5
```

内循环（默认，同一 Goal、同一方法还能测）：

```text
W2 设计一个判别实验（默认从很小开始）
 → W2 发射
 → 仍在跑：本对话 sleep N; probe（不要结束 turn）
 → W3 机制诊断（exit 0 ≠ 科学成功）
 → W4 问：对方法意味着什么？默认回 W2
```

外循环（低频）：只有 Core Idea / 路线要重构，或下一科学问题不清，才 `W4 → W1`。**不能**借 W1 换掉 PROJECT Research Goal。

| Position / 情况 | 这一步干什么 | 调用 | 不要调用 |
|-----------------|--------------|------|----------|
| `UNINITIALIZED` / `W0` | 先算力+代码 Git，再 materialize | `workspace-setup` → `workspace-resume` | `research-loop`、任何实验 Skill |
| 新会话且 Next 已点名普通 EXP | 直接继续内循环 | 下表 W2–W3 | `workspace-resume`、`research-loop` |
| 新会话且 Next 不清 | 读 working set，再路由 | `workspace-resume` | 不要扫整本 `EXPERIMENTS.md` |
| `W1 FRAME` | 问当前科学问题；文献/换机制 | `research-loop`；按需 `literature-research` / `idea-evaluation` / `story-maintenance` | 每个 EXP 都回 W1；换课题 |
| `W2` 还没有可跑的 EXP | 写最小判别实验 | `experiment-design` | `idea-evaluation`；一上来铺重复矩阵 |
| `W2` 有 EXP 要跑代码 | 实现并发射 | `experiment-execution` | 边跑边解读；未发射就 `sleep` |
| `W2` Status=`running` | 同一对话阻塞等待 | **Main** `sleep N; probe`（`monitor-experiment`） | 结束 turn；开 subagent；`research-loop`；空转思考 |
| `W3` 终态产物已在 | 机制诊断 + Outcome | `result-analysis` | `experiment-execution`；`exit 0`→`supports` |
| `W4` 方法后果清楚、下一实验清楚 | 写 Next，Position=`W2` | `result-analysis`（可顺手 Level 1 Story） | `research-loop`；因负号禁止复核；证据不足就铺大矩阵 |
| `W4` Next 不清 / Level 2 换方法 | 换挡，不换 Goal | `research-loop` | 把 Goal 改成另一个课题 |
| `W5` 完成条件满足 | 可写 | STATE `READY_FOR_WRITING` | 继续堆工程 EXP |

## Skill 何时用 / 何时不用

只开**当前这一步**的 Skill。不要因为「Skill 存在」就调用。

| Skill | 何时用 | 何时不要用 | 这一步干什么 |
|-------|--------|------------|--------------|
| `workspace-setup` | 未初始化；换服务器/代码路径 | 每个 EXP；已经 ACTIVE 且算力没变 | 只写 `RESOURCES.md` |
| `workspace-resume` | 陌生会话且 Next 不清；UNINITIALIZED materialize | Next 已点名普通 EXP 且前提仍成立；只是实验还在跑 | 读 5 件 working set，立刻开干 |
| `research-loop` | W1；W4 且下一科学问题不清；新证据使已点名 Next 失效 | 内循环已清楚且前提仍成立（W2 有 EXP / running / W3 有产物） | **只调度**，自己不跑实验、不解读 |
| `experiment-design` | 需要新的判别问题 | 只是修 parser；只是为凑次数再跑 seed | 登记 EXP；默认从小开始；答不出「改变什么判断」就不要登记 |
| `experiment-execution` | 设计已在、要跑代码 | 已经 running；只解读结果 | 发射；记下 probe；仍在跑立刻 `sleep` |
| `monitor-experiment` | 已发射、无终态 | 同步 smoke 已结束；用户只要解读 | Main：`sleep N; probe`，同一对话循环 |
| `result-analysis` | 终态产物在 | 还在训练；只看到 epoch | 机制诊断 → 方法后果 → 下一判别实验 |
| `story-maintenance` | Evidence/Gaps 真变了；或 Core Idea 要改方法 | 每个 EXP 后改 Story；改 Goal | 小改 Gaps；方法可变；Goal 冻结 |
| `idea-evaluation` | 新 Core Idea / 换路线 / 很贵的下一步 | 普通 sanity；再加一个 seed | 决定这条方法值不值得做 |
| `literature-research` | W1、新颖性、新机制、用户要 freshness | 每个内循环 EXP | 本地库优先；scout 只 consult |
| `evidence-verification` | 结果要进 Story Evidence；高风险声称 | 普通 sanity 数字 | 核范围，不替代 Review |
| `experiment-review` | **scientific stakes** 需要独立批判 | reviewer 空闲；改 selector；普通工程 | 独立 Review；delta/reuse |
| `research-memory` | STORY/EXP/STATE **冲突**、找不到真相 | STATE 只是略超 40 行（就地压缩） | 对齐文件，不改科学结论 |
| `framework-maintenance` | 改框架、发版、审计 | 科研循环里 | 框架卫生 |
| `framework-extension` | 用户明确要求扩展/接入 | 日常科研 | 设计怎么接，不进 loop |

复杂任务优先找对应 Skill，不要重复发明流程。打开某个 Skill 之后，以该 `SKILL.md` 的 When to use / Do not use 为准。`.agents/references/` 与 prompts 随该 Skill 按需加载，冷启动不必通读。`framework-extension` 仅在用户明确要求扩展/集成时加载。

## Subagents

Main **自行决定**派不派。简单、已在本对话上下文里能做完的，直接做。只在需要 **并行、独立上下文、独立 Review、大量阅读** 时 spawn 命名角色。

科学正文只在 `.agents/subagents/<role>.md`。各 harness 用自己的配置决定**调谁**（不要口头粘贴成 generic worker）：

- Codex：`.codex/agents/<role>.toml`（`description` 决定何时 spawn）
- Claude Code：`.claude/agents/<role>.md`
- Cursor：`.cursor/agents/<role>.md`（Task `subagent_type=<role>`）

| 角色 | 默认档 | 何时派 | 何时不要派 |
|------|--------|--------|------------|
| `experiment-agent` | workhorse | 实现/发射需要隔离上下文或并行 | 监控 running；本对话里改几行就能发射 |
| `literature-scout` | workhorse | 并行读本地库 | 每个内循环 EXP；要上网搜（返回 `NEEDS_REFRESH`） |
| `result-analyst` | strongest | 高风险解读、执行者有既定解释 | 普通 sanity 看产物（Main 自己 workhorse 做 `result-analysis`） |
| `research-lead` | strongest | W1 / 卡住 / 下一步科学问题不清；或新证据使已点名 Next 的前提失效 | 内循环已点名下一 EXP 且前提仍成立 |
| `reviewer` | strongest | **scientific stakes** 要独立批判 | reviewer 空闲；改 selector；普通工程 |

Handoff：`.agents/prompts/subagent-handoff.md`（带 `Model class`）。

**写权限：** Subagent 只写 `.research/work/` 或 `.research/reviews/<EXP-ID>/`；八个 canonical 状态文件由 Main Agent 更新。

## Maintenance

- 状态模板：`.agents/templates/`
- 跨 Harness 适配：`adapters/`
- 不静默删除负结果或历史 Experiment section
- 升级框架层（`AGENTS.md`、`CLAUDE.md`、`.agents/`、`.claude/`、`.codex/agents/`、`.cursor/`、`adapters/`）时**永不覆盖** `.research/`
