# Prompt-regression evidence

V0.2 Gate D / Wave G 行为证据。Fixture 原文在
[`../cases/`](../cases/)。**不要**把 case MOCK 拷进框架仓库根 `.research/`。

跨 Harness 跑在 `/tmp` clone 上；测试员 git 一律 `/usr/bin/git`。

Agent 可见 prompt 原文：[wave-g-prompts/](wave-g-prompts/)（docs 归档）。

## 本目录测什么 / 不测什么

Wave G live 证明三件事：(1) Codex / Claude Code harness 可移植；(2) 规定 heading 的输出形状；(3) 写纪律（canonical eight / EXP / Outcome / Story 不写）。科学判断以 **E-B1 去泄漏复跑**为准。2026-09-04 05:31 原 T1/T2 把评分规则写进 agent prompt，判断类 PASS 为 **contaminated / insufficient evidence**。

## Wave G（2026-09-04）

| 文件 | 内容 | T1 写纪律 | T1 判断力 | T2 写纪律 | T2 判断力 | Test 3 |
|------|------|-----------|-----------|-----------|-----------|--------|
| [wave-g-codex.md](wave-g-codex.md) | Codex `0.152.1` Case 01 + Case 03 | PASS | **PASS**（E-B1 clean；`REVISE`） | PASS | **PASS**（E-B1 clean；诚实 baseline `does not address`） | — |
| [wave-g-claude.md](wave-g-claude.md) | Claude Code `2.1.220` Case 01 + Case 03 | PASS | **PASS**（E-B1 clean；`REVISE`） | PASS | **PASS**（E-B1 clean；诚实 baseline `does not address`） | — |
| [wave-g-deeplink.md](wave-g-deeplink.md) | `.claude/skills` → `.agents/references/research-intelligence/` | — | — | — | — | PASS |

原 05:31 T1/T2 判断类行：contaminated / insufficient evidence（prompt 见 [wave-g-prompts/original-leaked/](wave-g-prompts/original-leaked/)）。写纪律事实仍真。E-B1 复跑 prompt：[wave-g-prompts/eb1-clean/](wave-g-prompts/eb1-clean/)。

无 CLI 缺失。E-B1 命中，**不**把原泄漏 PASS 算作 Q1 证据。残留限制（Case 01 官方 trigger 仍点名 rename+frozen λ；T2 stdout 自报行格式保留）见两份报告，**不阻塞 tag**。

## E-B3 Case 10 live protection（2026-09-04）

| 文件 | Harness | 判定 |
|------|---------|------|
| [wave-g-case10-protection.md](wave-g-case10-protection.md) | Codex `0.152.1` | **PASS**（Claude Code **not tested**） |

Prompt：[wave-g-prompts/eb3-case10-ordinary-exploratory.txt](wave-g-prompts/eb3-case10-ordinary-exploratory.txt)。落盘无 Idea-gate / Evidence-gate / Reviewer / `result-analyst` 工作文件。Agent 仍打开了仓内 Case 10 README（评分段在 fixture 里）；详见该报告 Limitation。

## 其它

- [gate-d.md](gate-d.md) — Gate D 十 case 静态对照（instruction-level；不是 live 判断证据）。
- [wave-h-idea-evaluation/REPORT.md](wave-h-idea-evaluation/REPORT.md) — skill-evolution dogfood（reject）。
