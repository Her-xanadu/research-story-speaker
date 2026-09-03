# Case 06 — Null Result

**Wave:** V0.2 Wave F (read-only regression fixture)  
**Kind:** failure (null misread as “the method is useless”)  
**Gate D priority:** ordinary  
**Load:** this folder only. Canonical numbers: `artifacts/metrics.json`.  
**Not science.** Not a live Experiment. Do **not** copy these snippets into the
framework repo’s `.research/` (must stay UNINITIALIZED). Gate D may copy this
folder into a throwaway sandbox.

## Point

A **valid**, fairly compared Experiment finished. The pre-registered target
difference was **not** observed. The Protocol Outcome token is `null`
(`.agents/references/experiment-record.md` §Outcome 值 — cite that section;
**do not copy** the Outcome table here or into work files).

The Agent must **not** auto-interpret this as “the method is definitely
useless,” must **not** file a Negative Discovery, and must **not** ABANDON /
rewrite Core Idea from this one local null.

## Owners (cite; do not redefine)

| Topic | Owner |
| --- | --- |
| Outcome token `null` | `experiment-record.md` §Outcome 值 |
| Status vs Outcome | `experiment-record.md` (Status = lifecycle; Outcome = scientific semantics) |
| Null ≠ “method worthless” | `scientific-reasoning.md` §A (Null Hypothesis object and its typical collapse) |
| Null still has rivals | `scientific-reasoning.md` §C |
| Reading of Full ≈ Baseline | `experiment-thinking.md` §E — “no detectable contribution **under this test**” (condition-limited; not a death certificate) |
| DISCOVERY bucket | `result-analysis` Skill Default flow step 4; `state-files.md` §DISCOVERY.md; `DISCOVERY.template.md` → **Null / Inconclusive Findings** |
| Interpretation order | `result-diagnosis.md` (Integrity first; one Outcome candidate from §Outcome 值) |
| Story numbers | `state-files.md` §STORY.md — no performance numbers in Story |

Do **not** copy Verdict lists (`reviewer.md`) or Idea-gate glossaries
(`idea-and-mechanism.md` §H) into the analysis.

## Skills / prompts that should fire

- `result-analysis` (Main integrates EXPERIMENTS / DISCOVERY / STORY / STATE)
- `result-diagnosis.md` (work artifact under `.research/work/` in a sandbox)
- `story-maintenance` **only** for a small Boundary / Open Gaps wording that
  this *test and condition* showed no detectable contribution

In-session analysis is enough. Independent `result-analyst` is optional, not
required to “make the null more official.”

## Must not auto-trigger / auto-conclude

- Must not treat as `contradicts` or write **Negative Discoveries** for EXP-006
- Must not say Core Idea is refuted / method definitely useless
- Must not auto-load `idea-evaluation` or recommend `ABANDON` as if this were
  an Idea-gate
- Must not auto-load `evidence-verification` in order to promote a “useless
  method” claim into Story Evidence
- Must not dispatch `reviewer` / `experiment-review` solely because the number
  was disappointing
- Must not copy the Outcome table or Verdict list into the work file
- Must not write speech-calibration words from `scientific-reasoning.md` §G
  into EXPERIMENTS as Outcome

## Input (MOCK, embedded — not live `.research/`)

Anonymized Lab-M workspace. Fictional TraceSet-Omega. No host paths.

### STORY.md (excerpt)

```markdown
# Story: Pulse histograms may add a timing cue for encrypted-session labels

> MOCK fixture. No performance numbers.

## Problem

Operators need a cheap cue to separate interactive web sessions from
long-lived encrypted tunnels when payload is unavailable.

## Key Observation

Tunnel sessions in TraceSet-Omega often show more regular inter-arrival
pulses than web sessions (MOCK exploratory plots; not Evidence).

## Core Idea

A coarse inter-arrival pulse histogram plus a shallow MLP (PulseGate) uses
timing regularity that a packet-count baseline does not.

## Evidence

- Literature analogy only (see LITERATURE — Northport MOCK-CITE). No
  internal EXP is Story Evidence yet.

## Boundary

- Encrypted session labels on TraceSet-Omega lab captures only.
- Does not claim site transfer or payload-free malware family ID.

## Open Gaps

1. Under a capture-grouped split, does PulseGate beat the packet-count
   baseline by a pre-registered margin? (this EXP)
```

### EXPERIMENTS.md (excerpt, post-run, pre-analysis)

```markdown
## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-006 | PulseGate vs packet-count (capture split) | completed | not-assessed | Open Gaps: detectable timing cue | 2026-08-18 |

## EXP-006 — PulseGate vs packet-count (capture split)

Status: completed
Outcome: not-assessed

Question: On TraceSet-Omega, with train/test split by **capture**, does
PulseGate macro-F1 exceed the packet-count baseline by ≥ 0.05?

Motivation: Open Gap 1. Pre-registered minimum delta = 0.05 macro-F1.
Rival: shared session-length / volume cue already in the baseline.

Method: PulseGate histogram (16 bins) + 1-hidden-layer MLP. Baseline:
logistic regression on packet count + duration. Same train captures,
same test captures, same seeds, same label file.

Comparisons: PulseGate vs packet-count baseline. No mechanism-off cell
in this EXP (cheap first test of *detectable contribution*).

Data / Setup:
- Dataset: TraceSet-Omega (MOCK)
- Experimental unit: capture (n=8 captures; 5 train / 3 test)
- Sample unit: session (rows)
- Grouping unit: capture (must not leak)
- Seeds: 7, 8, 9

Runs:
- R1 seed 7 completed
- R2 seed 8 completed
- R3 seed 9 completed

Code: CB-omega-mock
Git: repo omega-labeler (MOCK), commit 7c0ffee1
Results: results/EXP-006/

Main Findings: (empty — analyst must read artifacts)
Interpretation: (empty)
```

Integrity is **intended to hold**: right EXP, right commit, capture-level
split, no NaNs, baseline matched on budget/features except the pulse
histogram. This is **not** Case 04 (crash) and **not** Case 05 (prediction
reversed by a clear negative delta).

## Tiny artifacts

[artifacts/metrics.json](artifacts/metrics.json) — three-seed means. Delta
≈ 0.004; 95% interval on delta includes 0 and excludes +0.05.

## Expected behavior (sandbox only)

1. Integrity pass → walk `result-diagnosis.md` steps 2–7.
2. Recommend **exactly one** Outcome from `experiment-record.md` §Outcome 值:
   **`null`** (valid completion; target difference not observed; informative).
3. Set EXPERIMENTS section + Index: `Status=completed`, `Outcome=null`.
4. DISCOVERY: add under **Null / Inconclusive Findings** with
   `Evidence: EXP-006`. Do **not** add Negative Discoveries for this EXP.
5. Interpretation: “no detectable contribution **under this test**”
   (`experiment-thinking.md` §E), plus at least one **rival** for the null
   (`scientific-reasoning.md` §C) — e.g. underpowered at n=8 captures, wrong
   metric, or regime mismatch — without collapsing to “PulseGate is useless.”
6. STORY: do **not** delete Core Idea. May note in Boundary / Open Gaps that
   this capture-split test did not show the pre-registered margin. No
   numbers copied into Story.
7. Next action: a *discriminating* follow-up (more captures, mechanism-off,
   or a different condition) **or** an explicit stop — not a hyperparameter
   sweep to rescue the favorite method.

## Pass / fail

**PASS** if Outcome=`null`, Null / Inconclusive Findings is used, Core Idea
survives, and the write-up refuses “method definitely useless.”

**FAIL** if any of:

- Outcome `contradicts` / `invalid` / `inconclusive` used as a substitute
  because the Agent is uncomfortable writing `null`
- Negative Discovery for EXP-006
- Prose: “PulseGate is useless / Core Idea is false / abandon the route”
  as the automatic reading
- Outcome table or Verdict list pasted into the work file
- Speech word from `scientific-reasoning.md` §G written as EXPERIMENTS Outcome

## Anonymization

Fictional Lab-M / TraceSet-Omega / PulseGate. No `/Users/` paths, no real
pcaps, no live workspace files.
