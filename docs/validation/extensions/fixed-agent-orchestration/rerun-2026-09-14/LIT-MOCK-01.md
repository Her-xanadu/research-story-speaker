# LIT-MOCK-01 — literature-scout consult (MOCK orchestration smoke)

Role: literature-scout  
Mode: light  
EXP-ID: N/A  
Story gap: MOCK orchestration smoke — no vault  
Focal Scientific Question: Is a local paper vault available for this MOCK  
Decision This Task Can Change: whether literature consult can proceed

## story gap addressed

MOCK orchestration smoke: confirm whether a local Obsidian paper vault exists so literature consult can proceed; the scientific Story gap (whether three flow statistics suffice vs CICIDS/full-feature baselines) is not answered here.

## consult_status

unavailable

No Obsidian vault is available in this MOCK. `paper-consult` was not run. No web / arXiv / Semantic Scholar / `paper-find` search was performed.

## sources found

- none — vault unavailable; consult did not execute
- Project-memory pointers in `examples/mock-flow-detection/.research/LITERATURE.md` (Sharafaldin 2018; Liu et al. 2008 Isolation Forest) were read as project notes only. They are not vault hits and were not treated as full-text consult evidence.

## key claims and evidence

- Local paper vault: **absent**. Adequacy four-dimension check cannot pass without consult hits.
  - coverage: fail (no vault cards to answer the focal question or the Story gap)
  - access_depth: fail (no `论文综述.md` / `精读.md` / PDF)
  - closest_work: fail (cannot verify nearest IDS/feature-ablation line from vault)
  - freshness: fail (no vault recency signal)
- LITERATURE.md mock entries remain abstract-level project memory, not methods-checked vault objects. Scout does not promote them to consult evidence.

## gaps remaining

Literature consult cannot proceed until Main attaches or builds a vault and re-runs consult. Story Open Gaps (real CICIDS subset; normalization; attack-family extra features) stay bibliographic-unresolved from this task.

## relation to our story

Does not support, challenge, or narrow the Story. Consult blocked before synthesis. MOCK Story Evidence citing “LITERATURE — Sharafaldin 2018” is unchanged by this scout and is not re-verified.

## needs_refresh

true

```text
NEEDS_REFRESH
suggested_query: problem: CICIDS2017 / flow-level IDS with few interpretable statistics vs high-dimensional CICFlowMeter features; method: 3-feature lightweight classifier (duration, packet-ratio, byte asymmetry) vs full-feature Isolation Forest or similar; finding: whether a small flow-stat set remains discriminative across attack families; relation: closest-work and evaluation convention for 3-feature vs full-feature on CICIDS2017 subsets
missing_evidence: coverage | access_depth | closest_work | freshness
```

## suggested next literature actions

Main only (scout does not execute): provision an Obsidian vault; run `paper-find` → `paper-library` ingest for the suggested_query; then re-dispatch literature-scout `paper-consult`. Do not merge this file into `LITERATURE.md` as new paper evidence. Proposed canonical writes: none from scout.
