# Wave G agent prompts (archived)

These files are **docs**, not framework, not `.research/`.

Gate E **E-B1** found that the original Wave G Test 1 / Test 2 prompts
leaked scoring rules into the agent-visible task. Those originals lived
only in `/tmp/rss-wave-g-prompts/` and were not reviewable in-repo.

| Path | Role |
|------|------|
| [original-leaked/](original-leaked/) | 2026-09-04 morning Wave G prompts (contaminated). **Do not re-feed.** |
| [eb1-clean/](eb1-clean/) | E-B1 de-leaked re-run prompts. Fixture + Skill/prompt paths + write-discipline + stdout shape. No expected Recommended Action, no “ADVANCE is a miss”, no “typically does not address”, no “criterion still fails”. Case 01 candidate is an author-voice pitch (Wave H VAEG style), not an “Honest difference vs WES” self-reveal. |
| [eb3-case10-ordinary-exploratory.txt](eb3-case10-ordinary-exploratory.txt) | E-B3 Case 10 live protection. `AGENTS.md` + `research-loop` / `experiment-design` + MOCK sanity Question/log + write-discipline. **No** skip-list for idea-evaluation / evidence-verification / reviewer / result-analyst. |

Scoring stays in case READMEs and in the Wave G reports. It must not
appear in `eb1-clean/` files.

E-B1 re-run (2026-09-04 06:03 +08) used `eb1-clean/` on `/tmp` clones at
HEAD `32917595753ecc33fa5f8794e197c9eba10eeb5e`. Codex and Claude Code
both hit T1 `REVISE` and T2 honest-baseline `does not address`. See
`../wave-g-codex.md` and `../wave-g-claude.md`.

E-B3 Case 10 live protection (2026-09-04 06:07 +08) used
`eb3-case10-ordinary-exploratory.txt` on `/tmp/rss-wave-g-case10-codex-r2`
at the same HEAD. Codex **PASS**. See `../wave-g-case10-protection.md`.
