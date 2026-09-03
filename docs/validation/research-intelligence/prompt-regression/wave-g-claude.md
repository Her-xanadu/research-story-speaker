# Wave G — Claude Code cross-harness

- **日期:** 2026-09-04
- **Harness:** Claude Code `/Users/herxanadu/.local/bin/claude`（`2.1.220 (Claude Code)`；`command -v claude` 同路径）
- **源仓库 HEAD:** `25c8168481d11f14d14462b65828c3a4fa8a4d0c`（`v0.2-research-intelligence`）
- **Git:** 测试员一律 `/usr/bin/git`（`git version 2.50.1 (Apple Git-155)`）
- **写权限:** `--permission-mode acceptEdits` + `--allowedTools Read,Write,Edit,Glob,Grep,Bash` + `-p`
- **源仓库 `.research/`:** 未写入
- **Fixture 进 prompt:** 与 Codex 同一份 `/tmp/rss-wave-g-prompts/test1-idea-evaluation.txt` 与 `test2-evidence-verification.txt`

总判：

| 测试 | 判定 |
|------|------|
| Test 1 Case 01 idea-evaluation | **PASS** |
| Test 2 Case 03 evidence-verification | **PASS** |

Deep-link（Test 3）见 [wave-g-deeplink.md](wave-g-deeplink.md)，不在本文件重复判分。

---

## 预备

```bash
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-claude-t1
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-claude-t2
```

两 clone HEAD `25c8168481d11f14d14462b65828c3a4fa8a4d0c`。运行前 porcelain 空。Canonical eight SHA-256 与 [wave-g-codex.md](wave-g-codex.md) 所列相同，运行前后未变。

`--` 未用于本次（prompt 走 stdin）。先前 smoke 的 `--allowedTools` 吞 prompt 问题：stdin + `-p` 可用。

---

## Test 1 — Case 01 cosmetic successor（idea-evaluation only）

### 命令

```bash
cd /tmp/rss-wave-g-claude-t1
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/.local/bin/claude --permission-mode acceptEdits --output-format text \
  --allowedTools Read,Write,Edit,Glob,Grep,Bash -p \
  < /tmp/rss-wave-g-prompts/test1-idea-evaluation.txt
```

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788471092（2026-09-04 05:31:32 +08） |
| 结束 epoch | 1788471207 |
| 耗时 | 115s |
| HEAD | 前后 `25c8168481d11f14d14462b65828c3a4fa8a4d0c` |
| porcelain before | 空 |
| porcelain after | `?? .research/work/idea-evaluation-aea.md` |
| canonical eight hash | 前后相同 |
| stderr | 空（0 行） |

### stdout

```text
WORK_FILE: /private/tmp/rss-wave-g-claude-t1/.research/work/idea-evaluation-aea.md
RECOMMENDED_ACTION: REVISE
MECHANISM_DISTINCTION_ONE_LINE: AEA changes no mechanism axis — it renames WES's entropy→token and mix→attention and freezes λ at 0.3; no new information source, same threshold decision, deletion test fails.
FATAL_FLAW_ONE_LINE: Identity is a costume (renaming + fixed weight, §C/§G) with no discriminating test (§D), so ADVANCE is blocked.
CANONICAL_EIGHT_EDITED: no
EXP_CREATED: no
```

### 工作产物要点

路径：`/tmp/rss-wave-g-claude-t1/.research/work/idea-evaluation-aea.md`

11 个必填 heading 齐全。Mechanism Distinction 点名 §C costume **renaming** + **fixed weight**，§G 删除测试失败：

> The only non-cosmetic delta is a coefficient hand-frozen to 0.3 instead of 0.5. That is the §C costume "**fixed weight**" … layered on top of "**renaming**" … Deletion test (§G): … **The deletion test fails.**

Recommended Action：

> **REVISE** — per idea-and-mechanism.md §H.

未另抄四行 glossary；未 ADVANCE；未分配 EXP-ID。

### Test 1 核对

| 标准 | 判定 |
|------|------|
| idea-evaluation only / work artifact | **Y** |
| Fatal Flaw / weak mechanism distinction | **Y** |
| REVISE 或 PARK，非 ADVANCE | **Y**（REVISE） |
| 未创建 EXP | **Y** |
| 未改 canonical eight | **Y** |
| HEAD unchanged | **Y** |

**Test 1 = PASS**

---

## Test 2 — Case 03 weak baseline（evidence-verification only）

### 命令

```bash
cd /tmp/rss-wave-g-claude-t2
gtimeout --signal=TERM --kill-after=15 720 \
  /Users/herxanadu/.local/bin/claude --permission-mode acceptEdits --output-format text \
  --allowedTools Read,Write,Edit,Glob,Grep,Bash -p \
  < /tmp/rss-wave-g-prompts/test2-evidence-verification.txt
```

### 结果

| 项 | 值 |
|----|----|
| 退出码 | 0 |
| 开始 epoch | 1788471220 |
| 结束 epoch | 1788471292 |
| 耗时 | 72s |
| HEAD | 前后 `25c8168481d11f14d14462b65828c3a4fa8a4d0c` |
| porcelain before | 空 |
| porcelain after | `?? .research/work/EXP-201/` |
| canonical eight hash | 前后相同 |
| STORY / Outcome | 未改 |
| stderr | 空 |

### stdout

```text
WORK_FILE: /private/tmp/rss-wave-g-claude-t2/.research/work/EXP-201/evidence-verification.md
HONEST_BASELINE_GAP: yes
TREATED_METRIC_AS_CRITERION_SATISFIED: no
SATISFACTION_LABELS: does not address, does not address, supports
OUTCOME_CHANGED: no
STORY_CHANGED: no
CANONICAL_EIGHT_EDITED: no
```

### 工作产物要点

路径：`/tmp/rss-wave-g-claude-t2/.research/work/EXP-201/evidence-verification.md`

1. Honest-baseline：**does not address**。明确写 `F1 0.91 vs 0.41 therefore does not answer "beats an honest baseline" — metric existence ≠ criterion satisfied`（§A、§E）。baseline `tuned: false`、`features: raw_4d_counts_not_entropy`。
2. Mechanism isolation：**does not address**（`mechanism_off: null`）。
3. Local vs default IF：**supports**，但 Summary 限制为 site-A/seed-7，禁止升成 Story Evidence / 诚实 baseline / 机制句。

Executor “SOTA / promote to proven” 被标成 hypothesis。未改 Outcome、未写 STORY。

### Test 2 核对

| 标准 | 判定 |
|------|------|
| 找到弱 baseline evidence gap | **Y** |
| 未把 metric existence 当成准则满足 | **Y** |
| 未改 Outcome | **Y** |
| 未改 Story | **Y** |
| 只写 work | **Y** |
| HEAD unchanged | **Y** |

**Test 2 = PASS**

---

## 宿主限制

- `-p` 非 TTY 跳过 workspace trust dialog。
- `acceptEdits` 足以在 `/tmp` clone 写出 `.research/work/`；未使用 `--dangerously-skip-permissions`。
- stderr 两次均为空。
- plan 模式副作用（家目录 `.claude/plans/`）出现在可选 Test 3 深链实读，不在本文件的 Test 1/2。
- 未改 `.claude/skills/` symlink 目标。
