# Discovery

> **MOCK** — 科学经验积累；重要条目标注 `Evidence: EXP-xxx`。

## Current Scientific Understanding

网络入侵检测中，流统计特征可携带攻击信号；MOCK EXP-001 表明**在合成 toy 分布上**三统计量足以线性分离攻击，但当前 toy IF 基线偏弱，真实 CICIDS 结论仍待验证。

## Positive Discoveries

- **三流统计量在 MOCK 合成数据上具有强可分性**（LR F1=1.0 on test split）。
  - Evidence: EXP-001
- **流统计基线思路与文献一致**（CIC 特征族含冗余）。
  - Evidence: LITERATURE → Sharafaldin 2018

## Negative Discoveries

- **当前 MOCK IF 实现/阈值未构成强对照**（F1=0.6154 vs LR 1.0）。
  - Evidence: EXP-001 — 需在复验前修订基线或换库。

## Null / Inconclusive Findings

- 真实 CICIDS2017 子集上 3-feature vs full-feature 差距：**未测试**（MOCK 仅合成 CSV）。

## Invalidated Findings

- （none）

## Open Contradictions

- Story 假设“接近 full-feature 强基线”；MOCK 结果反而 3-feature 大幅领先弱 IF — 矛盾来自基线有效性而非已证实“少特征足够”。

## Research Evolution

1. MOCK 初始化：三特征 Core Idea。
2. EXP-001 完成：toy 上强信号 + 弱 IF 对照 → 收紧 Boundary，计划基线复验。
