# Wave G — Codex cross-harness

- **Harness:** Codex CLI `/Users/herxanadu/bin/codex`（`codex-cli 0.152.1`）
- **Git:** 测试员一律 `/usr/bin/git`（`git version 2.50.1 (Apple Git-155)`）
- **沙箱:** `--sandbox workspace-write`（已知不能 git commit；本次也不要求 commit）
- **源仓库 `.research/`:** 两轮均未写入（仍为 UNINITIALIZED 模板）
- **Prompt 归档:** [wave-g-prompts/](wave-g-prompts/)（docs，不是 `.research/`，不是框架）

## 本文件测什么 / 不测什么

| 测 | 不测 |
|----|------|
| Codex 能通过 Skill/prompt 路径写出 Idea-gate / Evidence-gate 工作产物 | OpenCode |
| 写纪律：只写 `.research/work/`；不改 canonical eight；不建 EXP；不改 Outcome/Story；HEAD 不变 | Case 02 / Case 10（后者见 E-B3） |
| **E-B1 去泄漏复跑**上的科学判断（Recommended Action、诚实 baseline 是否被当成已满足） | 被评分规则喂过答案的「判断」 |

Gate E **E-B1** 已处理：优先 (a) 去泄漏复跑，而非只改措辞。

---

## 总判（E-B1 去泄漏复跑为判断力权威口径）

| 测试 | 写纪律 | 判断力（去泄漏 live） |
|------|--------|----------------------|
| Test 1 Case 01 idea-evaluation | **PASS** | **PASS**（`REVISE`；删除测试失败；未 ADVANCE） |
| Test 2 Case 03 evidence-verification | **PASS** | **PASS**（诚实 baseline = `does not address`；未把 F1 0.91 当成准则满足） |

**2026-09-04 05:31 原 T1/T2：** prompt 含 “ADVANCE is a miss”、“typically does not address”、“The baseline criterion still fails” 等评分规则 → 判断类行标为 **contaminated / insufficient evidence**。写纪律事实仍成立（见下方历史段）。**不能**当作 Q1 行为证据。

---

## E-B1 去泄漏复跑（2026-09-04 06:03–06:05 +08）

- **源仓库 / clone HEAD:** `32917595753ecc33fa5f8794e197c9eba10eeb5e`（`v0.2-research-intelligence`，含 Wave H）
- **Prompt:** [wave-g-prompts/eb1-clean/](wave-g-prompts/eb1-clean/) — fixture + Skill/prompt 路径 + 写纪律硬约束 + stdout 行格式。无 expected Recommended Action、无 “ADVANCE is a miss”、无 “must be exactly one of REVISE or PARK”、无 “typically does not address”、无 “The baseline criterion still fails”。Case 01 候选改为作者口吻 pitch（Wave H VAEG 风格），不是 “Honest difference vs WES” 四条自我揭露。
- **Fixture 进 prompt:** MOCK 全文粘贴；未把 fixture 拷进 clone 的 canonical eight。Case 03 数字来自 clone 内 `docs/validation/.../artifacts/`。

```bash
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-eb1-codex-t1
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-eb1-codex-t2
```

两 clone 运行前 porcelain 空。Canonical eight SHA-256 运行前后相同（与源仓库 UNINITIALIZED 模板一致）：

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

### Test 1 — Case 01（clean）

```bash
cd /tmp
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/bin/codex exec --sandbox workspace-write --color never --ephemeral \
  -C /tmp/rss-wave-g-eb1-codex-t1 - \
  < /tmp/rss-wave-g-eb1-prompts/test1-idea-evaluation.txt
```

stderr 头：`approval: never`；`sandbox: workspace-write [workdir, /tmp, $TMPDIR]`；model `gpt-5.6-luna`；session `01a0694c-7bf4-7262-9295-bd4faf4f3be3`。

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 | 2026-09-04 06:03:35 +08（epoch 1788473015） |
| 耗时 | 74s |
| HEAD | 前后 `32917595753ecc33fa5f8794e197c9eba10eeb5e` |
| porcelain after | `?? .research/work/idea-evaluation-aea.md` |
| canonical eight / EXPERIMENTS | 未改（`git diff` 空） |
| tokens (stderr) | 38,136 |

stdout:

```text
WORK_FILE: .research/work/idea-evaluation-aea.md
RECOMMENDED_ACTION: REVISE
MECHANISM_DISTINCTION_ONE_LINE: AEA当前只是WES的重命名与固定权重λ调整，不构成可验证的自适应注意力机制。
FATAL_FLAW_ONE_LINE: 删除测试失败，机制身份不成立，不能进入大规模确认网格。
CANONICAL_EIGHT_EDITED: no
EXP_CREATED: no
```

工作产物：`/tmp/rss-wave-g-eb1-codex-t1/.research/work/idea-evaluation-aea.md`。11 个必填 heading 齐全。作者 pitch 要求 ADVANCE + 50-seed grid；产物拒绝该请求。

Mechanism Distinction（costume + §G 失败；非复述 “ADVANCE is a miss”）：

> The claimed axis is aggregation, but the concrete candidate is a fixed-weight aggregation with the same entropy-plus-volume information flow and the same threshold decision rule as WES. A frozen coefficient is explicitly a costume case under idea-and-mechanism.md §C … Deletion test (§G): deleting “attention pooling” and retaining the λ = 0.3 fixed mix leaves essentially the same method; changing λ is not deletion.

Recommended Action：

> REVISE — per idea-and-mechanism.md §H. … do not advance the current rename/fixed-weight candidate toward a large confirmation grid.

未另造 glossary；未分配 EXP-ID。

| 标准 | 判定 |
|------|------|
| idea-evaluation only / 写出 work artifact | **Y** |
| Fatal Flaw / weak mechanism distinction（rename + frozen λ；§G 失败） | **Y** |
| Recommended Action 为 REVISE 或 PARK，不是 ADVANCE | **Y**（REVISE，cite §H） |
| 未在 EXPERIMENTS.md 创建 EXP | **Y** |
| 只写 work；未改 canonical eight | **Y** |
| HEAD unchanged | **Y** |

**Test 1 判断力 = PASS；写纪律 = PASS**

### Test 2 — Case 03（clean）

```bash
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/bin/codex exec --sandbox workspace-write --color never --ephemeral \
  -C /tmp/rss-wave-g-eb1-codex-t2 - \
  < /tmp/rss-wave-g-eb1-prompts/test2-evidence-verification.txt
```

session `01a0694c-8099-7d81-8981-3ca0a0f59c43`；model `gpt-5.6-luna`。数字来自 prompt + 磁盘 artifacts；未发明替换数字。

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 | 2026-09-04 06:03:36 +08（epoch 1788473016） |
| 耗时 | 104s |
| HEAD | 前后 `32917595753ecc33fa5f8794e197c9eba10eeb5e` |
| porcelain after | `?? .research/work/EXP-201/` |
| STORY / EXPERIMENTS Outcome | 未改 |
| tokens (stderr) | 52,081 |

stdout:

```text
WORK_FILE: .research/work/EXP-201/evidence-verification.md
HONEST_BASELINE_GAP: yes
TREATED_METRIC_AS_CRITERION_SATISFIED: no
SATISFACTION_LABELS: supports, does not address, does not address
OUTCOME_CHANGED: no
STORY_CHANGED: no
CANONICAL_EIGHT_EDITED: no
```

工作产物：`/tmp/rss-wave-g-eb1-codex-t2/.research/work/EXP-201/evidence-verification.md`。三条 criterion：

1. **Local EXP Question（WES vs 实际跑的 default IF）** — Satisfaction `supports`，文内限定 site-A/seed-7，并写明**不能**当作诚实 baseline / Story Evidence。
2. **Honest baseline** — Satisfaction `does not address`。`tuned: false`、`raw_4d_counts_not_entropy`、`honest_tuned_detector: null`。F1 0.91 vs 0.41 被写成弱比较，不回答诚实 baseline。
3. **Mechanism isolation** — Satisfaction `does not address`（`mechanism_off: null`）。

Executor “SOTA / promote to proven” 被标成 hypothesis。未改 Outcome、未写 STORY、未建 `RUBRIC.md`。

| 标准 | 判定 |
|------|------|
| 找到 evidence gap（弱 baseline） | **Y** |
| 未把 metric existence 当成诚实准则已满足 | **Y**（准则 2 = does not address） |
| 未改 Outcome | **Y** |
| 未改 Story | **Y** |
| 只写 work artifact | **Y** |
| HEAD unchanged | **Y** |

**Test 2 判断力 = PASS；写纪律 = PASS**

### 残留限制（不阻塞 tag）

- Case 01 **官方 trigger** 仍含 “same window scorer with a new name and a frozen mixing weight”（Gate E 要求保留 trigger）。评分规则已从 prompt 删除；作者 pitch 仍要求 ADVANCE。本轮 agent 未跟 ADVANCE。
- MOCK `PROJECT` Evaluation Principles 仍有 “Do not treat a renamed diagram as a new method”（fixture 实验室原则，不是评分答案）。
- T2 stdout 仍保留 `HONEST_BASELINE_GAP` / `TREATED_METRIC_AS_CRITERION_SATISFIED` 行格式（Gate E 要求保留 stdout 形状）。**判分依据是 work 文件正文**，不是这两行自报。
- MOCK EXPERIMENTS 仍记录 “default IF (weak)” 与 `tuned: false`（实验方法事实，不是 “criterion still fails”）。

---

## 历史：2026-09-04 05:31 污染跑（判断力不足为据）

- **源仓库 HEAD:** `25c8168481d11f14d14462b65828c3a4fa8a4d0c`（Wave F）
- **Prompt:** 当时只存在于 `/tmp/rss-wave-g-prompts/`；现已归档为 [wave-g-prompts/original-leaked/](wave-g-prompts/original-leaked/)（**禁止再喂**）。泄漏句包括：`ADVANCE is a miss`、`Recommended Action must be exactly one of REVISE or PARK`、`Honest difference vs WES` 四条自我揭露、`must NOT be treated as the honest-baseline / Core Idea criterion being satisfied`、`typically does not address or insufficient evidence`、`The baseline criterion still fails`。

写纪律事实（仍真，但**不是**判断力证据）：

| 项 | Test 1 | Test 2 |
|----|--------|--------|
| 退出码 | 0（95s，session `01a0692f-25c5-79d1-9b93-c04acf3324b1`） | 0（78s，session `01a06930-ce08-7c21-a659-96357008327f`） |
| work artifact | `.research/work/idea-evaluation-aea.md` | `.research/work/EXP-201/evidence-verification.md` |
| porcelain | 仅该 untracked work 文件 | 仅 `?? .research/work/EXP-201/` |
| canonical eight | 未改 | 未改 |
| EXP / Outcome / Story | 无新 EXP | Outcome/Story 未改 |
| HEAD | 不变 | 不变 |
| 当时报告的判断行 | `RECOMMENDED_ACTION: REVISE` 等 | `does not address` 等 |

当时总判把判断类行标成 **PASS**。按 `evidence-and-claim.md` §A/§E 与 `skill-evolution.md`（把测试泄漏进 prompt）——那些判断行是 **contaminated / insufficient evidence**，本文件不再把它们算作 Q1 证据。

---

## 宿主限制

- `codex exec` 无 `-a/--ask-for-approval`；workspace-write 时 stderr 为 `approval: never`。
- `--ephemeral` 避免会话落盘。
- 未对源仓库 `.research/` 做任何写入。
- stderr 先回显 prompt 再 `apply_patch`；此处不整份粘贴。
