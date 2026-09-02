# Claude Code Adapter

This project uses **AGENTS.md** as the canonical project guide.

**On session start:**

1. Read `AGENTS.md` first.
2. Use `.agents/skills/` as the canonical skill library.
3. Use `.research/` as persistent research memory.

Do not duplicate scientific logic here — all workflows live in Skills and references.

## Claude Code mapping

| Canonical | Claude Code |
|-----------|-------------|
| `AGENTS.md` | Read via this pointer |
| `.agents/skills/` | Symlink or copy to `.claude/skills/` if native discovery required |
| `.agents/subagents/` | Map to Claude subagent definitions if needed |
| `.research/` | Direct read |

If Claude Code only auto-discovers `.claude/skills/`, create thin symlinks:

```text
.claude/skills/<name> → ../../.agents/skills/<name>
```

Subagent prompts: reference files in `.agents/subagents/` — do not fork content.

See `adapters/claude-code.md` for harness-specific notes after cross-platform testing.
