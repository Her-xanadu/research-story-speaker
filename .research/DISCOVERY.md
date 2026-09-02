# Discovery

> **MOCK** — 闭环试跑后由 result-analysis 填充。

## Current Scientific Understanding

网络入侵检测领域长期存在"特征工程 vs 端到端深度"的张力。MOCK 项目假设：对常见攻击，少量流统计量可能已承载大部分可分信号。EXP-001 在合成子集上提供了首个内部正向证据。

## Positive Discoveries

### 1. Three flow statistics suffice on MOCK subset (EXP-001)

- **Finding:** 仅 duration、packet_ratio、byte_asymmetry 三特征的逻辑回归在测试集 F1=1.0，优于六特征 Isolation Forest 基线（F1=0.5455）。
- **Evidence:** commit `b0621e2ed266cc26020fac5b3295a588469bb495`, `results/EXP-001/metrics.json`
- **Confidence:** MOCK-tier — 合成 50 行数据，样本极小，外推需谨慎
- **Story link:** 支持 Core Idea；Open Gap #1 在玩具数据上得到正向回答

## Negative Discoveries

_（待后续实验）_

## Null / Inconclusive Findings

_（待实验）_

## Invalidated Findings

_（待实验）_

## Open Contradictions

- 文献与先验暗示"更多特征通常有帮助"，但 EXP-001 MOCK IF 基线反而弱于三特征 LR — 可能源于玩具 IF 实现或小样本，非定论

## Research Evolution

- MOCK 初始化：从文献观察形成三特征 Core Idea，等待 EXP-001 验证
- EXP-001 完成：合成数据上三特征管线优于全特征 IF，Story 获首个内部证据

---

_Test F/G/J 可在 Phase 11 追加 MOCK 负结果与 Story 推翻条目。_
