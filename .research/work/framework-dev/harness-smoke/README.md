# Harness smoke（2026-09-02）

本机已安装四个 Agent Harness CLI 的非交互、只读冷启动 smoke。原始 stdout/stderr 与七项核对见各产物文件。OpenCode 未安装，未测。

统一 prompt 见各产物中的完整命令行。超时上限 360s（`gtimeout`）；四个 CLI **串行**执行。运行前后工作区 `git status --porcelain` 均为空，无一 harness 越权写工作区文件。

## 汇总

| Harness | 退出码 | 耗时 | 七项 Y 数 | 产物文件 |
|---------|--------|------|-----------|----------|
| Codex | 0 | 42s | 6/7 | [codex.md](codex.md) |
| Claude Code | 0 | 37s | 7/7 | [claude-code.md](claude-code.md) |
| DeepSeek Harness (`dsh`) | 0 | 16s | 7/7 | [dsh.md](dsh.md) |
| Cursor (`cursor-agent`) | 0 | 27s | 7/7 | [cursor.md](cursor.md) |
| OpenCode | 未安装（`command -v opencode` 无结果），未测 | — | — | — |

Claude Code 第一次因 `--allowedTools` 吃掉 prompt 以 exit 1 失败（2s）；立刻用 `--` 终结选项重跑成功。上表按成功那次计。
