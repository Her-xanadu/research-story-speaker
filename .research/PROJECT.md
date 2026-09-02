# Project: Lightweight Flow-Feature Anomaly Detection

> **MOCK** — 仅供 V0.1 冷启动与闭环测试。替换真实项目时从 `.agents/templates/PROJECT.template.md` 重建。

## Research Goal

证明基于流级统计特征的轻量检测器，在标准入侵检测基准上可达到与深度包检测相近的检出能力，同时显著降低计算与部署成本。

## Primary Scientific Problem

网络异常检测是否必须在包级或流级深度特征上依赖复杂模型，还是少量可解释的流统计量已足够？

## Target Performance Direction

在 CICIDS2017 子集上：检出率接近强基线（如 Isolation Forest on full features），推理延迟与内存显著更低；机制可解释。

## Key Datasets

- **CICIDS2017**（子集）：主评估，含多种攻击类型
- **UNSW-NB15**（可选）：跨数据集泛化验证

## Evaluation Principles

- 固定 train/test 划分并记录 seed
- 报告 precision/recall/F1 与误报率，不单报 accuracy
- 每个正式结论至少一个对照基线
- 可复现：Git commit + 配置 + 结果路径必须可追溯

## Persistent Constraints

- 单机 CPU 优先；GPU 仅作可选加速
- 不使用未公开的内部数据
- V0.1 框架内不开展论文写作

## Story Completion Criteria

1. 达到预设 F1 目标（相对强基线差距 < 5%）且延迟降低 > 50%
2. 形成可发表的机制 Story（为何少特征仍有效）
3. 独立 Reviewer 批准核心结论与 EXP 链

## Notes

MOCK 项目模拟模式 B 布局：代码在并列目录 `../story-research-code`。
