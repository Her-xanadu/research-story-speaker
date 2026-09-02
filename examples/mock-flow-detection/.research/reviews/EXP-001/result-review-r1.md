# EXP-001 Result Review

**Reviewer:** MOCK independent reviewer (Codex subagent)
**Model relation:** unknown
**Context relation:** unknown
**Date:** 2025-09-02
**Reviewed code commit:** `b0621e2ed266cc26020fac5b3295a588469bb495`
**Review round:** 1
**Provenance:** raw
**Recovered from commit:** 5540420
**Status:** done

---

## Results Summary

| Model | Test F1 |
|-------|---------|
| 3-feature LR | 1.0000 |
| Full-feature IF | 0.5455 |
| Gap (IF − LR, pp) | −45.45 |

Train/test: 40 / 10 flows; seed 42.

## Assessment

- **3-feature LR** achieves perfect test F1 on the MOCK subset — strong support for Core Idea on synthetic data.
- **Full-feature IF** underperforms substantially; extra features do not rescue the toy baseline at this sample size.
- Negative gap (−45.45 pp) means 3-feature is *better* than full-feature IF, well inside the <5% tolerance from EXPERIMENTS Next criteria.

## Anomalies / Caveats

- Perfect F1 on n_test=10 is plausible given deliberate synthetic separability but should not be over-interpreted.
- IF threshold calibrated on train may be unstable with 40 training points.

## Verdict

**Accept results for Story Evidence (MOCK tier).** Update STORY Evidence and DISCOVERY with positive finding. Proceed to attack-family ablation (EXP-002 candidate) before claiming generalization.
