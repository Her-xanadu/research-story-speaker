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

Main 自行决定派不派。模型**每次派发时沿光谱选**（便宜偏弱 → 最强最高 effort；详见 `AGENTS.md` §模型分档）：默认偏省，只有薄下限三类（独立 Review / 换核心方法 / 进 Story Evidence）保底用最强。原生默认只有 `reviewer` 保留最强（`.claude/agents/reviewer.md` 为 `opus`+`xhigh`），其余角色 `inherit`，Main 在下限派发时升到 `opus`+`xhigh`。

See `adapters/claude-code.md` for harness-specific notes.
