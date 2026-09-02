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

## Host CLI notes (moved from adapters/README)

Lead-host `command -v` on 2026-09-02:

```text
/Users/herxanadu/bin/codex
/Users/herxanadu/.local/bin/claude
/Users/herxanadu/.nvm/versions/node/v22.22.0/bin/dsh
/Users/herxanadu/.local/bin/cursor-agent
opencode not found
```

Non-interactive commands that worked:

```bash
codex exec --sandbox read-only --color never --ephemeral -C <workspace> "<prompt>"
claude --permission-mode plan --output-format text --allowedTools Read,Glob,Grep -p -- "<prompt>"
dsh --profile headless "<prompt>"
cursor-agent -p --trust --mode ask --workspace <workspace> "<prompt>"
```

Observed CLI details:

- Codex: `codex exec --help` has no `-a/--ask-for-approval` (interactive `codex` only); `--sandbox read-only` logs `approval: never`. Smoke also saw chronicle warnings and `ws://127.0.0.1:10100/v1/responses` HTTP 426 (did not block exit 0). Codex read `workspace-resume` before PROJECT-first order (content still correct; 6/7).
- Claude Code: `--allowedTools` is variadic; put the prompt after `-p` or `--`.
- DSH: `dsh --profile headless --help` has no read-only flag; enforce read-only via prompt and git porcelain.
- Cursor: `-p/--print` has write and shell access; overlay `--mode ask` or `--mode plan` for read-only.

