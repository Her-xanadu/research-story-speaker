# V0.2.2 workspace-setup — static acceptance

Branch: `v0.2.2-workspace-setup` (from `v0.2.1`).

## Frozen counts (expected)

| Item | Count |
|------|------:|
| Canonical `.research/` files | 8 |
| `research-loop` | 1 |
| Subagents | 5 |
| Skills | **13** (`workspace-setup` added) |
| RI references | 6 |
| Framework runtime scripts | 0 |

Root `.research/` remains **UNINITIALIZED** on the framework template.

## Cold-start behavior (manual / harness)

On `UNINITIALIZED`:

1. Agent runs `workspace-setup` **before** `workspace-resume` materialize.
2. Question order: **Compute** (local vs server; server first if any) → **Code Git** (layout A/B/C + paths + remote).
3. Answers land only in `.research/RESOURCES.md` (`## Compute`, `## Codebases`, `## External Capabilities`).
4. Then `workspace-resume` collects research goal and materializes eight files without fabricating STORY science.

On `ACTIVE` resource change: `workspace-setup` patches RESOURCES only; Story unchanged.

## Not in scope

- No new canonical file, Outcome field, or research-loop step.
- No auto `git clone` without user-chosen path.
- No credentials in RESOURCES.
