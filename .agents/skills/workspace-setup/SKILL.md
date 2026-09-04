---
name: workspace-setup
description: >-
  Personalize compute and experiment-code Git layout for this workspace.
  Use on first install, when PROJECT Status is UNINITIALIZED before
  materialize, when the user creates a new project from the template, or
  when they change server access or code repository location (resource
  authorization change). Ask where experiments run (local vs server — server
  first if available) and where the code Git repo lives (layout A/B/C).
  Writes only RESOURCES.md fields (and optionally one PROJECT constraint
  line). Not a research-loop step. Do not use for experiments, Story, or
  routine resume.
---

# Workspace Setup

Thin Skill for **per-machine / per-project resource personalization** — not
scientific judgment. Answers live only in `.research/RESOURCES.md` (and
optionally one line in `PROJECT.md` **Persistent Constraints**). Git binding
at run time stays in [git-linking.md](../../references/git-linking.md);
execution still resolves code via `RESOURCES` → repo → Entry in
`experiment-execution` — **do not** re-run this Skill on every EXP.

**Not a research-loop step.** Do not schedule it from `research-loop`. Pair
with `workspace-resume` for first materialize; use alone when an `ACTIVE`
project changes compute or code layout.

Field shapes: [RESOURCES.template.md](../../templates/RESOURCES.template.md).
Layout modes A/B/C: `README.md` §Workspace 定位.

## When to use

- Clone / copy template; `PROJECT.md` **Project Status** is `UNINITIALIZED`.
- User says install, set up workspace, 配置服务器, 代码放哪, new project.
- **Resource authorization change** on an `ACTIVE` project: new server, SSH
  alias change, move code repo, switch layout A ↔ B ↔ C, disk full on laptop.

Do **not** use for: research routing (`research-loop`), experiment design or
execution, Story edits, literature, Idea-gate, Evidence-gate, Review, or
`framework-maintenance` audits.

## Goal

Before `workspace-resume` materializes (or when reconfiguring), collect **two**
decisions and write them durably:

```text
1. Where experiments run (local vs server — configure server first if any)
2. Where the experiment code Git repo lives (layout + paths + remote)
```

Stop asking once both are writable. Batch questions, then write disk.

## Decision Frontier (fixed order)

Ask **only** what is missing. **Order is fixed:**

### 1. Compute — local or server?

If the user has a reachable server (SSH, VPN, institutional GPU host), **configure
Compute first** before code layout.

Collect:

- **Use** — e.g. GPU experiments, CPU-only, mixed.
- **Access** — `local shell` **or** `SSH <alias>` (e.g. `SSH njust`). **Never**
  ask for passwords, API keys, or private keys.
- **Working directory** — default cwd on that host for experiment commands.

If **no server**: one Compute entry with `Access: local shell`; set
**External Capabilities → Remote compute** to `N/A`.

When asking, include: current frontier, recommended default, reason.

### 2. Code Git — where does the repo live?

Disk space differs per user — **do not** assume `../project-code`.

Pick layout (cite `README.md`; do not invent new mode names):

| Mode | Meaning |
|------|---------|
| **A** | Code inside workspace (e.g. `code/`, `repos/<name>/`) |
| **B** | Code repo **beside** workspace (e.g. `../my-experiment-code`) |
| **C** | Code primarily on server; laptop may have no full clone |

Collect per primary codebase:

- **Codebase ID** (short slug, e.g. `detector-main`)
- **Purpose** (one line)
- **Git** — remote URL, or `local-only` / `no remote yet`
- **Recovery source** / **Portability** per template enums
- **Preferred relative location** (A/B) **or** **Last known local location**
- **Remote location** (`host:path` for C, else `N/A`)
- **Notes** — which layout (A/B/C)

**Do not** `git clone` into a path the user did not choose. **Do not** store
secrets in `RESOURCES.md`.

Optional: if compute is server-only, add **one** line under
`PROJECT.md` → **Persistent Constraints** (natural language, not a new field).

## Write mapping (owner table)

| User decision | Write here only | Fields |
|---------------|-----------------|--------|
| Local vs server | `RESOURCES.md` → `## Compute` | `Use`, `Access`, `Working directory` |
| Remote available | `RESOURCES.md` → `## External Capabilities` | `Remote compute` row |
| Code identity + layout | `RESOURCES.md` → `## Codebases` | `Git`, `Recovery source`, `Portability`, `Preferred relative location`, `Last known local location`, `Remote location`, `Notes` |
| Long-run “must run on X” | optional `PROJECT.md` | **Persistent Constraints** (one line) |

**Must not write:** `STORY.md`, `DISCOVERY.md`, `EXPERIMENTS.md` (no EXP sections),
`LITERATURE.md`, `REVIEWS.md`, `STATE.md` (except `workspace-resume` init),
framework files under `.agents/`, or absolute paths into adapters.

**Must not** copy Outcome / Verdict tables or add Protocol enums.

## Default flow

### A. UNINITIALIZED (before materialize)

1. Read `.research/PROJECT.md` — if not `UNINITIALIZED`, use flow B.
2. Run questionnaire §1 then §2. Stop after batch if answers incomplete.
3. **Materialize or patch** `.research/RESOURCES.md` from
   `.agents/templates/RESOURCES.template.md` shape (in place on disk). Fill
   `## Compute`, at least one `## Codebases` entry, `## External Capabilities`.
   Datasets may stay placeholder until user names data.
4. Emit a short confirmation (Compute name, Codebase ID, layout mode). **Do not**
   set `Project Status` to `ACTIVE` — `workspace-resume` owns that.
5. Hand off to `workspace-resume` §0 for research goal + full eight-file
   materialize. After materialize, `workspace-resume` sets Workflow Position
   `W1 FRAME` (or `W0 SETUP` if goal still missing). See `story-loop.md`.

**Gate:** If Compute or primary Codebase layout is still unknown, **do not**
tell `workspace-resume` to write `ACTIVE`.

### B. ACTIVE reconfiguration

1. Read current `.research/RESOURCES.md` (and `PROJECT.md` if constraints may change).
2. Ask only what changed (server move, new clone path, new remote).
3. Patch the affected `## Compute` / `## Codebases` / External Capabilities fields.
4. Do **not** reset Story, wipe EXPERIMENTS, or re-run Idea-gate.
5. Suggest `research-memory` if indexes or EXP Git lines now look stale.

### Ready check (for `workspace-resume`)

`RESOURCES.md` is **setup-complete** when:

- `## Compute` has at least one named entry with **Access** and **Working directory**.
- `## Codebases` has at least one entry with **Git**, layout fields (**Preferred
  relative location** and/or **Last known local location** / **Remote location**),
  and **Notes** mentioning A, B, or C.
- **External Capabilities → Remote compute** is set (`N/A` is valid).

## Reads

| Priority | Files |
|----------|-------|
| Required | `.research/PROJECT.md` (status), `.research/RESOURCES.md` |
| Template | `.agents/templates/RESOURCES.template.md` |
| Reference | [git-linking.md](../../references/git-linking.md), [state-files.md](../../references/state-files.md) §RESOURCES |
| Optional | `README.md` §Workspace 定位 |

## Updates

| File | What |
|------|------|
| `.research/RESOURCES.md` | Primary target — Compute, Codebases, External Capabilities |
| `.research/PROJECT.md` | Optional one **Persistent Constraints** line |

Must **not** update `STORY.md`, `DISCOVERY.md`, `EXPERIMENTS.md`, `LITERATURE.md`,
`REVIEWS.md`, or `STATE.md` except when explicitly co-running `workspace-resume`
initialization in the same turn after setup is complete.

## Deviation allowed

- Skip Datasets section detail until the user names datasets.
- Multiple Compute entries if user has local debug + remote training (still ask
  server first when both exist).
- Multiple Codebases if user already has repos; still require a **primary** one
  for the current Story.
- Re-run on `ACTIVE` without touching PROJECT goal or STORY.

Do **not** invent remotes, clone without consent, store credentials, rewrite
Story, or add a ninth canonical state file.
