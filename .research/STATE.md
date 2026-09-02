# State

> **MOCK** — 冷启动应读本文件了解"现在做到哪里"。

## Current Focus

EXP-001 已完成；评估是否在更大或真实 CICIDS 子集上复验 Core Idea，并规划攻击族 ablation（EXP-002）。

## Active Experiment

none — EXP-001 已 closed（MOCK tier）

## Recently Completed

- **EXP-001** 执行与 metrics 产出（3-feature F1=1.0 vs IF F1=0.5455）
- Git 记录 commit `b0621e2ed266cc26020fac5b3295a588469bb495` 于 `story-research-code`
- method-review 与 result-review 完成
- DISCOVERY、STORY Evidence、EXPERIMENTS 总账已同步

## Recommended Next Action

1. 设计 EXP-002：攻击族 ablation（哪些攻击依赖额外特征）
2. 可选：将 `mock_flows.csv` 扩展或替换为真实 CICIDS 子集，复跑 EXP-001 协议
3. 若复验仍支持 Core Idea，将 Story Status 从 `IN_PROGRESS` 推进为 `SUPPORTED_MOCK`

## Blockers

- none（MOCK 阶段）

## Key Files

| 用途 | 路径 |
|------|------|
| Story | `.research/STORY.md` |
| Experiments | `.research/EXPERIMENTS.md` |
| Discovery | `.research/DISCOVERY.md` |
| Code resources | `.research/RESOURCES.md` |
| EXP-001 reviews | `.research/reviews/EXP-001/` |
| Git linking | `.agents/references/git-linking.md` |

## Story Status

`IN_PROGRESS` — MOCK 层有正向证据，待复验

_最后更新：EXP-001 闭环（Worker F, 2025-09-02）_
