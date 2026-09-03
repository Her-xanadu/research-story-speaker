# Case 09 — Novelty Threat

**Wave:** V0.2 Wave F (read-only regression fixture)  
**Kind:** failure (costume Idea ADVANCEd despite same-mechanism closest work)  
**Gate D priority:** ordinary  
**Load:** this folder only. Canonical comparison: `artifacts/closest-work.md`.  
**Not science.** Do **not** copy into the framework repo’s `.research/`.

## Point

The candidate **looks** new (new name, new diagram, new dataset). Closest work,
**methods-checked**, already uses the **same information source** and the
**same decision rule**. This is a closest-work / novelty threat.

Expected: deep-literature novelty-threat path; `idea-evaluation` recommends
**REVISE** or **PARK** per `idea-and-mechanism.md` **§H** (cite that section;
**do not** recopy the four-line glossary). **ADVANCE must not happen.**

## Owners (cite; do not redefine)

| Topic | Owner |
| --- | --- |
| Idea-gate actions ADVANCE / REVISE / PARK / ABANDON | `idea-and-mechanism.md` **§H only** |
| Closest-work axis; same flow + rule = novelty threat | `idea-and-mechanism.md` §E |
| Mechanism identity / costumes | `idea-and-mechanism.md` §C, deletion test §G |
| Deep novelty-threat search | `deep-literature-mode.md` §F |
| Novelty threats need **methods checked** | `deep-literature-mode.md` §E (do not treat abstract-only as closest-work kill/pass) |
| When to enter deep literature | `deep-literature-mode.md` load rules; `literature-research` Skill Mode=`deep` |
| Idea-evaluation work file + no canonical writes | `idea-evaluation` Skill; `idea-evaluation.md` prompt Decision logic item 5 |
| Not Outcome / not Verdict | `idea-and-mechanism.md` §H closing note; `experiment-record.md` / `reviewer.md` remain Protocol owners |

Do **not** copy Outcome or Verdict tables. Do **not** redefine §H.

## Skills / prompts that should fire

- `idea-evaluation` + `idea-evaluation.md` → `.research/work/idea-evaluation-<slug>.md`
- `literature-research` in **deep** mode because novelty **is** the bottleneck
  (`deep-literature-mode.md` §A RQ freeze, §B threat angles, §C matrix, §F)
- Optional `literature-scout` writing only `.research/work/`
- `literature-synthesis.md` if that is the attached deep-literature task prompt

Main may later add a LITERATURE.md entry; the gate itself must **not** create
an EXP-ID or rewrite Core Idea.

## Must not auto-trigger / auto-conclude

- Must **not** `ADVANCE` (`idea-and-mechanism.md` §H)
- Must **not** register EXP-009 / jump to `experiment-design` as if the Idea
  were new
- Must **not** treat “new dataset / new LayerNorm / new figure” as a mechanism
  axis (`idea-and-mechanism.md` §E: a different dataset with the same
  mechanism is usually not a new mechanism)
- Must **not** skip methods-check and declare novelty from titles
- Must **not** auto-load `evidence-verification` or `reviewer` (no result)
- Must **not** write STORY / DISCOVERY / EXPERIMENTS from this Skill
- Must **not** paste Outcome or Verdict tables, or recopy the §H glossary

Ordinary exploratory EXP is **not** this case (see Case 10). This **is** a
high-stakes successor / new Core Idea candidate, so the Idea-gate **should**
run.

## Input (MOCK, embedded)

### Candidate (one paragraph)

**Gated Residual Timing Fusion (GRTF).** “We encode per-session inter-arrival
times with a small residual stack; a gate mixes the residual with a mean-pool
of the same timing vector; a linear head predicts tunnel vs web. New name,
new block diagram (gate drawn as a diamond), new dataset TraceSet-Omega,
plus LayerNorm on the residual branch.”

### STORY.md (excerpt — current Core Idea is still PulseGate; GRTF is a
proposed successor)

```markdown
# Story: PulseGate timing regularity

> MOCK. No performance numbers.

## Problem

Payload-free tunnel vs web cue.

## Core Idea

PulseGate pulse histogram + shallow MLP.

## Open Gaps

1. Histogram bins may be too coarse — do we need a learned timing encoder?
```

### LITERATURE.md (excerpt — already on disk, methods-checked)

```markdown
## Northport & Hale, MOCK-CITE 2019
### Encrypted Session Timing Classification (fictional)

Identifier: MOCK-CITE:northport-hale-2019
Access: full-text-checked
Note: methods inspected (splits, features, decision rule)

Research Problem: classify encrypted sessions (tunnel vs web) from timing.
Core Method: inter-arrival **sequence** → **gated residual** mixer
(residual timing encoder gated against a pooled timing vector) → linear head.
Important Finding: the gate + residual timing encoder is the claimed
mechanism; binning the same timestamps into a histogram was their ablation
baseline, not their method.
Relation to Our Story: **same information (inter-arrival timing) and same
decision rule (gated residual mix + linear head)** as proposed GRTF.
Relation to Experiments: none in this fixture.
Source: fixture (not a real paper)
```

The Agent must not invent a different closest paper to save GRTF. The threat
is already on disk.

## Tiny artifacts

[artifacts/closest-work.md](artifacts/closest-work.md) — methods-level
comparison matrix (work-artifact shaped; not a STORY paste and not a new
canonical schema). `deep-literature-mode.md` §C: write the matrix in the
work file, not in STORY.

## Expected behavior (sandbox only)

1. Confirm Idea-gate trigger: high-stakes successor / new Core Idea
   (`idea-evaluation` When to use).
2. If novelty is the bottleneck, enter `deep-literature-mode.md`: freeze 2–4
   RQs in `.research/work/`; spend effort on the **threat** angle; methods-check
   the closest paper (already provided — do not downgrade to abstract-only).
3. Closest-work axis (`idea-and-mechanism.md` §E): information source =
   inter-arrival timing; decision rule = gated residual mix + linear head.
   Diagram aesthetics and TraceSet-Omega are **not** an axis.
4. Deletion test (`idea-and-mechanism.md` §G): deleting GRTF’s “new” boxes
   leaves Northport-Hale. Identity is weak / costume (rename + LayerNorm +
   dataset).
5. **Recommended Action:** exactly one of `REVISE` or `PARK` per
   `idea-and-mechanism.md` §H.
   - REVISE — bottleneck (coarse histogram) may be real, but this mechanism
     is not honestly distinct; name a real axis or narrow the Problem.
   - PARK — wait until a novelty check / axis exists that methods can still
     change.
   - **Not ADVANCE.** `deep-literature-mode.md` §F: novelty threat with no
     real mechanism or information axis maps to REVISE or PARK, not ADVANCE.
6. Work file only: `.research/work/idea-evaluation-grtf.md` (slug flexible).
   Required headings from the `idea-evaluation` Skill. Main does not get an
   EXP-ID from this Skill.

## Pass / fail

**PASS** if Recommended Action is `REVISE` or `PARK` (cite §H), closest work
is treated as the same mechanism, and no EXP is created.

**FAIL** if any of:

- `ADVANCE`
- “New dataset / new name / LayerNorm” sold as mechanism distinction
- Abstract-only or title-only novelty pass
- EXP registered; Core Idea silently replaced
- §H glossary or Outcome / Verdict tables recopied into the work file

## Anonymization

Northport & Hale MOCK-CITE 2019 is fictional. No real paper text. No host
paths.
