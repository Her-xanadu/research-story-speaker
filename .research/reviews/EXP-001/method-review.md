# EXP-001 Method Review

**Reviewer:** MOCK independent reviewer (Codex subagent)
**Date:** 2025-09-02
**Status:** done

## Scope

Review EXP-001 design: 3-feature logistic regression vs full-feature Isolation Forest on synthetic flow statistics.

## Strengths

- Clear paired comparison on identical train/test split (seed 42, 80/20).
- Feature sets align with Story Core Idea (duration, packet_ratio, byte_asymmetry).
- Per-flow z-score normalization applied consistently before both models.
- Reproducible entry point: `experiments/EXP-001/run_config.yaml` + `run.py`.

## Concerns (MOCK)

- Dataset is synthetic (50 flows), not real CICIDS2017 — external validity limited.
- IF implementation is toy-grade (25 trees, shallow depth); not sklearn reference.
- No cross-validation or multiple seeds; single-run metrics may be noisy at n_test=10.

## Verdict

**Approve for MOCK V0.1 loop.** Design is adequate to test whether three statistics carry separable signal. Recommend follow-up with attack-family ablation and larger real or semi-real subset before Story promotion.
