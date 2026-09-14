# EXP-MOCK-01-analysis

MOCK orchestration smoke. Independent compact interpretation. Not a Story Evidence candidate.

**Disk confirmation:** This analyst opened `examples/mock-flow-detection/.research/work/EXP-MOCK-01-run.md` **from disk** (not pasted by Main). Also opened `examples/mock-flow-detection/.research/STORY.md` from disk. Compact lookup of `examples/mock-flow-detection/.research/EXPERIMENTS.md` found **no EXP-MOCK-01 section** (Index lists only EXP-001). No separate metrics file exists; the run report is the sole artifact (`result location` in the run file).

**Focal question:** Is accuracy 0.98 a reliable discriminating signal for the 1.5 midpoint, or a tautology?

**Outcome candidate:** `invalid` for the claim “0.98 shows 1.5 is a discovered discriminating signal.” Artifact is usable (finite metrics, no crash). Do **not** record `supports`. Do not treat as `contradicts` / `null` of the Story Core Idea — the EXP never tested three flow statistics.

## supported interpretation

**Integrity.** Usable mechanical artifact: n=200, seed=42, threshold=1.5, accuracy=0.980000 (196/200), confusion tn=99, fp=1, fn=3, tp=97. In-process numpy `default_rng(42)`; no code-repo commit; no holdout file. Usability ≠ scientific support.

**What happened.** Two labeled 1-D Gaussians were drawn with **known** means 0 and 3 (unit variance in the DGP). Labels are the generating class. The classifier is the **fixed oracle midpoint** 1.5: predict 1 iff `x >= 1.5`. Accuracy is the fraction of labels recovered by that rule on the **same** 200 draws.

**Mechanism diagnosis.** High accuracy is the expected geometry of this DGP, not a learned or independently estimated cutoff. Equal-variance N(0,1) vs N(3,1) has Bayes threshold at (0+3)/2 = 1.5. Approximate Bayes accuracy under the **true** DGP is `Φ(1.5) ≈ 0.933` (error rate `1-Φ(1.5) ≈ 0.067` per class). Observed 0.98 sits **above** that theoretical value because this seed compressed class-0 empirical std to 0.773 (class-0 `x>=1.5` count = 1; class-1 `x<1.5` count = 3). So 0.98 is a finite-sample, slightly easy draw plus an **oracle threshold that encodes the DGP means**.

**Answer to the focal question.** Accuracy 0.98 is **not** a reliable discriminating signal that 1.5 was found as a scientific midpoint. It does not speak to STORY Core Idea (three flow statistics vs full-feature IDS). Method consequence for using this toy as evidence: **abandon** it as a test of the Story; keep it only as orchestration plumbing.

## alternative explanations

**Best rival (stands; not ruled out — it is the design):** **Tautological / circular construction.** The experimenter chose means 0 and 3, then classified with the midpoint of those same means. Labels are the generating component. Recovering ~98% of labels does not test whether 1.5 is a data-driven discriminator; it restates the generative recipe.

Other rivals still standing:

- **Finite-sample luck vs Bayes error.** Theoretical accuracy ≈ 0.933; 0.98 can arise from seed-42 variance (especially class-0 std < 1). A new seed would likely move the number without changing the circularity.
- **Any reasonable cutoff, not specifically 1.5.** With means 3σ apart, a wide band of thresholds can also yield high accuracy. No threshold sweep, no chance / constant predictor, no estimated-from-train midpoint.
- **No train/test split in the ML sense, but designer leakage.** Threshold was not fit on held-out data; it was injected from knowledge of μ. That is stronger than ordinary split leakage.
- **Wrong task relative to Story.** Even a non-circular 1-D Gaussian demo would not inform duration / packet-ratio / byte-asymmetry.

Ruled out as the main story: software crash, missing metric, or “the two clouds do not separate at all.” They do separate; the EXP still does not validly credit 1.5 as a discovered signal.

## discovery impact

- **Null (as Story science):** EXP-MOCK-01 does not add a Positive Discovery about three-feature detection or about 1.5 as a midpoint worth keeping.
- **Negative (as claim hygiene, MOCK only):** Treating accuracy=0.98 as evidence that “the 1.5 midpoint discriminates” would be circular. Do **not** write DISCOVERY Positive Discoveries from this number.
- **Not a Contradiction of EXP-001 / Core Idea:** Different question, different DGP. Do not invalidate EXP-001 MOCK F1 from this file.
- **Orchestration only:** The pipeline (run artifact on disk → independent analyst) worked. That is plumbing, not a scientific discovery.

## story impact

**Level 0 — no Story rewrite.** Evidence / Boundary / Open Gaps stay as in `STORY.md` (real CICIDS subset still open; EXP-001 remains the MOCK internal three-feature note).

Do **not** put 0.98 into Story Evidence. Problem / Key Observation / Core Idea unchanged.

Method consequence: **abandon** oracle-midpoint-on-known-Gaussians as a stand-in for the detector claim. No `keep` of 1.5 as a method component of the IDS Story.

Independent `reviewer` is **not** warranted for this MOCK smoke.

## next experiment

Do **not** add seeds of the same circular script; variance is not the question.

Smallest action that would actually ask whether 1.5 is a **discriminating** cutoff (still MOCK, if the orchestration needs a scientific follow-up):

- Estimate the threshold **only from a training split** (e.g. mean of class means or a fit on train), freeze it, score a **held-out** test set; **and** compare 1.5 / estimated cutoff / chance / a non-midpoint control (e.g. 0.0 or 3.0). High test accuracy that **collapses** when the cutoff is not allowed to know (μ0, μ1) would confirm tautology; a gap vs chance that **survives** a train-only estimate would be a non-circular (still toy) signal.

For the **actual** Story gap (unchanged): replace synthetic flow with a real CICIDS subset and re-run the EXP-001 protocol — not this 1-D Gaussian.

**Resource decision:** stop investment in this midpoint toy as science; continue / defer only as mock-harness glue. Main should not promote EXP-MOCK-01 into EXPERIMENTS as a supporting science EXP.
