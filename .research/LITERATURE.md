# Literature

> **MOCK** — 已进入科研认知的文献摘要，非完整书目库。

---

## CICIDS2017 — Sharafaldin et al. 2018

**Reference:** Sharafaldin, I., Lashkari, A. H., & Ghorbani, A. (2018). Toward generating a new intrusion detection dataset and intrusion traffic characterization. ICISSp.

**Research Problem:** 缺乏含现代攻击类型的公开 IDS 基准与一致特征提取流程。

**Core Method:** 提出 CICFlowMeter 流特征与 CICIDS2017 数据集构建流程。

**Important Finding:** 流级统计特征可区分多种攻击；特征维度高但存在冗余可能。

**Relation to Our Story:** 支持 Key Observation（流统计含攻击信号）；Boundary 限于 CICIDS 特征定义。

**Relation to Experiments:** EXP-001 使用其子集与特征子集对照。

**Possible Inspiration:** 从 CIC 特征集中选取可解释的三元组做 ablation。

**Source:** public dataset documentation / ICISSp paper

---

## Isolation Forest — Liu et al. 2008

**Reference:** Liu, F. T., Ting, K. M., & Zhou, Z. H. (2008). Isolation forest. ICDM.

**Research Problem:** 高维异常检测需高效无监督方法。

**Core Method:** 随机隔离异常点，线性级复杂度。

**Important Finding:** 在高维特征上仍可用，适合作为 full-feature 强基线。

**Relation to Our Story:** EXP-001 的 full-feature 对照；若 3-feature 接近 IF，Core Idea 获支持。

**Relation to Experiments:** EXP-001 Comparisons

**Possible Inspiration:** 报告 IF 训练时间与 3-feature LR 对比以支撑部署叙事。

**Source:** ICDM 2008

---
