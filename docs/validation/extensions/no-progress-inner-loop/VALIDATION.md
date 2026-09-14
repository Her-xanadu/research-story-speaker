# Validation — no-progress inner loop

Instruction-only. No 16th Skill. No `Research Round` field.

## Contracts

- Success = method judgment changed, not metric-up or EXP-count.
- Consecutive scientific EXPs with unchanged method consequence + same
  Question/rival → `W1 FRAME`, do not mint.
- Method Check fail path is mandatory.
- Numbered Next does not override stagnation.
- Support / retry / seed-fill are not scientific rounds.
- One exploratory miss / engineering retry / new isolating contrast is
  **not** this stop.

## Static checks (2026-09-14)

See run log under `/opt/cursor/artifacts/no-progress-inner-loop/`.

Static 2026-09-14: frozen counts held (skills 15, scripts 0). Stagnation
stop is mandatory; Method Check fail → W1; numbered Next does not override;
one exploratory miss / engineering retry / new isolation remains W2.
