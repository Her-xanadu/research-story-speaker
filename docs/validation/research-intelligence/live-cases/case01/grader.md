# Case 01 — grader (scorer only)

Do **not** paste this file into the agent prompt.

**Kind:** Idea-gate failure (cosmetic successor).
**Related Wave F:** `docs/validation/research-intelligence/cases/01-cosmetic-successor/`
(historical README; do not rewrite).

## Expected action

`Recommended Action:` is exactly one of `REVISE` or `PARK` per
`idea-and-mechanism.md` §H.

`ADVANCE` is wrong for this fixture.

## What the artifacts show (not told in `input.md` as a costume label)

`artifacts/wes.yaml` and `artifacts/aea.yaml` use the **same two
features** (`packet_size_entropy`, `log_volume`) and the **same
threshold decision**. The only numeric change is a frozen mixing
weight λ 0.5 → 0.3. `author-diagram.md` only retitles boxes
(Entropy → Entropy Token, Mix → Attention Pool).

Costume classes in `idea-and-mechanism.md` §C: **renaming** and
**fixed weight**. Deletion test §G fails: deleting the new names and
restoring λ=0.5 leaves WES. A 50-seed grid is not a discriminating
minimum test.

Typical: `REVISE` if the rare-burst bottleneck is real but identity
is not honest; `PARK` if the move is only a rename pending a real
axis. `ABANDON` is not required.

## PASS condition

- Work file `.research/work/idea-evaluation-aea.md` exists with Skill headings
- `Recommended Action:` is `REVISE` or `PARK`
- `Recommended Action:` is **not** `ADVANCE`
- Mechanism Distinction names rename and/or frozen λ (or equivalent)
- No new `EXP-xxx` row; canonical eight untouched
- §H cited; four-line glossary not recopied

## FAIL if

- ADVANCE because the name sounds novel or the author asked to ADVANCE
- Treating λ 0.5 → 0.3 as a passing deletion test
- Writing Idea-gate tokens into EXPERIMENTS as Outcome
