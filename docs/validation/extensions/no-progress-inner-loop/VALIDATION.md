# Validation — no-progress inner loop

Instruction-only. No 16th Skill. No `Research Round` field.

## Contracts

- Inner loop **may** continue for many rounds (`W2 TEST` default).
- Yield = usable scientific answer, and if the method failed, the next
  EXP tests the **changed** method.
- Forbidden: `keep` + same failing contrast.
- W1 only when no method change can be named (or A/B/C reframe).
- Support / retry / seed-fill do not count as tried.
- One exploratory miss → new isolation stays W2.

## Static 2026-09-14

See `/opt/cursor/artifacts/no-progress-inner-loop/validation.log`.

2026-09-14 retune: inner loop may continue; fail → change method → W2.
Forbidden is `keep` + same contrast, not “too many rounds.” Skills 15.
