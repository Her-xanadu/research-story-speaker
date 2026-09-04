# Framework Extension Reference

Canonical detailed guidance for `framework-extension`.

This file owns extension-design judgment. It is not project Protocol, not
Research Intelligence for scientific claims, and not a project-state source.

---

## 1. Extension decisions

```text
CONFIGURE_ONLY
COMPOSE_EXISTING
EXTEND_EXISTING
PACKAGE_OPTIONAL
CORE_CANDIDATE
REJECT
```

| Decision | Meaning |
|---|---|
| `CONFIGURE_ONLY` | Existing configuration, `RESOURCES`, `PROJECT`, or adapter binding is enough |
| `COMPOSE_EXISTING` | Existing Skills already cover the need; add a recipe/handoff |
| `EXTEND_EXISTING` | One current owner needs one bounded behavior |
| `PACKAGE_OPTIONAL` | Useful but domain/provider/user-specific; keep opt-in |
| `CORE_CANDIDATE` | Broadly reusable candidate, pending independent validation |
| `REJECT` | Duplicate, incompatible, unsafe, unbounded, or unjustified |

These labels are local to framework extension work. They are not Experiment
Outcome or Reviewer Verdict values.

---

## 2. Extension ladder

Choose the lowest rung that fully solves the need.

| Rung | Form | Typical use |
|---:|---|---|
| 0 | No change | Existing capability already works |
| 1 | Project configuration | Paths, resources, constraints, preferences |
| 2 | Composition recipe | Combine existing Skills/Subagents/MCPs |
| 3 | Prompt/reference/template | New judgment knowledge or task shape |
| 4 | Mode/route in existing Skill | Same owner, bounded behavior |
| 5 | Thin adapter or MCP binding | New host/provider capability |
| 6 | Optional extension pack | Domain/provider/user-specific capability |
| 7 | New core Skill/Subagent | Broad, distinct, repeatedly needed behavior |
| 8 | Protocol/state/core change | Last resort; migration review required |

Never move directly from “upstream has a Skill” to rung 7.

For rung 4 or higher, document:

- why lower rungs are insufficient;
- default-context impact;
- ordinary-path impact;
- maintenance owner;
- rollback/removal path.

---

## 3. User-specific customization

Many requests are configuration rather than framework development.

Try first:

```text
RESOURCES.md
PROJECT.md persistent constraints
existing Skill mode
adapter/provider binding
optional extension
```

Use `workspace-setup` for compute and code layout.

Never hard-code one user's:

```text
absolute path
server alias
MCP name
API provider
vault path
institution policy
```

into reusable core logic.

Separate capability from value:

```text
Local value:
Vault path = /Users/Alice/Papers

Reusable capability:
Local-paper-library integration with configurable vault location
```

A local customization becomes a reusable candidate only when the general
capability can be separated from local values.

---

## 4. External workflow localization

External projects often bring:

```text
state files
progress logs
orchestrators
databases
CLI runners
review loops
```

The useful part may be only one operator.

Localization procedure:

1. Identify the useful capability or judgment operator.
2. Remove the upstream state/control plane from the proposed integration.
3. Map inputs to existing RSS files.
4. Map transient outputs to `.research/work/`.
5. Map durable science to Main-Agent integration into existing state files.
6. Map provider invocation to an adapter or optional integration package.
7. Keep `research-loop` as the only scientific orchestrator.
8. Record source, specific upstream file, and license.
9. Rewrite in local terminology and original wording.
10. Verify removal does not corrupt `.research/`.

Valid:

```text
existing RSS Skill
→ optional bounded step
→ work artifact
→ Main integration
```

Invalid:

```text
new autonomous loop
→ new state/progress files
→ external agent edits canonical state
```

---

## 5. New Skill assessment

A new Skill is justified only when:

1. It is distinct from existing Skills.
2. It has one clear trigger and owner.
3. Reads/writes are bounded.
4. It is not merely a reference, prompt, or mode.
5. It is useful beyond one one-off task.
6. It has a must-not-trigger protection case.
7. It introduces no second state model.
8. Its value exceeds permanent discovery and maintenance cost.

Required sections:

```text
When to use
When not to use
Goal
Reads
Default flow
Outputs
Allowed writes
Forbidden writes
Dependencies
Fallback
Deviation allowed
Boundaries
```

Test:

```text
positive trigger
near-miss negative trigger
ordinary-path protection trigger
```

Provider/domain-specific Skills should normally ship as optional extensions
first.

---

## 6. MCP and external-tool integration

MCP is a capability binding, not a new research memory.

Core workflows should request capabilities such as:

```text
literature retrieval
local paper consultation
remote compute
repository access
independent review
reference management
```

Provider-specific names belong in adapters, integration packages, or
`RESOURCES.md`, not in scientific logic.

Capability contract:

```text
Capability name
Purpose
Provider/server
Tools/resources/prompts exposed
Read permissions
Write permissions
Data sent externally
Authentication location
Rate limits
Idempotence
Provenance
Failure signals
Degraded behavior
Fallback
```

Never store credentials in Skills, adapters, `.research/`, examples, or
fixtures.

A shared mutable service needs:

```text
designated writer when concurrency is unsafe
duplicate detection
idempotent retry
explicit failure receipt
```

Test:

```text
MCP unavailable
authentication missing
rate limit
partial result
write succeeds but downstream indexing fails
```

The workspace remains readable if the provider disappears.

---

## 7. Harness adapters

Adapters answer only:

```text
How does the host find AGENTS?
How does it discover Skills?
How are Subagents created?
How are MCP/tools exposed?
How are permissions handled?
What host limitation exists?
```

Do not copy Story, Experiment, Review, domain, or research-loop logic into an
adapter.

Prefer a short compatibility note over a mirrored Skill tree.

---

## 8. Domain packs

A domain pack specializes scientific judgment without forking the framework.

It may define:

```text
domain terminology and scope
scientific and experimental units
common leakage/confounding risks
metrics and failure modes
standard baselines and controls
artifact expectations
reproducibility conventions
evidence-depth expectations
literature sources and venues
ethical/safety constraints
examples and fixtures
```

It must not define:

```text
a second STORY format
a second experiment ledger
hard-coded scientific conclusions
one user's paths
a new top-level research loop
automatic paper claims
```

Recommended shape:

```text
extensions/domains/<domain-slug>/
├── EXTENSION.md
├── references/
├── prompts/
├── templates/
├── examples/
└── tests/
```

Create only directories with content.

Prefer:

```text
existing Skill
→ conditionally load domain reference
```

over duplicating experiment, result, and review Skills per domain.

---

## 9. Subagents

Create a Subagent only when work repeatedly benefits from:

```text
parallelism
independent context
independent reviewer identity
large bounded reading
specialized permission
```

Do not create one only for a persona.

Define:

```text
trigger
input packet
tools
read scope
write scope
output
handoff owner
stop condition
```

Subagents return bounded artifacts; Main integrates canonical state.

Test one delegation-positive case and one direct-execution protection case.

---

## 10. Optional extension packaging

Recommended catalog:

```text
extensions/
├── domains/
├── integrations/
├── workflows/
└── skills/
```

An extension may contain:

```text
EXTENSION.md
references/
prompts/
skills/
adapters/
templates/
examples/
tests/
```

No empty directories. Presence in the repository does not activate it.

`EXTENSION.md` states:

- capability and intended users;
- compatible framework versions;
- files added/patched;
- dependencies and permissions;
- activation and verification;
- degraded behavior;
- removal and rollback;
- maturity class;
- maintainer.

A wrapper in `.agents/skills/`, when necessary, stays thin and links to the
package. Do not copy an extension into multiple Harness roots.

---

## 11. Complexity budget

Every proposal records:

```text
permanent files added
core files modified
new terminology
new dependencies
default context increase
ordinary-path increase
network/tool calls
new permissions
new failure modes
maintenance owner
what complexity is avoided or removed
```

A feature that fixes one edge case by increasing every ordinary task's cost
fails its protection case.

The goal is not zero added files. The goal is a favorable value-to-permanent
complexity ratio.

---

## 12. Validation

### Static architecture

- no ninth canonical research file;
- no second research orchestrator;
- no duplicate Protocol vocabulary;
- no framework runtime code;
- no secrets or reusable personal paths;
- one owner;
- thin adapters;
- optional content dormant by default;
- relative links resolve;
- install/removal documented.

### Behavioral

Required:

```text
expected-use case
must-not-trigger protection case
```

When relevant:

```text
dependency-missing
partial failure
duplicate/retry
held-out
cross-Harness
```

### Cost

Record:

```text
files loaded
context/token change
network/tool calls
user questions
permissions
maintenance surface
```

### State and removal

- Main remains canonical integrator.
- Durable output does not exist only in chat.
- Removal leaves `.research/` readable.
- Existing projects continue without migration when possible.
- User-local extensions are not overwritten by framework upgrades.

### Security

- least privilege;
- no credentials in files;
- sensitive data flow documented;
- writes bounded;
- retry behavior explicit.

### Regression

Modified Skills/prompts use `skill-evolution.md`:

```text
failure case
protection case
held-out case
baseline vs candidate
independent scorer preferred
no automatic deployment
```

---

## 13. Promotion and deprecation

Maturity path:

```text
local experiment
→ optional extension
→ validated extension
→ core candidate
→ core
```

Promote toward core only when:

- multiple independent users/projects need it;
- it is broad across domains/providers;
- it cannot remain cleanly optional;
- owner and trigger are stable;
- it does not duplicate existing behavior;
- positive, protection, and held-out evidence exist;
- relevant Harness behavior is acceptable;
- maintenance ownership is explicit;
- context, permission, and failure costs are acceptable;
- independent framework review accepts it.

Keep optional when provider/domain-specific, uncommon, high-cost, or externally
volatile.

Reject/remove when it duplicates core, lacks reproducible value, introduces
state drift, imports an incompatible control plane, triggers broadly, or costs
more to maintain than it returns.

Deprecation records:

```text
date
reason
replacement
migration
last compatible version
```

Do not silently delete history.

---

## 14. Open-source contribution rules

A reusable contribution should include:

```text
Extension Brief
small reviewable diff
source/license record
installation and removal
expected-use case
protection case
limitations
maintainer
```

Prefer one capability per contribution.

When learning from an upstream Skill or project:

1. inspect the actual source and license;
2. identify the smallest useful operator;
3. distinguish concept learning from copied text;
4. rewrite in local terminology and structure;
5. reject incompatible state/orchestration;
6. cite the upstream source in design records;
7. preserve license obligations;
8. do not install an entire collection for one useful sub-Skill.

Core maintainers may keep a contribution optional even when it is high quality.

---

## 15. Stop conditions

Reject or move to a lower rung when:

- existing behavior solves the need;
- trigger or owner is unclear;
- the proposal requires a second state/control plane;
- all users pay for a niche feature;
- dependency absence has no safe behavior;
- secrets enter project files;
- removal damages project state;
- scientific conclusions become framework rules;
- one anecdotal request is the only core evidence;
- the request is configuration rather than capability;
- no protection case can be defined.

Require architecture review for rung 8.

---

## 16. Anti-patterns

- Installing an entire upstream collection for one operator.
- Promoting one user's preference directly into core.
- Forking all scientific Skills for a domain.
- Hard-coding providers in scientific logic.
- Adding a state file per integration.
- Treating MCP/chat output as durable truth.
- Creating a Subagent only as a persona.
- Putting science logic in an adapter.
- Adding another top-level loop.
- Loading optional content on cold start.
- Shipping without fallback or removal.
- Broad Skill triggers such as “research” or “analyze.”
- Fixing a failure by firing every gate.
- Candidate author scoring and auto-merging its patch.
- Unrelated “while we are here” refactors.
- Deleting rejected-extension evidence.
- Writing framework-extension records into project DISCOVERY.

---

## 17. Classification examples

### Local paper-vault MCP

Usually:

```text
PACKAGE_OPTIONAL or EXTEND_EXISTING
```

Keep the vault path in `RESOURCES.md`, provider calls in the integration or
adapter, and scientific synthesis in the current literature workflow. No second
literature state or orchestrator.

### Network-security specialization

Usually:

```text
PACKAGE_OPTIONAL
```

Package units, leakage risks, metrics, controls, artifacts, and review lenses.
Existing Skills load domain references conditionally.

### Different Reviewer provider

Usually:

```text
CONFIGURE_ONLY
```

Bind the existing review capability. Do not create one Reviewer Skill per
provider.

### Large upstream research framework

Usually:

```text
COMPOSE_EXISTING or EXTEND_EXISTING
```

Extract useful operators, reject incompatible state/orchestration, record
source/license, and rewrite in local terminology.

---

## 18. Final rule

```text
core permanence must be earned
optional capability is a valid final state
removal is part of design
extension evidence must outlive the chat
```

Grow an extension ecosystem, not an unlimited core.
