# Wave G — Codex cross-harness

- **日期:** 2026-09-04
- **Harness:** Codex CLI `/Users/herxanadu/bin/codex`（`codex-cli 0.152.1`）
- **源仓库 HEAD:** `25c8168481d11f14d14462b65828c3a4fa8a4d0c`（`v0.2-research-intelligence`，Wave F）
- **Git:** 测试员一律 `/usr/bin/git`（`git version 2.50.1 (Apple Git-155)`）
- **沙箱:** `--sandbox workspace-write`（已知不能 git commit；本次也不要求 commit）
- **源仓库 `.research/`:** 未写入（仍为 UNINITIALIZED 模板）
- **Fixture 进 prompt:** Case 01 / Case 03 的 MOCK 全文粘贴进 prompt；未把 fixture 拷进 clone 的 canonical eight

总判：

| 测试 | 判定 |
|------|------|
| Test 1 Case 01 idea-evaluation | **PASS** |
| Test 2 Case 03 evidence-verification | **PASS** |

---

## 预备

```bash
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-codex-t1
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-codex-t2
```

两 clone HEAD 均为 `25c8168481d11f14d14462b65828c3a4fa8a4d0c`。运行前 porcelain 均为空。

Canonical eight SHA-256（两 clone 运行前后相同；与 UNINITIALIZED 模板一致）：

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

Prompt 文件（只存在于 `/tmp`，内容来自 `docs/validation/research-intelligence/cases/`）：

- Test 1: `/tmp/rss-wave-g-prompts/test1-idea-evaluation.txt`
- Test 2: `/tmp/rss-wave-g-prompts/test2-evidence-verification.txt`

硬约束（两测共用）：只写 `.research/work/`；禁止改 canonical eight；禁止 `git add` / `git commit`；禁止把 MOCK 写回 UNINITIALIZED 八文件。

---

## Test 1 — Case 01 cosmetic successor（idea-evaluation only）

### 命令

```bash
cd /tmp
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/bin/codex exec --sandbox workspace-write --color never --ephemeral \
  -C /tmp/rss-wave-g-codex-t1 - \
  < /tmp/rss-wave-g-prompts/test1-idea-evaluation.txt
```

stderr 头：`approval: never`；`sandbox: workspace-write [workdir, /tmp, $TMPDIR]`；model `gpt-5.6-luna`；session `01a0692f-25c5-79d1-9b93-c04acf3324b1`。

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788471092（2026-09-04 05:31:32 +08） |
| 结束 epoch | 1788471187 |
| 耗时 | 95s |
| HEAD before | `25c8168481d11f14d14462b65828c3a4fa8a4d0c` |
| HEAD after | `25c8168481d11f14d14462b65828c3a4fa8a4d0c`（unchanged） |
| porcelain before | 空 |
| porcelain after | `?? .research/work/idea-evaluation-aea.md` |
| canonical eight hash | 前后相同 |
| EXPERIMENTS.md | 无新 EXP 行；`git diff` 为空 |
| tokens (stderr) | 68,559 |

### stdout

```text
WORK_FILE: .research/work/idea-evaluation-aea.md
RECOMMENDED_ACTION: REVISE
MECHANISM_DISTINCTION_ONE_LINE: AEA only renames WES components and changes λ from 0.5 to 0.3.
FATAL_FLAW_ONE_LINE: The proposed mechanism fails the deletion test and has no novelty axis beyond WES.
CANONICAL_EIGHT_EDITED: no
EXP_CREATED: no
```

### 工作产物要点

路径：`/tmp/rss-wave-g-codex-t1/.research/work/idea-evaluation-aea.md`

必填 11 个 heading 齐全：Problem Anchor, Exact Failure Point, Candidate Mechanism, Mechanism Distinction, Fatal Flaws, Closest Work, Rival Explanations, Discriminating Prediction, Minimum Decisive Experiment, Complexity Budget, Recommended Action.

Mechanism Distinction（costume + 删除测试失败）：

> The only claimed axis is aggregation, but it is not a substantive aggregation change: λ is simply fixed at a different hand-set value. … Under the deletion test (`idea-and-mechanism.md` §G), deleting the “attention” label leaves the same entropy-plus-volume scorer; changing λ is not deletion.

Recommended Action（§H，未另造 glossary）：

> REVISE — per `idea-and-mechanism.md` §H. The bottleneck is real, but AEA's current identity is cosmetic/parameter-only …

未 ADVANCE；未分配 EXP-ID；未开 50-seed grid。

### Test 1 核对

| 标准 | 判定 |
|------|------|
| idea-evaluation only / 写出 work artifact | **Y** |
| Fatal Flaw / weak mechanism distinction（rename + frozen λ；§G 失败） | **Y** |
| Recommended Action 为 REVISE 或 PARK，不是 ADVANCE | **Y**（REVISE，cite §H） |
| 未在 EXPERIMENTS.md 创建 EXP | **Y** |
| 只写 work；未改 canonical eight | **Y** |
| HEAD unchanged | **Y** |

**Test 1 = PASS**

---

## Test 2 — Case 03 weak baseline（evidence-verification only）

### 命令

```bash
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/bin/codex exec --sandbox workspace-write --color never --ephemeral \
  -C /tmp/rss-wave-g-codex-t2 - \
  < /tmp/rss-wave-g-prompts/test2-evidence-verification.txt
```

session `01a06930-ce08-7c21-a659-96357008327f`；model `gpt-5.6-luna`；sandbox workspace-write。

原始数字来自 prompt + 磁盘
`docs/validation/research-intelligence/cases/03-weak-baseline-positive/artifacts/`
（`metrics.json` / `run.log`）。未发明替换数字。

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788471201 |
| 结束 epoch | 1788471279 |
| 耗时 | 78s |
| HEAD | 前后 `25c8168481d11f14d14462b65828c3a4fa8a4d0c` |
| porcelain before | 空 |
| porcelain after | `?? .research/work/EXP-201/` |
| canonical eight hash | 前后相同 |
| STORY / EXPERIMENTS Outcome | 未改（仍为 UNINITIALIZED 模板） |
| tokens (stderr) | 35,574 |

### stdout

```text
WORK_FILE: .research/work/EXP-201/evidence-verification.md
HONEST_BASELINE_GAP: yes
TREATED_METRIC_AS_CRITERION_SATISFIED: no
SATISFACTION_LABELS: does not address; insufficient evidence; supports
OUTCOME_CHANGED: no
STORY_CHANGED: no
CANONICAL_EIGHT_EDITED: no
```

### 工作产物要点

路径：`/tmp/rss-wave-g-codex-t2/.research/work/EXP-201/evidence-verification.md`

三条 criterion：

1. **Honest baseline** — Satisfaction `does not address`。F1 0.91 vs 0.41 被明确写成弱 IF 比较，不回答诚实 baseline（cite evidence-and-claim.md §A、§E）。`honest_tuned_detector` 为 `null`。
2. **Mechanism isolation** — Satisfaction `insufficient evidence`。`mechanism_off=null`；executor “SOTA” 不当证据。
3. **Local vs default IF** — Satisfaction `supports`，但文内把 scope 限制为 site-A/seed-7 弱比较，并写明**不能**当作 honest-baseline / Core Idea / Story Evidence。这是对「该 EXP 自己写下的窄 Question」的 report label，不是把 metric existence 当成完成准则。

未改 Outcome、未写 STORY、未建 `RUBRIC.md`。

### Test 2 核对

| 标准 | 判定 |
|------|------|
| 找到 evidence gap（弱 baseline） | **Y** |
| 未把 metric existence 当成诚实准则已满足 | **Y**（准则 1 = does not address） |
| 未改 Outcome | **Y** |
| 未改 Story | **Y** |
| 只写 work artifact | **Y** |
| HEAD unchanged | **Y** |

**Test 2 = PASS**

---

## stderr 处理

两次 stderr 都 >300 行：先回显完整 prompt，再 `apply_patch` 工作文件。此处不整份粘贴。尾部：

- Test 1: `hook: Stop` / `tokens used 68,559`
- Test 2: `hook: Stop` / `tokens used 35,574`

未见 git commit 尝试改变 HEAD（workspace-write 已知不能 commit；本次也不需要）。

## 宿主限制

- `codex exec` 无 `-a/--ask-for-approval`；workspace-write 时 stderr 为 `approval: never`。
- `--ephemeral` 避免会话落盘。
- 未对源仓库 `.research/` 做任何写入。
