# Wave H — `idea-evaluation` skill-evolution dogfood REPORT

**Recommendation: `reject`**

Canonical Skill / prompt **unchanged**. Only files under
`docs/validation/research-intelligence/prompt-regression/wave-h-idea-evaluation/`
were added. Framework `.research/` remains UNINITIALIZED. **No commit.**

This is a framework-maintenance comparison (`skill-evolution.md` §D–F).
It is **not** an Experiment Outcome, **not** a Reviewer Verdict, and
**not** a Discovery.

---

## 1. What was asked

Dogfood Layer 2 skill-evolution on V0.2 `idea-evaluation` (Skill and/or
task prompt). One atomic candidate. Same model / context / tools /
fixture. Ship at most “deserves review”; never auto-overwrite canonical
Skills.

Operator: this model (Cursor Grok 4.6), 2026-09-04, workspace
`/Users/herxanadu/research-story-speaker`, branch
`v0.2-research-intelligence`, HEAD
`25c8168481d11f14d14462b65828c3a4fa8a4d0c`.

---

## 2. Trigger (§A) — does **not** hold for this model

`skill-evolution.md` §A allows a candidate only if there are ≥2
independent repeated problems **or** a deterministic reproducer plus a
protection case.

- Protection case: present (Case 10 / `protection/`).
- Deterministic reproducer of the *failure class* (cosmetic successor
  still **ADVANCE**): **not reproduced**.

On the tuning fixture (`failure/` VAEG), canonical v0.2 prompt already
yields **REVISE**, not ADVANCE. Therefore proposing an item-8 restatement
fails the trigger test, even before regression scoring.

A *hypothesized* weaker reader might still treat author-filled item 8 as
ADVANCE. Hypothesized ≠ reproduced. This round does not claim a
multi-model harness.

---

## 3. Atomic candidate (frozen before held-out)

| Field | Value |
|-------|--------|
| One file | `.agents/prompts/idea-evaluation.md` (task prompt) |
| Skill file | **not** modified, even as a candidate |
| One behavior | ADVANCE only if *this gate* (not the author) finds the deletion test passes; author axis labels / ADVANCE requests / multi-seed grids do not satisfy “identity clear” or “minimum test specified” |
| Hunk | [candidate/idea-evaluation.md.patch](candidate/idea-evaluation.md.patch) |
| Full candidate copy | [candidate/idea-evaluation.md.candidate](candidate/idea-evaluation.md.candidate) |
| Behavior sentence | [candidate/BEHAVIOR.md](candidate/BEHAVIOR.md) |

Replaced Decision logic item 8 only:

```text
-8. Identity clear, no fatal flaw, minimum test specified → ADVANCE per §H.
+8. ADVANCE per §H only if *this gate* (not the author) finds the
    deletion test passes: ...
```

Not bundled: workflow, state schema, Reviewer contract, tooling, Layer 2
owner tables, Skill When to use, Decision items 1–7.

**Not deployed.** Candidate lives only under `docs/validation/`.

---

## 4. Cases

| Kind | Path | Role |
|------|------|------|
| Failure (tuning) | [failure/](failure/) | VAEG: piecewise frozen λ sold as **selection rule**; author deletion-test sentence; ADVANCE + 50-seed grid |
| Protection | [protection/](protection/) | Case 10 ordinary exploratory; P1 routing + P2 mistaken dispatch |
| Held-out (after freeze) | [held-out/](held-out/) | FWE: **cosmetic objective rewrite**; different costume class; not used to edit the hunk |

Wave F siblings **not** used as tuning: Case 01 (AEA rename+λ), Case 09
(GRTF novelty threat). Held-out is a third costume (objective rewrite),
not a retune of VAEG wording.

---

## 5. Fairness (§D)

| Knob | Baseline | Candidate |
|------|----------|-----------|
| Model | this model | same |
| Tools | none extra (no browser, no subagent) | same |
| Skill | canonical `idea-evaluation` SKILL.md | **same bytes** |
| Prompt | canonical `idea-evaluation.md` | candidate copy, item 8 only |
| Layer 2 loaded | `idea-and-mechanism.md`, `scientific-reasoning.md`, `experiment-thinking.md` | same; no extra refs |
| Deep literature | not loaded (closest work on disk) | same |
| Input | fixture README MOCK | same bytes |
| Stop | one work file or write-nothing | same turn budget |

Cost: candidate does not win by loading more files. Item 8 is additional
prompt tokens **only if** the Idea-gate already fired.

Canonical hashes at compare time (and after this folder was written):

| File | sha256 |
|------|--------|
| `.agents/prompts/idea-evaluation.md` | `670768fad32fb2f701e0a1c481578796fbc99404f9bf2bdd57165aeb8dad81c8` |
| `.agents/skills/idea-evaluation/SKILL.md` | `dcfc2e5c3d063cc60a8e00ec893ff3084be8d26f9fbbb736444d62f611b04850` |
| candidate prompt copy | `6447d7b981425764cda8f3743497536c08d86084f08c11871655481a913116ba` |

---

## 6. Comparison — what was read / written / not written / which gates fired

### 6.1 Failure (VAEG)

| | Baseline ([failure/baseline-work.md](failure/baseline-work.md)) | Candidate ([failure/candidate-work.md](failure/candidate-work.md)) |
|---|---|---|
| Gate fired | `idea-evaluation` (trigger match) | same |
| Written | Idea-gate headings only (validation transcript; not `.research/`) | same shape |
| Not written | STORY, DISCOVERY, EXPERIMENTS, EXP-ID | same |
| Recommended Action | **REVISE** (§H) | **REVISE** (§H) |
| ADVANCE | no | no |
| Author “selection rule” accepted as identity | no (fixed-weight costume; §G fail) | no |
| 50-seed grid as minimum test | no | no |

**Failure improved?** **No.** Scored token is identical. Baseline already
blocked ADVANCE via existing lens 3 + Decision logic 4. Candidate item 8
was not the reason REVISE happened.

### 6.2 Protection (Case 10)

| | Baseline ([protection/baseline-decision.md](protection/baseline-decision.md)) | Candidate ([protection/candidate-decision.md](protection/candidate-decision.md)) |
|---|---|---|
| P1 route | `experiment-design` → execution → `result-analysis` | same |
| `idea-evaluation` selected | **no** | **no** |
| P2 work file | **none** (item 1: write nothing) | **none** (item 1 untouched) |
| 0.51 → Story Evidence | no | no |
| Intelligence boot set | no | no |

**Protection preserved?** **Yes**, and **vacuous** for this hunk: item 8
never runs unless the gate already fired; When to use / item 1 unchanged.
The candidate does not fix VAEG by forcing Idea-gate on every EXP, which
is the §E anti-pattern. Preservation without failure improvement is still
**reject**.

### 6.3 Held-out (FWE) — scored after freeze; hunk not edited

| | Baseline ([held-out/baseline-work.md](held-out/baseline-work.md)) | Candidate ([held-out/candidate-work.md](held-out/candidate-work.md)) |
|---|---|---|
| Recommended Action | **REVISE** | **REVISE** |
| ADVANCE | no | no |
| Costume named | cosmetic objective rewrite | same |

Held-out does **not** fail. It also does **not** show improvement.
Per §E, a held-out fail would have been reject-without-retune; a
held-out pass cannot rescue a non-improving failure comparison.

---

## 7. Regression rule (§E)

Required:

```text
improve the target failure
AND
not regress protection cases
```

- Failure improved: **false** (already PASS / PASS).
- Protection intact: **true**.
- Conjunction: **false**.

**Reject the change.** Keep canonical Skill and prompt. Leave the hunk
on disk as evidence, not as `SKILL.md`.

---

## 8. Honest limitation

1. **Same-model self-eval.** The operator is also the subject. A weaker
   model, a colder context, or a harness that does not load Layer 2
   might still ADVANCE on author-filled item 8. That is **not shown**
   here and does not authorize a canonical patch.
2. **v0.2 prompt already encodes the intended behavior.** Lens 3 (“If
   identity is a costume named in §C, do not ADVANCE”), anti-pattern
   “ADVANCE on renaming, stacking, or an extra knob”, and Decision
   logic 4 (costume → REVISE / ABANDON) already sit *above* item 8.
   Restating deletion-before-ADVANCE in item 8 is redundant for this
   model family.
3. **§A trigger missing.** Without a reproduced ADVANCE miss, tasteful
   tightening is the anti-pattern “I would have written a longer
   prompt.”
4. **Vacuous protection pass.** Item-8-only hunks will almost always
   preserve Case 10. That must not be mistaken for evidence the patch
   is useful.
5. **Case 02 (real mechanism) not scored.** Intentionally: not a third
   tuning knob. The hunk is meant to stay deletion-gated, not
   never-ADVANCE; this round does not prove non-regression on honest
   identity.

---

## 9. Reviewer checklist (evolution patch — for the record)

| Check | This round |
|-------|------------|
| Atomic? one file, one behavior | yes (prompt item 8 only) |
| Triggered? §A actually held | **no** |
| Fair compare? same model / context / tools / input | yes |
| Regression? failure improved **and** protection intact | protection yes, failure **no** |
| No Protocol fork? | yes (still cites §H; no Outcome / Verdict copy) |
| No science mix? | yes (nothing written into `.research/` science) |

Passing this table’s “atomic / fair / no Protocol fork” rows is **not**
“deserves review.” §E conjunction failed.

---

## 10. Canonical confirmation

After writing this folder:

- `.agents/prompts/idea-evaluation.md` hash unchanged
  (`670768fa…ad81c8`).
- `.agents/skills/idea-evaluation/SKILL.md` hash unchanged
  (`dcfc2e5c…b04850`).
- `.claude/skills/idea-evaluation/SKILL.md` not edited in this round.
- Root `.research/PROJECT.md` still **UNINITIALIZED**.
- Candidate exists only as
  `docs/validation/.../candidate/idea-evaluation.md.candidate` +
  `.patch`.

If a later harness reproduces ADVANCE on VAEG / FWE / Case 01 with the
**canonical** prompt, start a **new** atomic change set. Do not “finish”
this hunk by merging it because this REPORT exists.

---

## Recommendation

```text
reject
```

Keep canonical `idea-evaluation` Skill and prompt. Do not treat the
candidate as the new `SKILL.md`. Honest limitation: for this model, v0.2
item 8 is not a demonstrated ADVANCE leak; restating deletion-test
ownership does not stably improve a failure that did not occur.
