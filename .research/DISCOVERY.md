# Discovery

> **MOCK** — 闭环试跑后由 result-analysis 填充。

## Current Scientific Understanding

网络入侵检测领域长期存在"特征工程 vs 端到端深度"的张力。MOCK 项目假设：对常见攻击，少量流统计量可能已承载大部分可分信号。EXP-001 在合成子集上提供了首个内部正向证据。

## Positive Discoveries

- 三特征在 MOCK 合成分布上可分，且强于本实验的 full-feature Isolation Forest 玩具基线。 — Evidence: EXP-001

## Negative Discoveries

- 当前 full-feature Isolation Forest 玩具实现不构成强对照，对照结论需更强基线复验。 — Evidence: EXP-001

## Null / Inconclusive Findings

_（待实验）_

## Invalidated Findings

_（待实验）_

## Open Contradictions

- 文献与先验暗示"更多特征通常有帮助"，但 EXP-001 的 full-feature Isolation Forest 玩具实现不构成强对照；对照结论需更强基线复验。 — Evidence: EXP-001

## Research Evolution

EXP-001 后 Core Idea 获得 MOCK 内部证据。

---

_普通 debug 不进 Discovery；只记录改变科学结论、失效旧结果、揭示设计/数据/机制问题的发现。_
