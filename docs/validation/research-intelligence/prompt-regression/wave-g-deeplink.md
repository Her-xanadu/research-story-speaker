# Wave G — Test 3 Claude Skill symlink deep-link

- **日期:** 2026-09-04
- **源仓库 HEAD:** `25c8168481d11f14d14462b65828c3a4fa8a4d0c`（`v0.2-research-intelligence`）
- **Clone:** `/tmp/rss-wave-g-deeplink`（macOS `/tmp` → `/private/tmp`）
- **Git:** `/usr/bin/git`（`git version 2.50.1 (Apple Git-155)`）
- **模式:** 文件系统证明为主（不需要长 agent run）。另加一次与 v0.1.1 C3 同型的短 Claude 只读实读。
- **判定:** **PASS**

通过标准（C3 同型，不改写 symlink 为 `../../../.agents/`）：

- `.claude/skills/idea-evaluation` 与 `evidence-verification` 是指向 `.agents/skills/<name>` 的相对 symlink。
- SKILL.md 内 `../../references/research-intelligence/…` 相对**物理** skill 目录解析到 `.agents/references/research-intelligence/`。
- 若把 symlink 目录当逻辑根，会落到不存在的 `.claude/references/` —— 这正是 C3 要避免的错误解析，**不要**靠改链接目标来修。
- clone HEAD 不变；porcelain 空。

---

## 预备命令

```bash
rm -rf /tmp/rss-wave-g-deeplink
/usr/bin/git clone --branch v0.2-research-intelligence --single-branch \
  /Users/herxanadu/research-story-speaker /tmp/rss-wave-g-deeplink
```

Clone HEAD = `25c8168481d11f14d14462b65828c3a4fa8a4d0c`。根 `.research/` 保持 UNINITIALIZED（本测试不读科研状态）。`.claude/references/` **不存在**。

### symlink（未改写）

```text
.claude/skills/idea-evaluation        -> ../../.agents/skills/idea-evaluation
.claude/skills/evidence-verification  -> ../../.agents/skills/evidence-verification
```

不是 `../../../.agents/`。与 v0.1.1 C3 的 `.claude/skills/<skill> -> ../../.agents/skills/<skill>` 相同。

---

## A. 文件系统证明（充分）

脚本：`/tmp/rss-wave-g-prompts/test3-deeplink.py`（对 clone 只读）。

```bash
python3 /tmp/rss-wave-g-prompts/test3-deeplink.py /tmp/rss-wave-g-deeplink
```

退出码 0。HEAD 前后 `25c8168481d11f14d14462b65828c3a4fa8a4d0c`。porcelain 前后空。

`realpath` 后物理目录：

```text
/private/tmp/rss-wave-g-deeplink/.agents/skills/idea-evaluation
/private/tmp/rss-wave-g-deeplink/.agents/skills/evidence-verification
```

SKILL.md 中指向 Layer 2 的相对链接（未改）：

| Skill | 链接 |
|-------|------|
| idea-evaluation | `../../references/research-intelligence/scientific-reasoning.md` |
| idea-evaluation | `../../references/research-intelligence/idea-and-mechanism.md` |
| idea-evaluation | `../../references/research-intelligence/experiment-thinking.md` |
| evidence-verification | `../../references/research-intelligence/evidence-and-claim.md` |
| evidence-verification | `../../references/research-intelligence/scientific-reasoning.md` |

### 错误逻辑根 vs 物理根

从 **symlink 路径** `normpath(.claude/skills/<skill>/../..)` 会得到：

```text
/tmp/rss-wave-g-deeplink/.claude/references/research-intelligence/<file>
exists=False
```

从 **realpath 物理目录** 解析同一相对链接：

```text
/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/<file>
exists=True
```

| 目标 | 物理解析存在 | 落在 `.agents/` | 逻辑 `.claude/references/` 存在 |
|------|----------------|-----------------|----------------------------------|
| scientific-reasoning.md | Y | Y | N |
| idea-and-mechanism.md | Y | Y | N |
| experiment-thinking.md | Y | Y | N |
| evidence-and-claim.md | Y | Y | N |

磁盘首行（与实读一致）：

| 文件 | 首行 |
|------|------|
| scientific-reasoning.md | `# Scientific Reasoning` |
| idea-and-mechanism.md | `# Idea and Mechanism` |
| experiment-thinking.md | `# Experiment Thinking` |
| evidence-and-claim.md | `# Evidence and Claim` |

**文件系统：PASS。** 不需要把 Skill 相对链接改成 `../../../.agents/`。

---

## B. 短 Claude 只读实读（C3 同型，非判定所必需）

只读参数：`-p` + `--permission-mode plan` + `--allowedTools Read,Glob,Grep`。

```bash
cd /tmp/rss-wave-g-deeplink
gtimeout --signal=TERM --kill-after=15 360 \
  /Users/herxanadu/.local/bin/claude --permission-mode plan --output-format text \
  --allowedTools Read,Glob,Grep -p -- \
  "$(cat /tmp/rss-wave-g-prompts/test3-claude-follow.txt)"
```

| 项 | 值 |
|----|----|
| CLI | `/Users/herxanadu/.local/bin/claude` `2.1.220` |
| 退出码 | 0 |
| 开始 epoch | 1788471328 |
| 结束 epoch | 1788471402 |
| 耗时 | 74s |
| porcelain after | 空（与 before 相同） |
| HEAD | unchanged |
| stderr | 空 |

Claude 报告的解析路径（`realpath`，`/tmp` → `/private/tmp`）：

| target | resolved realpath | first heading | under `.agents/`? |
|--------|-------------------|---------------|-------------------|
| scientific-reasoning.md | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/scientific-reasoning.md` | `# Scientific Reasoning` | Y |
| idea-and-mechanism.md | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/idea-and-mechanism.md` | `# Idea and Mechanism` | Y |
| experiment-thinking.md | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/experiment-thinking.md` | `# Experiment Thinking` | Y |
| evidence-and-claim.md | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/evidence-and-claim.md` | `# Evidence and Claim` | Y |

无任何路径落在 `.claude/references/`。Claude 说明：经 symlink 进入物理 `SKILL.md` 后，`../../references/` = `.agents/references/`。

clone 内未改 Skill 链接。plan 模式可能在家目录写 `.claude/plans/`（clone 外，与 C3 相同，不改判）。

### stdout

```text
The task is complete — it was entirely read-only (no files modified). Here are the results.

**Resolved symlink realpaths used:**
- `.claude/skills/idea-evaluation/SKILL.md` → `/private/tmp/rss-wave-g-deeplink/.agents/skills/idea-evaluation/SKILL.md`
- `.claude/skills/evidence-verification/SKILL.md` → `/private/tmp/rss-wave-g-deeplink/.agents/skills/evidence-verification/SKILL.md`

Because the SKILL.md realpaths live under `.agents/skills/…`, the `../../references/research-intelligence/…` links resolve to `.agents/references/research-intelligence/` (which exists), **not** `.claude/references/` (which does not exist). No FAIL.

| target | resolved realpath | first heading | under .agents/? |
| --- | --- | --- | --- |
| scientific-reasoning.md (idea-evaluation) | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/scientific-reasoning.md` | `# Scientific Reasoning` | Y |
| idea-and-mechanism.md (idea-evaluation) | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/idea-and-mechanism.md` | `# Idea and Mechanism` | Y |
| experiment-thinking.md (idea-evaluation) | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/experiment-thinking.md` | `# Experiment Thinking` | Y |
| evidence-and-claim.md (evidence-verification) | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/evidence-and-claim.md` | `# Evidence and Claim` | Y |
| scientific-reasoning.md (evidence-verification, again) | `/private/tmp/rss-wave-g-deeplink/.agents/references/research-intelligence/scientific-reasoning.md` | `# Scientific Reasoning` | Y |

All five links resolved under `.agents/references/` — no FAIL cases. No files or symlinks were modified.
```

---

## 核对表

| # | 标准 | 判定 | 依据 |
|---|------|------|------|
| 1 | idea-evaluation 相对链接解析到 `.agents/references/research-intelligence/` | **Y** | 物理 realpath + 文件存在 |
| 2 | evidence-verification 同上 | **Y** | 同上 |
| 3 | `.claude/references/` 不存在（逻辑错根） | **Y** | `ls` 无此目录 |
| 4 | 未把 symlink 改成 `../../../.agents/` | **Y** | 仍为 `../../.agents/skills/<name>` |
| 5 | clone porcelain / HEAD 不变 | **Y** | 空 porcelain；HEAD `25c8168…` |
| 6 | Claude 实读路径均在 `.agents/`（可选） | **Y** | 上表；无 FAIL |

**Test 3 = PASS**
