# Story: Three Flow Statistics May Suffice for Attack Detection

> **MOCK** — 模拟当前 Story。不含具体性能数字。

## Problem

入侵检测系统常依赖大量流级特征或深度模型，带来高计算成本与难部署问题。我们需要知道：在标准基准上，极简特征集是否仍能可靠区分攻击与正常流量。

## Key Observation

初步数据探索（MOCK 文献与先验）表明：许多攻击在**流持续时间、包数比率、字节不对称性**三个统计量上呈现与正常流量可分的分布，且不同攻击族对这些量的敏感度不同。

## Core Idea

用一个仅含上述三个流统计特征的轻量分类器（如逻辑回归或浅层树），配合简单的 per-flow 归一化，即可捕获多数常见攻击的主要信号；复杂特征主要带来边际收益。

## Evidence

- 文献支持：流统计在传统 IDS 中长期有效（见 LITERATURE — Sharafaldin 2018）
- 内部证据：**EXP-001** 在 MOCK 合成流上建立可复现管线；三特征模型在 toy 分布上表现极强，但对照 IF 基线在该 MOCK 中偏弱 — 真实数据集结论仍开放

## Boundary

- 仅验证**流级聚合**特征，不涉及 payload 内容
- **MOCK 合成数据**已跑通；真实 CICIDS2017 子集尚未验证
- 当前 IF toy 基线可能不足以代表“full-feature 强基线”
- 对低速率、长周期 APT 类攻击可能不足

## Open Gaps

1. **真实子集上** 3-feature 与 **有效** full-feature 强基线差距多少？（EXP-001 MOCK 仅部分回答）
2. 三个统计量的归一化策略是否影响结论？
3. 哪些攻击族主要依赖额外特征才能检出？

---

_Story 小改已应用 EXP-001 MOCK 证据；真实数据前勿宣称完成。_
