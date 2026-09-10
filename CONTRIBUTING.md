# Contributing

This repository is an **instruction-only research workspace template**.
PRs should change the framework (`AGENTS.md`, `.agents/`, `adapters/`,
`docs/validation/`), not a live science project.

## What GitHub carries

| Path | On GitHub |
|------|-----------|
| Eight `.research/*.md` files | **UNINITIALIZED** templates only |
| `.research/work/.gitkeep` | empty dir marker |
| `.research/reviews/.gitkeep` | empty dir marker |
| `examples/mock-flow-detection/` | fictional MOCK |

GitHub must **not** receive:

- an `ACTIVE` `.research/` overlay (Story, experiments, literature, reviews)
- `.research/work/` or `.research/reviews/<EXP-ID>/` artifacts
- nested experiment code (`repos/`, `code/`)
- host Codex steward files (`.codex/config.toml`, `.codex/hooks.json`)
- server paths, dataset dumps, credentials, or personal project names

If `PROJECT.md` says `ACTIVE` in your clone, you are overlaying a private
research workspace on the template. Keep that overlay **uncommitted**.

## Dual-use checkout

Some maintainers run science in the same clone as the framework. That is
allowed locally. It is not a reason to push science.

Suggested local guard after `git pull` (not part of the template runtime):

```text
git update-index --skip-worktree \
  .research/PROJECT.md .research/STORY.md .research/STATE.md \
  .research/DISCOVERY.md .research/EXPERIMENTS.md .research/LITERATURE.md \
  .research/REVIEWS.md .research/RESOURCES.md
```

Undo with `git update-index --no-skip-worktree -- <file>` if you are
intentionally editing the **template** text.

## Pull requests

- One concern per PR. Do not mix Skill wording with README polish.
- Do not add a 16th Skill without the `framework-extension` ladder.
- Do not add runtime `.py` / `.sh` orchestration. Frozen count: scripts **0**.
- Validation records go under `docs/validation/`, never into project `.research/`.
- Do not rewrite historical Gate reviews unless the change is a privacy redact.

## License

Contributions are accepted under the MIT License in `LICENSE`.
