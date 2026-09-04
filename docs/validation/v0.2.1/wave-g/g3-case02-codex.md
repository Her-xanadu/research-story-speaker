# V0.2.1 Wave G — G3 Case 02 (Codex)

Real-mechanism successor. Goal: Idea-gate is not always-reject.

## Standard fields

| Field | Value |
|-------|--------|
| Harness | Codex CLI `/Users/herxanadu/bin/codex`（`codex-cli 0.153.0-alpha.5`） |
| Model/version | `gpt-5.6-luna`（`~/.codex/config.toml` `model =`；`approval_policy = never`） |
| Context relation | **fresh-clone** `/tmp/rss-v021-g3-codex` from `v0.2.1-wave-g-g3g5` @ `e612f8ee340130d00b01e0d9346db0096db2b6a2` |
| Host memory status | inspected; fixture-relevant hits **0** (see below) |
| Agent-visible prompt SHA | `e9ea468d952f72ecb7c222f87378b7330fd45dd971f2854da9308beb44e69f2f` ([prompts/g3-case02-agent-visible.txt](prompts/g3-case02-agent-visible.txt)) |
| Grader-visible file SHA | `643089ed94eca352fd9c082eb8bc23d3220fea38e7815c2be5cf1504e7c7fb31`（`live-cases/case02/grader.md`；**不在 clone / 不在 prompt**） |
| Files read | Skill + prompt + Layer 2 named by those two + `artifacts/prrw-spec.md` + on-disk UNINITIALIZED eight (see list) |
| Files written | `.research/work/idea-evaluation-prrw.md` only（clone） |
| Canonical files changed? | **no**（八文件 SHA-256 与 UNINITIALIZED 模板相同） |
| Skill/Prompt/RI loaded | `idea-evaluation` SKILL + `.agents/prompts/idea-evaluation.md`；`idea-and-mechanism.md`；`scientific-reasoning.md`；`experiment-thinking.md`。**未**加载 `deep-literature-mode.md` / `skill-evolution.md` |
| Token count | input `178527`（cached input `140032`）+ output `3812` + reasoning `1120` |
| Wall time | 101s（2026-09-04 14:26:20–14:28:01 +08） |
| Observed decision | **ADVANCE** |
| Expected range | **ADVANCE**（grader，事后） |
| PASS / MISS | **PASS** |
| Limitation | 见 §Limitation |

Git 测试员：`/usr/bin/git` `2.50.1 (Apple Git-155)`。源仓库与本 worktree 的 `.research/` 仍为 UNINITIALIZED（未写入）。Clone HEAD 前后均为 `e612f8e`。Session `01a06b18-c46b-7ef2-9c66-85bc9deea147`。`--ephemeral --sandbox workspace-write --json`。

## 本文件测什么 / 不测什么

| 测 | 不测 |
|----|------|
| 未泄漏 live：clone 无 `grader.md`、无历史 `cases/` 评分 README、无 `prompt-regression/` | OpenCode / Claude G3（本分支只要求 ≥1 harness） |
| Case 02 真实信息源变化时 Idea-gate 能否 **ADVANCE** | Case 01/03/10 |
| 写纪律：只写 work；不改 canonical eight；不建 EXP | compact-path token（G1） |

禁止把 expected ADVANCE 写进 agent prompt。评分在跑完后用源仓库 `grader.md`。

## Isolation

```bash
/usr/bin/git clone --branch v0.2.1-wave-g-g3g5 --single-branch \
  /tmp/rss-v021-g3g5 /tmp/rss-v021-g3-codex
# then delete from clone:
#   docs/validation/research-intelligence/{cases,prompt-regression,live-cases,gate-reviews,source-audit}
#   docs/validation/v0.2.1
# copy only artifacts/prrw-spec.md to clone-root artifacts/
```

Agent 可见：`live-cases/case02/input.md` 全文 + 作者方法笔记 `prrw-spec.md`。Prompt 泄漏扫描（`expected action` / `PASS condition` / `ADVANCE is wrong` / `must output REVISE` / `grader` / `typically does not address` / `should not trigger`）= **0**。

## Host memory check

```text
Host memory check:
- Path inspected: /Users/herxanadu/.codex/memories
- Hits: 0 (PRRW | Port-Rarity | MOCK-WBS | idea-evaluation-prrw | live-cases/case02 | 02-real-mechanism)
- Fixture-relevant hits: 0
- Hits expected-action / PASS condition / ADVANCE is a miss: 0
- Disclose: 4 unrelated files mention ADVANCE in other projects (PROJECT-001 DO_NOT_ADVANCE). Not this fixture.
- Agent also rg'd ~/.codex/memories/MEMORY.md during the run; MEMORY HIT section empty.
```

`memories = true` in Codex config. Fixture-relevant = 0 → 本 run **计为 clean judgment evidence**。

## Claude file-read trace

```text
Claude file-read trace:
- Capture: n/a (not Claude)
- Trace present: n/a (not Claude)
- Opened grader.md: n/a (file deleted from clone; Codex jsonl shows no grader path)
- Opened live-cases/README.md: n/a (directory removed from clone)
- Opened historical cases/*/README.md or prompt-regression reports: no (removed from clone)
```

Codex `--json` 命令轨迹（读）：

1. `.agents/skills/idea-evaluation/SKILL.md`
2. `.agents/prompts/idea-evaluation.md`
3. `~/.codex/memories/MEMORY.md`（宿主记忆自检；无命中）
4. `.agents/references/research-intelligence/idea-and-mechanism.md`
5. `.agents/references/research-intelligence/scientific-reasoning.md`
6. `.agents/references/research-intelligence/experiment-thinking.md`
7. `.research/{PROJECT,STORY,STATE,DISCOVERY,EXPERIMENTS,LITERATURE,RESOURCES}.md`（磁盘 UNINITIALIZED 模板；科学状态以 prompt MOCK 为准）
8. `artifacts/prrw-spec.md`

写：`.research/work/idea-evaluation-prrw.md`。随后自检 11 个 heading 与 `ADVANCE`。

## 跑法

```bash
gtimeout --signal=TERM --kill-after=15 900 \
  /Users/herxanadu/bin/codex exec \
    --sandbox workspace-write --color never --ephemeral --json \
    -C /tmp/rss-v021-g3-codex \
    -o /tmp/rss-v021-g3-logs/last.txt - \
  < docs/validation/v0.2.1/wave-g/prompts/g3-case02-agent-visible.txt
```

退出码 0。stderr 有 websocket `426 Upgrade Required`（`ws://127.0.0.1:10100/v1/responses`）；会话仍完成。

## 写纪律

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| HEAD | 前后 `e612f8ee340130d00b01e0d9346db0096db2b6a2` |
| porcelain after（相对 clone 的科学树） | `?? .research/work/idea-evaluation-prrw.md`（另有测试员放置的 `?? artifacts/prrw-spec.md`） |
| canonical eight | 未改（SHA 与 E-B1 / UNINITIALIZED 模板一致） |
| EXPERIMENTS / EXP-ID | 无 |
| 源仓库 `.research/` | 空 porcelain |

Canonical eight SHA-256（跑后 = 跑前）：

```text
PROJECT.md     0c26aa3d475ee3e139bd027b28f8b0500c45f4cb3b97d9118c22aec9245caaee
STORY.md       bbae0e0c14d9d42f8116fafa1564496ef5e0b889ff68847ef9a7f25ed37f9f06
STATE.md       d740257dfb859a9a6c47de0f62a8d7e337d1900175f49acdecf78fc5d3b1f44a
DISCOVERY.md   92e6bb88f7bd88f773d57274cebd436558e24ec82e7681a6f2adcc8c8c622f43
EXPERIMENTS.md 705c3b3a9693babf5aab8eacfe13426db538168e92f2271654f840f69103b8f9
LITERATURE.md  a3d51d6965336168b6c82fef042b04b73fe7c5080e35aa9d401fd6ef9d2ec3c7
REVIEWS.md     f43531f7004d0011bcd832c7952fb58ba7130e49f8164fac4ef78db8cf80b22a
RESOURCES.md   6baec0f1a7ef17319abf7ae9645aa73b35ab9430040a98c3574ccc843a78a8bd
```

stdout（last message）：

```text
WORK_FILE: .research/work/idea-evaluation-prrw.md
RECOMMENDED_ACTION: ADVANCE
MECHANISM_DISTINCTION_ONE_LINE: PRRW 将信息源从 packet-size entropy 改为 site-local destination-port rarity rank。
FATAL_FLAW_ONE_LINE: none
CANONICAL_EIGHT_EDITED: no
EXP_CREATED: no
```

工作产物副本：[artifacts/g3-idea-evaluation-prrw.md](artifacts/g3-idea-evaluation-prrw.md)（sha256 `65273144e670c585b9dcb66a3759e1add4b3827066a1cfd2c45d90473331c6dd`）。11 个必填 heading 齐全。未抄 §H 四行 glossary。未分配 EXP-ID。最小测试为 Full PRRW vs **mechanism-off**（窗内 shuffle ranks）vs WES，单 split，命中低流量 rare-burst；明确排除 50-seed grid。

Mechanism Distinction（信息源，非改名）：

> 机制变化位于 `information` 轴：WES 使用 packet-size entropy，而 PRRW 使用相对同 site 最近 K=12 个窗口的 destination-port rarity rank…删除 rarity-rank 组件会删除该历史端口信息…因而通过 `idea-and-mechanism.md §C` 的 deletion test（§G）。

Recommended Action：

```text
ADVANCE
```

## Grader（事后，源仓库）

`live-cases/case02/grader.md` 期望 `Recommended Action: ADVANCE`。对照：

| 标准 | 判定 |
|------|------|
| work file 存在 | **Y** |
| Recommended Action ADVANCE | **Y** |
| 最小测试点名 mechanism-off + rare-burst / low-volume | **Y** |
| 无 EXP-ID；Skill 未改写 Core Idea | **Y** |
| 不是 50-seed protocol | **Y** |
| 未因“名字新”而 PARK/REVISE | **Y** |

**G3 判断力 = PASS；写纪律 = PASS。** Idea-gate 在本 fixture 上不是 always-reject。

未改 fixture 以迎合结果。

## Limitation

- Token 高于 V0.2 E-B1 Case 01（~38k）：本 run 把 Layer 2 与模板八文件经 shell 整段读回，input 被放大；这是 Codex 执行风格，不是 compact-path 测量。
- Agent 读了磁盘 UNINITIALIZED 八文件；prompt 已声明 MOCK 才是科学状态。判断正文锚定 MOCK / `prrw-spec.md`，未把模板写成 Story。
- stderr websocket 426；不影响完成。
- 仅 Codex 一 harness（任务允许）。
- Recommended Action 段只写 token `ADVANCE`；§H 引用在 Minimum Decisive Experiment 段，未在 Action 同行重复 cite。
