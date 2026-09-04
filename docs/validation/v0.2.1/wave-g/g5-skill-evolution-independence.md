# V0.2.1 Wave G — G5 Skill-evolution independence

独立 scorer + grader 不泄漏 + candidate 不 auto-deploy。不要求 `deserves review`。

## Standard fields

| Field | Value |
|-------|--------|
| Harness | Claude Code CLI `/Users/herxanadu/.local/bin/claude` **2.1.220**（scorer）；候选作者不在本 session 写 hunk |
| Model/version | Scorer: `deepseek-v4-pro[1m]`（Claude CLI init 自报；非 Anthropic Claude 权重） |
| Context relation | **fresh-context**（新 CLI session `5b947f03-cb87-4731-9253-7f2ac4c61fcf`；无作者聊天史） |
| Host memory status | n/a for Claude CLI scorer；Codex 记忆未注入本评分包 |
| Agent-visible prompt SHA | Scorer prompt `0b3b7593d99bd8cb6bd554bf7a698f0acfa7d3c463ef8e5e043f859f94c37ae7`（[prompts/g5-scorer-prompt.md](prompts/g5-scorer-prompt.md)） |
| Grader-visible file SHA | **not shown**（包内无 `grader.md`、无 Wave H `REPORT.md`、无 “expected PASS”） |
| Files read | 见 §Scorer file-read trace（仅 `/tmp/rss-v021-g5-scorer/**`） |
| Files written | **none**（Read/Glob only；canonical Skills 未改） |
| Canonical files changed? | **no** |
| Skill/Prompt/RI loaded | 评分包内的 `skill-evolution.md` 副本（v0.2.1）；**未**加载 Idea-gate 去跑科学 |
| Token count | input `28963` + cache_read `74368` + output `6774`（API usage） |
| Wall time | 117s（2026-09-04 14:29:41–14:31:38 +08；`duration_api_ms` 115335） |
| Observed decision | **reject** |
| Expected range | `deserves review` **或** `reject`（协议允许；禁止规定 PASS） |
| PASS / MISS | 本项不是 Idea-gate PASS/MISS。独立性：**成立**。§E 结论：**reject** |
| Limitation | 见 §Limitation |

## 本文件测什么 / 不测什么

| 测 | 不测 |
|----|------|
| author ≠ scorer；different-family + fresh-context | 把 Wave H 候选 merge 进 Skills |
| scorer 看不到 expected PASS / Case 02 grader ADVANCE | 再 live 跑一遍 candidate prompt 的 Case 02 harness |
| §E 合取：failure improved **且** protection intact | 强行 `deserves review` |
| auto-deploy 未发生 | 新的 atomic hunk |

## Roles

| Role | Identity |
|------|----------|
| Candidate author | Wave H 历史作者（Grok / Cursor）。候选仍只在 `docs/validation/research-intelligence/prompt-regression/wave-h-idea-evaluation/candidate/`。**本 Grok 会话未新写 hunk、未部署。** |
| Executor | Wave H 已有 baseline vs candidate 产物（`failure/`、`protection/`）。本轮未重跑 VAEG harness。 |
| Scorer | Claude CLI 2.1.220，模型 `deepseek-v4-pro[1m]`，cwd `/tmp/rss-v021-g5-scorer`（无 RSS Skills、无 grader） |

```text
Model relation: different-family
Context relation: fresh-context
Assurance: not lowered (author Grok ≠ scorer DeepSeek via Claude CLI)
```

若只计 “Claude CLI 品牌 / Anthropic 权重”，则本机 CLI 实际走了 DeepSeek：仍记录为 **different-family**，并在 Limitation 写明权重来源。

## Protection / held-out

| Kind | Fixture | Scorer saw |
|------|---------|------------|
| Failure (known class) | Wave H VAEG work files | `failure/baseline-work.md` + `candidate-work.md`（**不是** failure README 的 Expected behavior 段） |
| Protection | Case 10 | `live-cases/case10/input.md` + Wave H `protection/*-decision.md` |
| Held-out | Case 02 | `live-cases/case02/input.md` + `artifacts/prrw-spec.md` **only**（无 `grader.md`） |

候选：item 8 收紧（ADVANCE 仅当 *this gate* 认为 deletion test 通过）。v0.2.1 canonical item 8 仍为原文（prompt sha256 `7119264c11f31ce20e3cee4ee0edd400051b440a4651e4bd6c6315c31bb7b51c`）。Skill sha256 `5330da47bc7ddaeee0a775a18612a9cbd50e0115295353ffc35785e76876f13c`。候选副本 sha256 仍 `6447d7b981425764cda8f3743497536c08d86084f08c11871655481a913116ba`。

## Isolation of the scorer prompt

评分包泄漏扫描：除 `skill-evolution.md` 作为 **禁语表 owner** 列出的短语外，包内无 `expected PASS`、无 `Recommendation: reject`、无 `grader.md`。Scorer 指令明确：**没有**规定推荐 token。

## Scorer file-read trace

```text
Claude file-read trace:
- Capture: --output-format stream-json --verbose
- Trace present: yes
- Opened grader.md: no (not in packet)
- Opened live-cases/README.md: no
- Opened historical cases/*/README.md or prompt-regression REPORT.md: no
- Opened Wave H REPORT.md: no
```

Read（绝对路径均在 `/private/tmp/rss-v021-g5-scorer/`）：

- `skill-evolution.md`
- `candidate/BEHAVIOR.md`
- `candidate/idea-evaluation.md.patch`
- `candidate/idea-evaluation.md.candidate`
- `failure/baseline-work.md`
- `failure/candidate-work.md`
- `protection/baseline-decision.md`
- `protection/candidate-decision.md`
- `canonical-item8.txt`
- `hashes.txt`
- `case10/input.md`
- `case02/input.md`
- `case02/artifacts/prrw-spec.md`

Glob `**/*` 一次。Grep 0。Write 0。`permission_denials` 空。

## Scorer output（verbatim token block）

全文：[artifacts/g5-scorer-stdout.txt](artifacts/g5-scorer-stdout.txt)

```text
RECOMMENDATION: reject
TRIGGER_HOLDS: no
FAILURE_IMPROVED: no
PROTECTION_INTACT: vacuous
HELD_OUT_CASE02_REGRESSION: no
AUTO_DEPLOY: no
CANONICAL_SKILLS_MODIFIED: no
MODEL_RELATION: different-family
CONTEXT_RELATION: fresh-context
```

Rationale（scorer）：baseline 在 VAEG 上已经是 `REVISE`，candidate 未改变 Recommended Action，故 §A 触发不成立且 §E 第一合取失败；Case 10 保护成立但 vacuous（item 8 在 When-to-use 之后）；held-out Case 02 的 hunk **不会**变成 never-ADVANCE（gate 自己认定 deletion test 通过仍可 ADVANCE）。

## Proof: no auto-deploy

本 worktree 对 `.agents/` 的 `git diff` 为空。Canonical `idea-evaluation` Skill / prompt 哈希与 `origin/v0.2.1-micro-hardening` @ `e612f8e` 相同。候选仍只存在于 `docs/validation/.../wave-h-idea-evaluation/candidate/`。

## G5 结论

| 证明项 | 结果 |
|--------|------|
| 独立 scorer | **Y**（different-family + fresh-context + 无作者历史） |
| grader 不泄漏 | **Y**（无 grader / 无 expected PASS / 无 REPORT 推荐） |
| candidate 未 merge 到 Skills | **Y** |
| 协议推荐 | **reject**（与 Wave H 同向，但是 **另一模型族独立得出**，不是复述 REPORT） |

## Limitation

- Claude CLI 品牌 ≠ Anthropic Claude 权重；本机 init 模型为 `deepseek-v4-pro[1m]`。独立性相对 Grok 作者仍成立。
- Held-out Case 02 是对 hunk 文本的 dry-score，不是 candidate prompt 的第二次 live Codex。G3 的 ADVANCE 是 **canonical** prompt 的 live 证据，不能算 candidate 的 live held-out。
- Failure/protection 比较复用 Wave H executor 产物，未在 v0.2.1 上重跑 VAEG（v0.2.1 item 8 字节与当时基线句相同；prompt 其余部分有 Wave B 落地句）。
- 本 coordinator session 是 Grok：只组包与归档，**未**代 scorer 填写 `RECOMMENDATION`。
