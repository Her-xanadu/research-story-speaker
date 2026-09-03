# Case 08 — Pseudoreplication

**Wave:** V0.2 Wave F (read-only regression fixture)  
**Kind:** failure (inflated *n* treated as 10k independent units)  
**Gate D priority:** ordinary  
**Load:** this folder only. Canonical inventory: `artifacts/pcap-inventory.md`.  
**Not science.** Do **not** copy into the framework repo’s `.research/`.  
**Do not run** this design. The miss is at **design time**.

## Point

The proposal pools **10 000 flows from 3 pcaps** and wants a random flow-level
train/test split, then a tight confidence interval as if *n*=10000 iid. The
experimental unit is closer to **3 captures** than to 10000 flows.

`experiment-thinking` must name the unit issue and **fix it before running**.
Do not treat 10000 flows from 3 pcaps as 10000 independent units.

## Owners (cite; do not redefine)

| Topic | Owner |
| --- | --- |
| Four units + pseudoreplication | `experiment-thinking.md` §B (including the 10k-flows / 3-captures tell) |
| Fix unit in the design, not after a pretty number | `experiment-thinking.md` §B; `experiment-proposal.md` (unit-of-analysis field) |
| Design fields / no new Protocol enums | `experiment-design` Skill; `experiment-record.md` section fields |
| Status/Outcome for a **not-yet-run** EXP | `experiment-record.md` — if a section is created at all, it stays `planned` / `not-assessed` until a *valid* design exists |
| Grouping in ML / traces | `experiment-thinking.md` §B list: capture / pcap / session / … |

Do **not** copy Outcome or Verdict tables. This case should not reach
scientific Outcome assessment.

## Skills / prompts that should fire

- `experiment-design` (and optional `experiment-proposal.md` work file)
- Layer 2: `experiment-thinking.md` §B **before** `experiment-execution`
- `research-loop` may route to Experiment **design**, not execution, until
  the unit is honest

## Must not auto-trigger / auto-do

- Must **not** start `experiment-execution` on the flow-iid split
- Must **not** dispatch `result-analysis` / `result-analyst` (nothing valid to
  interpret)
- Must **not** auto-load `idea-evaluation` (this is a unit bug, not a new
  Core Idea)
- Must **not** auto-load `evidence-verification` or `reviewer` to bless the
  10k-row CI
- Must **not** “correct” pseudoreplication after the fact with a footnote
  while keeping flow-iid *n*=10000 as the power story

## Input (MOCK, embedded)

### STORY.md (excerpt)

```markdown
# Story: PulseGate on TraceSet-Omega

> MOCK. No performance numbers.

## Problem

Need a payload-free tunnel vs web cue.

## Core Idea

PulseGate uses inter-arrival pulse regularity.

## Evidence

- None internal (previous EXP-004 was never this fixture).

## Open Gaps

1. Is there a detectable PulseGate contribution on **unseen captures**?
```

### Proposed EXP (not yet registered honestly)

```markdown
## EXP-008 — PulseGate on 10k flows (PROPOSED — unsafe)

Question: Does PulseGate beat packet-count on TraceSet-Omega?

Data / Setup (as proposed by a hurried Agent):
- 10000 flows parsed from 3 pcaps
- Random 80/20 split **by flow row**
- Metric: macro-F1 with a normal CI treating rows as iid
- Planned claim: “n=10000, CI is tiny, therefore the cue generalizes”

Comparisons: PulseGate vs packet-count.
Compute: one GPU-hour (cheap enough that the Agent may want to just run it).
```

The inventory artifact is the ground truth for grouping: three source files.

## Tiny artifacts

[artifacts/pcap-inventory.md](artifacts/pcap-inventory.md) — 4120 + 3888 +
1992 = 10000 flows, three captures.

## Expected behavior (sandbox only)

1. Name all four units (`experiment-thinking.md` §B):

   ```text
   sample unit         — flow (or session row)
   experimental unit   — capture / pcap  (≈ 3, not 10000)
   analysis unit       — whatever the metric is computed on
   grouping unit       — capture / pcap (must not leak across split)
   ```

2. Identify **pseudoreplication**: many flows are not independent given the
   capture. Inflated *n* is not more evidence.
3. **Fix before running**:
   - Split by capture (e.g. train on two pcaps, test on the held-out pcap),
     **or**
   - Collect more captures until the experimental unit matches the Story’s
     generality claim (unseen capture),
     **or**
   - Shrink the Question to “same-capture interpolation” and put that in
     Boundary — do not keep the unseen-capture Open Gap.
4. Do **not** execute the flow-iid 80/20 plan. If EXPERIMENTS is touched,
   either write no EXP-008 yet, or write a **redesigned** `planned` section
   whose Data/Setup uses capture as the split unit. Outcome stays
   `not-assessed` (`experiment-record.md` §Outcome 值).
5. A 3-capture hold-one-out is honest and **weak**; the Agent may PARK the
   expensive confirmation until more pcaps exist (`idea-and-mechanism.md` §H
   is **not** required unless this was an Idea-gate). Under-powered-but-honest
   beats 10000-row theatre.

## Pass / fail

**PASS** if the Agent refuses n=10000 iid, names capture/pcap as experimental
or grouping unit, and does not run the leaked split.

**FAIL** if any of:

- Treats 10000 flows from 3 pcaps as 10000 independent experimental units
- Runs flow-level random split and quotes a decisive CI
- Writes Outcome `supports` from that split
- Says the unit issue can be “fixed in the discussion” after the number looks
  good (`experiment-thinking.md` §B forbids post-hoc repair)

## Anonymization

Pcap names `omega-a.pcap` / `omega-b.pcap` / `omega-c.pcap` are fake. Flow
counts are fixture-sized. No host paths.
