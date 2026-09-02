# EXP-001 Result Review

**Reviewer:** Grok 4.6
**Model relation:** different-family
**Context relation:** fresh-context
**Date:** 2026-09-02
**Reviewed code commit:** `b0621e2ed266cc26020fac5b3295a588469bb495`
**Review round:** 2
**Provenance:** synthesis
**Source reviews:**
- result-review-r1.md
**Status:** done

## Verdict

PROCEED

## strongest evidence

At commit `b0621e2ed266cc26020fac5b3295a588469bb495`, `results/EXP-001/metrics.json` reports 3-feature LR test F1 = **1.0000** vs full-feature IF test F1 = **0.5455** (gap −45.45 pp; n_train=40, n_test=10, seed 42). On this MOCK split the three-feature supervised pipeline is numerically stronger than the recorded IF baseline.

## main weakness

n_test=10 with a perfect F1 is too small to support generalization. The IF threshold calibrated on 40 training points can be unstable. Results are bound to a synthetic 50-row table, not CICIDS2017.

## alternative explanation

Perfect LR F1 may come from **linear separability of the synthetic generator** rather than from “three features suffice” as a scientific claim: if labels were planted on duration / packet-ratio / byte-asymmetry, a linear model will look complete by construction. The weak IF (0.5455) may come from the **toy implementation and parameters** (tree count, depth, threshold rule) rather than from full-feature representations being invalid; a stronger full-feature baseline could reverse the gap.

## story impact

Accept as **MOCK-tier Story Evidence** only. Core Idea gains internal support on the toy distribution; IF-as-strong-baseline is not established; real-subset conclusions stay open. Do not copy these F1 numbers into STORY or DISCOVERY.

## recommended next move

Do not promote Story Status on this run. Replicate the protocol on a real CICIDS subset with a stronger full-feature control; keep EXP-001 section and this review as the MOCK SoT.
