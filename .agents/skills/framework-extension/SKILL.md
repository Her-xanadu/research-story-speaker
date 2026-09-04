---
name: framework-extension
description: >-
  Design and integrate user-specific workflows, optional capabilities, external
  Skills, MCP services, harness adapters, and domain packs into
  research-story-speaker while preserving its Story-driven, file-native,
  prompt-native architecture. Use only when a user explicitly asks to extend,
  customize, integrate, package, or contribute a capability. Prefer the least
  invasive extension form, keep optional capabilities outside the core until
  validated, and hand candidates to framework-maintenance for audit and
  regression. Not a research-loop step and not for routine science, workspace
  setup, or one-off prompt editing.
---

# Framework Extension

A maintainer/contributor Skill for deciding **how to extend
research-story-speaker without making its core accumulate unrelated workflows,
providers, domain rules, or state systems**.

Detailed extension judgment lives in
[framework-extension.md](../../references/framework-extension.md). The Extension
Brief shape lives in
[EXTENSION-BRIEF.template.md](../../templates/EXTENSION-BRIEF.template.md).

Related owners:

- [AGENTS.md](../../../AGENTS.md) — workspace identity, routing, autonomy, and
  Main/Subagent write boundaries.
- [state-files.md](../../references/state-files.md) — eight canonical research
  files and single-source rules.
- [story-loop.md](../../references/story-loop.md) — the only project-level
  scientific loop.
- [framework-maintenance](../framework-maintenance/SKILL.md) — architecture
  audit, regression, Skill Evolution, and release recommendation.
- [skill-evolution.md](../../references/research-intelligence/skill-evolution.md)
  — evidence threshold for modifying existing Skills/prompts.
- [subagent-handoff.md](../../prompts/subagent-handoff.md) — bounded delegated
  work and Main-Agent integration.

## When to use

Use when a user or contributor asks to:

- connect a personal workflow;
- add an optional feature or capability;
- localize a useful idea from another project;
- integrate a Skill, Subagent, MCP, external service, or Harness;
- specialize the framework for a scientific domain;
- package a local customization for reuse;
- decide whether a contribution belongs locally, optionally, or in core.

Do not use for:

- routine research, experiments, result analysis, or Review;
- Story or Experiment scientific updates;
- compute/code-path setup (`workspace-setup`);
- repeated-friction repair (`framework-maintenance` + `skill-evolution`);
- release audit (`framework-maintenance`);
- adding something merely because an upstream project looks comprehensive.

## Goal

Apply this rule:

```text
preserve the Story-driven core
→ reuse the existing owner
→ choose the least invasive extension form
→ keep narrow capabilities optional
→ validate value and protection
→ promote only what repeatedly earns permanence
```

The framework should gain useful capabilities while the default core remains
clear, portable, prompt-native, and inexpensive to understand.

## Frozen invariants

Confirm before implementation:

1. Story-driven science remains central.
2. The existing eight `.research/` files remain canonical.
3. Main Agent remains the canonical writer.
4. `research-loop` remains the single project-level scientific orchestrator.
5. Framework-side integration remains prompt/file/config based.
6. Canonical logic stays canonical; Harness adapters remain thin.
7. Optional content is not loaded by default.
8. Chat/MCP output is not durable truth until saved or referenced.
9. External tools provide capabilities, not a second memory/control plane.
10. Rejected and deprecated extension evidence is preserved.

A proposal that violates one of these is normally redesigned as an optional
extension or sent to explicit architecture review.

## Default flow

### 1. Restate the need

Describe the user need as a capability, not as the user's proposed
implementation.

### 2. Read current owners

Read the minimum set:

```text
AGENTS.md
README.md
state-files.md
story-loop.md
framework-maintenance/SKILL.md
skill-evolution.md
the closest existing Skill/prompt/reference/adapter
```

For project-local fit, read only relevant sections of:

```text
PROJECT.md
STORY.md
STATE.md
RESOURCES.md
```

### 3. Check overlap

Determine whether the request is already covered by:

```text
configuration
existing Skill composition
prompt/reference/template
existing-Skill mode
adapter/capability binding
```

Do not propose a new Skill before this check.

### 4. Classify and choose scope

Choose one primary type:

```text
project customization
workflow recipe
prompt/reference enrichment
existing-Skill mode
optional Skill
MCP/tool integration
Harness adapter
domain pack
Subagent
Protocol/core change
```

Choose the narrowest scope:

```text
one project
one user
one domain
one provider
one Harness
optional public extension
framework core
```

### 5. Apply the extension ladder

Use the lowest rung that solves the need. The ladder and decision vocabulary are
defined in `framework-extension.md`.

For a proposal above the prompt/reference level, explain:

```text
Why lower rungs are insufficient
```

### 6. Write an Extension Brief

Default:

```text
docs/validation/extensions/<slug>/EXTENSION-BRIEF.md
```

Use `EXTENSION-BRIEF.template.md`.

### 7. Define the capability contract

Before editing, define:

```text
trigger
owner
inputs
outputs
read/write scope
dependencies
permissions
fallback
failure semantics
provenance
observability
rollback/removal
```

### 8. Implement the smallest candidate

Prefer:

```text
one owner + links
```

over duplicated logic.

Framework-side implementation stays instruction-only. Use a reviewable branch
or commit series. Do not silently merge a new core component.

### 9. Validate

At minimum:

```text
expected-use case
must-not-trigger protection case
```

Add dependency-missing, partial-failure, retry/idempotence, held-out,
cross-Harness, security, and rollback cases when applicable.

Modified Skills/prompts follow `skill-evolution.md`.

### 10. Hand off

Run `framework-maintenance`:

- `standard` — architecture and duplication audit;
- `regression-eval` — baseline vs candidate;
- `skill-evolution` — modification of an existing Skill/prompt due a repeated
  failure class.

This Skill designs and integrates. `framework-maintenance` evaluates release
fitness.

### 11. Assign release class

```text
local-only
optional extension
validated extension
core candidate
reject
```

Do not present an optional or local capability as core.

## Required output

```text
## Extension Decision
CONFIGURE_ONLY | COMPOSE_EXISTING | EXTEND_EXISTING |
PACKAGE_OPTIONAL | CORE_CANDIDATE | REJECT

## User Need
...

## Existing Owner
...

## Minimal Integration
...

## Capability Contract
...

## File Map
...

## Core Invariants
...

## Validation Plan
...

## Rollback / Removal
...

## Open-Source Packaging
...

## Handoff
framework-maintenance mode: <standard | regression-eval | skill-evolution>
```

When implementation is authorized, create or update the Extension Brief.

## Reads

| Priority | Files |
|---|---|
| Required | `AGENTS.md`, `README.md`, this Skill |
| Architecture | `state-files.md`, `story-loop.md`, `framework-extension.md` |
| Maintenance | `framework-maintenance/SKILL.md`, `skill-evolution.md` |
| Relevant owner | Closest existing Skill/prompt/reference/adapter |
| Project fit | Relevant `PROJECT`, `STORY`, `STATE`, `RESOURCES` sections |
| External idea | Upstream Skill/docs/license when localizing |

Read only what the decision requires. Do not preload the whole framework or all
extensions.

## Updates

Allowed by default:

- `docs/validation/extensions/<slug>/`;
- `extensions/<type>/<slug>/`;
- a narrowly scoped candidate branch/patch;
- source/license registry;
- targeted documentation and cross-links;
- the selected owner after explicit implementation authorization.

Requires explicit approval:

- new core Skill or Subagent;
- `AGENTS.md` routing change;
- Protocol-owner change;
- change affecting every workspace;
- new external write permission;
- promotion from optional extension into core.

Not allowed:

- direct scientific edits to the eight `.research/` files;
- a ninth canonical project-state file;
- a second research orchestrator;
- framework runtime scripts;
- credentials;
- automatic merge/deployment;
- importing an entire upstream control plane when one operator is sufficient;
- silently overwriting user-local extensions.

Project-local configuration changes are handed to their existing owner.

## Boundaries

- This Skill is not part of `research-loop`.
- It does not judge scientific hypotheses.
- It does not replace `framework-maintenance`.
- It does not authorize a new core component merely because a user requested it.
- It does not turn extension metadata into project research state.
- It keeps optional provider/domain capabilities dormant until invoked.
- It preserves original wording, license records, and source attribution when
  localizing external ideas.

## Stop conditions

Return `REJECT` or a lower rung when:

- existing behavior already solves the need;
- trigger or owner is unclear;
- the design requires a second state/control plane;
- every user would pay for a niche feature;
- dependency absence has no safe behavior;
- secrets would enter project files;
- removal would damage project state;
- scientific conclusions would become framework rules;
- one anecdotal request is the only core-promotion evidence;
- the request is configuration rather than capability;
- no meaningful protection case can be defined.

Stop for architecture review before a Protocol/state/core change.
