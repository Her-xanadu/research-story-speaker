# Experiments

> **MOCK** — 单文件总账。字段规范：`.agents/references/experiment-record.md`

## Index

| EXP-ID | Title | Status | Story Gap | Updated |
|--------|-------|--------|-----------|---------|
| EXP-001 | Three-Feature vs Full-Feature Baseline | completed | Open Gap #1 | 2026-09-02 |

---

## EXP-001 — Three-Feature vs Full-Feature Baseline

**Status:** completed

**Question:** 仅使用 duration、packet-ratio、byte-asymmetry 三特征的检测器，F1 与 full-feature Isolation Forest 基线差距多大？

**Motivation:** 直接检验 STORY Open Gap #1 与 Core Idea；结果决定 Story 走向。

**Method:**

- 3-feature：逻辑回归 on 三统计量 + per-flow z-score
- Baseline：Isolation Forest on 全流特征集（MOCK 配置见代码）

**Comparisons:** 3-feature LR vs full-feature IF

**Data / Setup:** CICIDS2017-MOCK 合成 CSV，train/test 80/20，seed 42

**Runs:**

- R1: seed 42, completed (CPU smoke)

**Code:** flow-detector

**Git:**

- Repository: `../story-research-code`
- Commit: `d785539e6937ffe0fb1c66c4861c14494eb8b8e7`
- Entry: `experiments/EXP-001/run.py` (config: `run_config.yaml`)

**Results:** `../story-research-code/results/EXP-001/metrics.json`

**Main Findings:**

- 3-feature LR F1 = 1.0000；full-feature IF F1 = 0.6154（MOCK 合成数据，n_test=120）。
- F1 差距约 -38.46 pp（3-feature 高于 IF 基线）。

**Interpretation:**

- 在 MOCK 合成流上，三统计量 + LR 完全可分攻击；IF 基线未匹配该 toy 分布，**不能**据此宣称真实 CICIDS 结论。
- Core Idea 在 toy 上未被否定，但 full-feature 强基线在此 MOCK 中偏弱，需更 faithful IF 或真实子集复验。

**Discovery Impact:**

- 正向：三特征在 toy 数据上携带强信号（见 DISCOVERY）。
- 负向：当前 IF 配置不足以作为“强基线”对照（见 DISCOVERY）。

**Story Impact:**

- Evidence：首个可复现内部实验完成；机制仍 plausible，但 Boundary 应强调 MOCK 合成局限。
- Open Gap #1 部分关闭（有数字，但仅 MOCK）；Gap #2/#3 仍开放。

**Review:**

- method-review: `.research/reviews/EXP-001/method-review.md`
- result-review: `.research/reviews/EXP-001/result-review.md`

**Next:**

- 改进 IF 基线或换 sklearn 对照后复跑（EXP-001b 或 EXP-002）。
- 检验归一化策略（Open Gap #2）。
