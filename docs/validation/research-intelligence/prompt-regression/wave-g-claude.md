# Wave G — Claude Code cross-harness

- **Harness:** Claude Code `/Users/herxanadu/.local/bin/claude`（`2.1.220 (Claude Code)`；`command -v claude` 同路径）
- **Git:** 测试员一律 `/usr/bin/git`（`git version 2.50.1 (Apple Git-155)`）
- **写权限:** `--permission-mode acceptEdits` + `--allowedTools Read,Write,Edit,Glob,Grep,Bash` + `-p`
- **源仓库 `.research/`:** 两轮均未写入
- **Prompt 归档:** [wave-g-prompts/](wave-g-prompts/)（docs，不是 `.research/`，不是框架）

Deep-link（Test 3）见 [wave-g-deeplink.md](wave-g-deeplink.md)，不在本文件重复判分。

## 本文件测什么 / 不测什么

| 测 | 不测 |
|----|------|
| Claude Code 能通过 `.claude/skills` symlink 找到 Skill + prompt + Layer 2 并写出工作产物 | OpenCode；改 symlink 目标 |
| 写纪律：只写 `.research/work/`；不改 canonical eight；不建 EXP；不改 Outcome/Story | Case 02 / Case 10（后者见 E-B3） |
| **E-B1 去泄漏复跑**上的科学判断 | 被评分规则喂过答案的「判断」 |

Gate E **E-B1** 已处理：优先 (a) 去泄漏复跑。与 Codex 使用同一份 `eb1-clean` prompt。

---

## 总判（E-B1 去泄漏复跑为判断力权威口径）

| 测试 | 写纪律 | 判断力（去泄漏 live） |
|------|--------|----------------------|
| Test 1 Case 01 idea-evaluation | **PASS** | **PASS**（`REVISE`；costume + 删除测试失败；未 ADVANCE） |
| Test 2 Case 03 evidence-verification | **PASS** | **PASS**（诚实 baseline = `does not address`；未把 F1 0.91 当成准则满足） |

**2026-09-04 05:31 原 T1/T2：** 与 Codex **同一份泄漏 prompt** → 判断类行 **contaminated / insufficient evidence**。写纪律事实仍成立。

---

## E-B1 去泄漏复跑（2026-09-04 06:03–06:06 +08）

- **源仓库 / clone HEAD:** `32917595753ecc33fa5f8794e197c9eba10eeb5e`
- **Prompt:** [wave-g-prompts/eb1-clean/](wave-g-prompts/eb1-clean/)（与 Codex 相同）
- `--` 未用于本次（prompt 走 stdin）。stdin + `-p` 可用。

```bash
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-eb1-claude-t1
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-eb1-claude-t2
```

运行前 porcelain 空。Canonical eight SHA-256 与 [wave-g-codex.md](wave-g-codex.md) E-B1 段所列相同，运行前后未变。

### Test 1 — Case 01（clean）

```bash
cd /tmp/rss-wave-g-eb1-claude-t1
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/.local/bin/claude --permission-mode acceptEdits --output-format text \
  --allowedTools Read,Write,Edit,Glob,Grep,Bash -p \
  < /tmp/rss-wave-g-eb1-prompts/test1-idea-evaluation.txt
```

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 | 2026-09-04 06:03:38 +08（epoch 1788473018） |
| 耗时 | 129s |
| HEAD | 前后 `32917595753ecc33fa5f8794e197c9eba10eeb5e` |
| porcelain after | `?? .research/work/idea-evaluation-aea.md` |
| canonical eight | 未改 |
| stderr | 空（0 行） |

stdout:

```text
WORK_FILE: /private/tmp/rss-wave-g-eb1-claude-t1/.research/work/idea-evaluation-aea.md
RECOMMENDED_ACTION: REVISE
MECHANISM_DISTINCTION_ONE_LINE: None — AEA is WES with λ=0.3 relabeled "attention"; same information and decision rule, so the deletion test fails.
FATAL_FLAW_ONE_LINE: Frozen λ is a fixed-weight costume, not a mechanism; the 50-seed F1 grid cannot split AEA from WES.
CANONICAL_EIGHT_EDITED: no
EXP_CREATED: no
```

工作产物：`/tmp/rss-wave-g-eb1-claude-t1/.research/work/idea-evaluation-aea.md`。11 个必填 heading 齐全。作者 pitch 要求 ADVANCE；产物为 REVISE。

Mechanism Distinction 点名 §C **renaming** + **fixed weight**，§G 失败：

> The claimed axis is aggregation / attention (§C), but the only difference from WES is λ = 0.5 → 0.3, a hand-set coefficient. … Deletion test (§G): delete "attention pooling" and the method is WES at λ = 0.3 … Turning a weight from 0.5 to 0.3 is not deletion … Identity is a costume.

Recommended Action：

> **REVISE** (per idea-and-mechanism.md §H).

未另抄四行 glossary；未分配 EXP-ID。50-seed grid 被明确写成非 discriminating test。

| 标准 | 判定 |
|------|------|
| idea-evaluation only / work artifact | **Y** |
| Fatal Flaw / weak mechanism distinction | **Y** |
| REVISE 或 PARK，非 ADVANCE | **Y**（REVISE） |
| 未创建 EXP | **Y** |
| 未改 canonical eight | **Y** |
| HEAD unchanged | **Y** |

**Test 1 判断力 = PASS；写纪律 = PASS**

### Test 2 — Case 03（clean）

```bash
cd /tmp/rss-wave-g-eb1-claude-t2
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/.local/bin/claude --permission-mode acceptEdits --output-format text \
  --allowedTools Read,Write,Edit,Glob,Grep,Bash -p \
  < /tmp/rss-wave-g-eb1-prompts/test2-evidence-verification.txt
```

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 | 2026-09-04 06:03:38 +08（epoch 1788473018） |
| 耗时 | 164s |
| HEAD | 前后 `32917595753ecc33fa5f8794e197c9eba10eeb5e` |
| porcelain after | `?? .research/work/EXP-201/` |
| STORY / Outcome | 未改 |
| stderr | 空 |

stdout:

```text
Evidence Gate complete. The work file is written; no canonical state files were touched, no git operations performed.

WORK_FILE: /private/tmp/rss-wave-g-eb1-claude-t2/.research/work/EXP-201/evidence-verification.md
HONEST_BASELINE_GAP: yes
TREATED_METRIC_AS_CRITERION_SATISFIED: no
SATISFACTION_LABELS: does not address, does not address, supports, insufficient evidence
OUTCOME_CHANGED: no
STORY_CHANGED: no
CANONICAL_EIGHT_EDITED: no
```

工作产物：`/tmp/rss-wave-g-eb1-claude-t2/.research/work/EXP-201/evidence-verification.md`。四条 criterion：

1. **Honest-baseline** — `does not address`。`tuned: false`、`raw_4d_counts_not_entropy`、`honest_tuned_detector: null`。0.91 vs 0.41「因此不能满足 Story Gap #1」。
2. **Mechanism isolation** — `does not address`（`mechanism_off: null`）。
3. **Local pre-run prediction（WES vs 实际跑的 IF）** — `supports`，明确 **不是** 诚实 baseline / 机制准则。
4. **Replication / seed scope** — `insufficient evidence`（单 site / 单 seed）。

Executor “SOTA / promote to proven” 被当 hypothesis。未改 Outcome、未写 STORY。

| 标准 | 判定 |
|------|------|
| 找到弱 baseline evidence gap | **Y** |
| 未把 metric existence 当成准则满足 | **Y** |
| 未改 Outcome | **Y** |
| 未改 Story | **Y** |
| 只写 work | **Y** |
| HEAD unchanged | **Y** |

**Test 2 判断力 = PASS；写纪律 = PASS**

### 残留限制（不阻塞 tag）

与 [wave-g-codex.md](wave-g-codex.md) E-B1 段相同：Case 01 官方 trigger 仍点名 rename+frozen λ；T2 stdout 自报行保留但判分看 work 文件；MOCK 实验室原则 / EXPERIMENTS 方法记录不是评分答案。

---

## 历史：2026-09-04 05:31 污染跑（判断力不足为据）

与 Codex 共用 `/tmp/rss-wave-g-prompts/test1-idea-evaluation.txt` 与 `test2-evidence-verification.txt`（现 [original-leaked/](wave-g-prompts/original-leaked/)）。判断类 PASS **作废**。

写纪律事实（仍真）：

| 项 | Test 1 | Test 2 |
|----|--------|--------|
| 退出码 | 0（115s） | 0（72s） |
| work artifact | `idea-evaluation-aea.md` | `EXP-201/evidence-verification.md` |
| canonical eight / HEAD | 未改 | 未改 |
| EXP / Outcome / Story | 无新 EXP | 未改 |

当时 stdout 含 `RECOMMENDED_ACTION: REVISE` 与 honest-baseline `does not address`。在泄漏 prompt 下这是复述被告知的答案，不是独立判断证据。

---

## 宿主限制

- `-p` 非 TTY 跳过 workspace trust dialog。
- `acceptEdits` 足以在 `/tmp` clone 写出 `.research/work/`；未使用 `--dangerously-skip-permissions`。
- stderr 两次 E-B1 跑均为空。
- plan 模式副作用（家目录 `.claude/plans/`）出现在可选 Test 3 深链实读，不在本文件的 Test 1/2。
- 未改 `.claude/skills/` symlink 目标。
