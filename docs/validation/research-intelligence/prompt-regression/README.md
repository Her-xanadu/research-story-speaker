# Prompt-regression evidence

V0.2 Gate D / Wave G 行为证据。Fixture 原文在
[`../cases/`](../cases/)。**不要**把 case MOCK 拷进框架仓库根 `.research/`。

跨 Harness 跑在 `/tmp` clone 上；测试员 git 一律 `/usr/bin/git`。

## Wave G（2026-09-04）

| 文件 | 内容 | Test 1 | Test 2 | Test 3 |
|------|------|--------|--------|--------|
| [wave-g-codex.md](wave-g-codex.md) | Codex `0.152.1` Case 01 + Case 03 | PASS | PASS | — |
| [wave-g-claude.md](wave-g-claude.md) | Claude Code `2.1.220` Case 01 + Case 03 | PASS | PASS | — |
| [wave-g-deeplink.md](wave-g-deeplink.md) | `.claude/skills` → `.agents/references/research-intelligence/` | — | — | PASS |

无 CLI 缺失；没有 `not tested`。

## 其它

- [gate-d.md](gate-d.md) — Gate D 十 case 静态对照（若已落盘）。
