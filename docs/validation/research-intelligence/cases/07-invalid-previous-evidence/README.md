# Case 07 — Invalid Previous Evidence

**Wave:** V0.2 Wave F (read-only regression fixture)  
**Kind:** failure (leaked EXP filed as Negative instead of Invalidated)  
**Gate D priority:** important  
**Load:** this folder only. Canonical files: `artifacts/metrics.json`, `artifacts/leakage-audit.md`.  
**Not science.** Do **not** copy into the framework repo’s `.research/`.

## Point

An older EXP was **trusted** and cited as Story Evidence. A later audit finds
**leakage** (session identity overlapping train/test; capture ID used as a
feature). The Experiment record must move to Outcome `invalid`. DISCOVERY must
use **Invalidated Findings**, **not** Negative Discoveries. Story must **drop**
the Evidence line that rested on that artifact.

## Owners (cite; do not redefine)

| Topic | Owner |
| --- | --- |
| Outcome token `invalid` | `experiment-record.md` §Outcome 值 |
| Completed-but-unusable vs technical failure | `result-diagnosis.md` hard mappings (technical → `failed` + `not-assessed`; unusable completed → `invalid`; previously trusted → Invalidated Findings) |
| Invalidated Findings, not Negative | `evidence-and-claim.md` §G; `result-analysis` Skill Default flow step 4; `DISCOVERY.template.md` / `state-files.md` §DISCOVERY.md |
| Drop Story Evidence line | `evidence-and-claim.md` §G; `story-maintenance` (Evidence is a small-section edit) |
| Integrity before interpretation | `evidence-and-claim.md` §D; `result-diagnosis.md` step 1 |
| Keep the EXP section | `experiment-record.md` writing principles; `result-analysis` Deviation — do not delete the section |
| Report labels ≠ Outcome | `evidence-and-claim.md` §F — if `evidence-verification` runs, `invalid evidence` is a **report label**, not a second Outcome table |

Do **not** copy the Outcome table or the Verdict list. Do **not** invent a
local Outcome synonym for “leaked.”

## Skills / prompts that should fire

- `result-analysis` + `result-diagnosis.md` (re-open EXP-004 after the audit)
- `story-maintenance` to **remove** the Evidence bullet that cited EXP-004
- Optional: `evidence-verification` as an Evidence Gate on EXP-004 **after**
  leakage is known (integrity fail → report label `invalid evidence` per
  `evidence-and-claim.md` §F — still must not auto-write Outcome)
- Optional: `failure-diagnosis.md` if the Agent needs to name
  **experimental invalidity** vs hypothesis contradiction — leakage is
  invalidity, not “the idea died”

Main still writes EXPERIMENTS Outcome and DISCOVERY.

## Must not auto-trigger / auto-conclude

- Must **not** write **Negative Discoveries** for this leakage
- Must **not** set Outcome `contradicts` (“PulseGate failed”) or `null`
- Must **not** treat this as Case 04 technical failure (`Status=failed`,
  Outcome `not-assessed`) — the job **completed**; the measurement is unusable
- Must **not** leave the old Positive Discovery and Story Evidence in place
- Must **not** auto-rewrite Problem / Key Observation / Core Idea as a silent
  substitute for dropping Evidence; if Core Idea would fall with no remaining
  support, **suggest** `experiment-review` rather than rubber-stamping a new
  Core Idea (`story-maintenance` large-change rule)
- Must not copy Outcome / Verdict tables

## Input (MOCK, embedded)

### STORY.md (excerpt, **before** the audit — still cites EXP-004)

```markdown
# Story: PulseGate timing regularity labels encrypted tunnels

> MOCK fixture. No performance numbers.

## Problem

Cheap payload-free cue for tunnel vs web sessions in a lab capture pool.

## Key Observation

Tunnel sessions show more regular inter-arrival pulses (MOCK).

## Core Idea

PulseGate (pulse histogram + shallow MLP) uses that regularity.

## Evidence

- EXP-004: PulseGate separates tunnel vs web on TraceSet-Omega lab splits
  (internal Evidence — **this line must be removed after the audit**).
- Literature analogy: Northport MOCK-CITE (not methods-checked here).

## Boundary

- Lab captures only.

## Open Gaps

1. Does the cue survive a site the model has not seen?
```

### DISCOVERY.md (excerpt, **before**)

```markdown
## Positive Discoveries

- PulseGate timing histogram is sufficient to separate tunnel vs web on
  TraceSet-Omega — Evidence: EXP-004

## Negative Discoveries

- (none)

## Invalidated Findings

- (none)
```

### EXPERIMENTS.md (excerpt, **before** — still looks like a win)

```markdown
## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-004 | PulseGate vs packet-count (row split) | completed | supports | Evidence: timing cue | 2026-08-02 |

## EXP-004 — PulseGate vs packet-count (row split)

Status: completed
Outcome: supports

Question: Does PulseGate beat packet-count on TraceSet-Omega sessions?

Data / Setup (as recorded — **wrong unit**): random 80/20 **session** split
inside a pooled mix of three captures. Features included `capture_id` and
truncated `session_token` (documented in the feature list as “routing hints”).

Git: commit b0a7d1e5 (MOCK)
Results: results/EXP-004/
```

### What changed later

A split audit (this fixture’s artifact) shows most “test” sessions share
`session_token` prefixes and `capture_id` with train. The number in
`metrics.json` is therefore **not interpretable** as generalization.

## Tiny artifacts

- [artifacts/metrics.json](artifacts/metrics.json) — the old pretty number
- [artifacts/leakage-audit.md](artifacts/leakage-audit.md) — overlap counts

## Expected behavior (sandbox only)

1. Integrity **fails** (`evidence-and-claim.md` §D). Stop claim support.
2. Outcome candidate = `invalid` per `experiment-record.md` §Outcome 值
   (`result-diagnosis.md`: completed but unusable).
3. EXPERIMENTS: keep `Status=completed`; set `Outcome=invalid` (section + Index).
   Keep the EXP section; do not delete history.
4. DISCOVERY:
   - Add **Invalidated Findings** (the previously trusted timing-cue support
     is unusable) with `Evidence: EXP-004`
   - **Remove or strike** the Positive Discovery that rested on EXP-004
   - Do **not** add Negative Discoveries for EXP-004
   - Research Evolution may note “trusted → leaked”
5. STORY via `story-maintenance`: **delete** the Evidence bullet that cited
   EXP-004. Put leftover ambition in Boundary / Open Gaps (need a
   capture-grouped, identity-free retest). Do not copy metrics into Story.
6. Next: redesign (`experiment-thinking.md` §B) — not a sweep on the leaked
   split.

## Pass / fail

**PASS** if Outcome=`invalid`, Invalidated Findings is used, Negative is not
used for this EXP, Story Evidence line is gone, EXP section remains.

**FAIL** if any of:

- Negative Discovery for EXP-004 / “PulseGate is false”
- Outcome `contradicts` or `null` because the Agent wants a scientific death
- Status flipped to `failed` with Outcome `not-assessed` (wrong failure class)
- Story still lists EXP-004 as Evidence
- Outcome or Verdict tables copied

## Anonymization

Fictional tokens and captures (`omega-a` / `omega-b` / `omega-c`). No host
home directories. No live `.research/` copy.
