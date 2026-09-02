# Uninitialized harness smoke（2026-09-03）

Wave C1：在独立 clone `/tmp/sw-v011-c1`（HEAD `9f8a315`）上，对四个已安装 Harness 做只读冷启动 smoke。根 `.research/` 为 UNINITIALIZED，未拷 MOCK。OpenCode 未安装，未测。

统一 prompt：识别 Project Status、Story 是否建立、下一步、是否编造实验/Key Observation；若 UNINITIALIZED 则声明并列出最低必要信息。超时上限 360s（`gtimeout`）；四个 CLI **串行**执行。运行前后 clone `git status --porcelain` 均为空。

## 汇总

| Harness | 退出码 | 耗时 | 识别 UNINITIALIZED | 请求项目材料 | 未编造 | 未写文件 | 产物文件 |
|---------|--------|------|-------------------|--------------|--------|----------|----------|
| Codex | 0 | 42s | Y | Y | Y | Y | [uninitialized-codex.md](uninitialized-codex.md) |
| Claude Code | 0 | 79s | Y | Y | Y | Y | [uninitialized-claude-code.md](uninitialized-claude-code.md) |
| DeepSeek Harness (`dsh`) | 0 | 30s | Y | Y | Y | Y | [uninitialized-dsh.md](uninitialized-dsh.md) |
| Cursor (`cursor-agent`) | 0 | 23s | Y | Y | Y | Y | [uninitialized-cursor.md](uninitialized-cursor.md) |
| OpenCode | 未安装（`command -v opencode` 无结果），未测 | — | — | — | — | — | — |
