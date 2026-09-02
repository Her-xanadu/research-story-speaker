# EXP-001 Method Review

**Reviewer:** Grok 4.6
**Model relation:** different-family
**Context relation:** fresh-context
**Date:** 2026-09-02
**Reviewed code commit:** `b0621e2ed266cc26020fac5b3295a588469bb495`
**Review round:** 2
**Provenance:** synthesis
**Source reviews:**
- method-review-r1.md
**Status:** done

## Verdict

PROCEED

## strongest evidence

Paired comparison on one split (seed 42, 80/20) with a feature set that matches the Story Core Idea (duration, packet_ratio, byte_asymmetry) and a consistent per-flow z-score applied before both models. Entry at the cited commit is `experiments/EXP-001/run.py` with config `experiments/EXP-001/run_config.yaml`; results at `results/EXP-001/metrics.json`.

## main weakness

The dataset is synthetic (50 flows), not a real CICIDS2017 subset, so external validity is limited. The Isolation Forest side is toy-grade (25 trees, shallow depth), not a sklearn-grade full-feature strong baseline. There is no cross-validation and only one seed; metrics at n_test=10 can be noisy.

## alternative explanation

A clean 3-feature vs IF comparison on this generator may succeed because the synthetic labels were built from (or are linearly aligned with) those three statistics, not because three statistics are scientifically sufficient on real traffic. Using unsupervised IF against supervised LR also confounds “extra features do not help” with “this unsupervised toy is the wrong control class.”

## story impact

The design is adequate to test whether three statistics carry separable signal **on MOCK data**. It does not license promoting the Core Idea beyond MOCK-tier Story Evidence. Open Gap #1 (real subset vs an effective full-feature baseline) remains open.

## recommended next move

Keep EXP-001 as a MOCK closed loop. Next: rerun the same protocol on a real CICIDS subset (Open Gap #1), and only then design attack-family ablation. Do not treat the current IF implementation as the strong baseline required by PROJECT completion criteria.
