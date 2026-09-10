# Claude Code Adapter

This project uses **AGENTS.md** as the canonical project guide.

**On session start:**

1. Read `AGENTS.md` first (identity, 模型分档, Workflow, Subagents).
2. Use `.agents/skills/` as the canonical skill library (native discovery via `.claude/skills/<name>` symlinks).
3. Use `.research/` as persistent research memory.

Do not duplicate scientific logic here — all workflows live in Skills and references.

## Claude Code mapping

| Canonical | Claude Code |
|-----------|-------------|
| `AGENTS.md` | Read via this pointer |
| `.agents/skills/` | `.claude/skills/<name>` → `../../.agents/skills/<name>` |
| `.agents/subagents/` | `.claude/agents/<role>.md` (`name` + `description` 决定调谁) |
| `.research/` | Direct read |

Subagent 科学正文只在 `.agents/subagents/<role>.md`。不要在本文件或 `.claude/agents/` 里复制协议。

Main 自行决定派不派。模型分两档（详见 `AGENTS.md`）：干活用 inherit；Review / lead / 高风险解读用当前账号最强推理（`.claude/agents/` 里 `reviewer`、`research-lead`、`result-analyst` 默认 `opus`）。

See `adapters/claude-code.md` for harness-specific notes.
