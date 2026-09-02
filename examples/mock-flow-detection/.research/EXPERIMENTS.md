# Experiments

> **MOCK** — 单文件总账。字段规范：`.agents/references/experiment-record.md`

## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-001 | Three-Feature vs Full-Feature Baseline | completed | supports | Open Gaps: 真实子集未验证 | 2026-09-02 |

---

## EXP-001 — Three-Feature vs Full-Feature Baseline

**Status:** completed

**Outcome:** supports

**Question:** 仅使用 duration、packet-ratio、byte-asymmetry 三特征的检测器，F1 与 full-feature Isolation Forest 基线差距多大？

**Motivation:** 直接检验 STORY Open Gap #1 与 Core Idea；结果决定 Story 走向。

**Method:**

- 3-feature：逻辑回归 on 三统计量 + per-flow z-score
- Baseline：Isolation Forest on 全流特征集（MOCK 配置见代码）

**Comparisons:** 3-feature LR vs full-feature IF

**Data / Setup:** CICIDS2017 子集（MOCK：合成 50 行 CSV），train/test 80/20，seed 42

**Runs:**

- R1: seed 42, completed 2026-09-02

**Code:** flow-detector

**Git:**

- Repository: `../../../story-research-code`
- Commit: `b0621e2ed266cc26020fac5b3295a588469bb495`
- Entry: `experiments/EXP-001/run.py`
- Config: `experiments/EXP-001/run_config.yaml`

**Results:** `../../../story-research-code/results/EXP-001/metrics.json`

**Main Findings:**

- 3-feature LR test F1 = **1.0000**；full-feature IF test F1 = **0.5455**
- F1 gap (IF − LR) = **−45.45** percentage points（三特征优于全特征 IF 基线）
- n_train=40, n_test=10；合成数据上三统计量足以完美分离攻击/正常

**Interpretation:** 在 MOCK 合成流上，额外特征未改善 IF 基线；三特征 + 简单 LR 已捕获全部可分信号。差距远小于 5% 阈值（方向相反：三特征更强）。

**Discovery Impact:** 正向发现 — 写入 DISCOVERY Positive Discoveries；弱 IF 对照 — 写入 Negative Discoveries

**Story Impact:** MOCK 层有证据、真实层仍开放

**Review:** done — `.research/reviews/EXP-001/` (`method-review-r2.md`, `result-review-r2.md`; r1 raw recovered)

**Next:** 真实 CICIDS 子集复跑 EXP-001 协议（Open Gap #1）；攻击族 ablation（EXP-002 候选）

---
