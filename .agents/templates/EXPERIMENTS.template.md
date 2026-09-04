# Experiments

> 从本模板创建 `.research/EXPERIMENTS.md`。**所有实验的单文件总账**。字段规范见 `.agents/references/experiment-record.md`。Status 值与 Outcome 值见 `experiment-record.md`，不要把定义表抄进本模板。

## Index

| EXP-ID | Title | Status | Outcome | Story Gap | Updated |
|--------|-------|--------|---------|-----------|---------|
| EXP-001 | {{短标题}} | {{planned\|running\|completed\|failed\|abandoned\|superseded}} | not-assessed | {{Open Gaps / Boundary 短语}} | {{YYYY-MM-DD}} |

---

## EXP-001 — {{EXPERIMENT_NAME}}

**Status:** {{planned|running|completed|failed|abandoned|superseded}}

**Outcome:** {{per experiment-record.md}}

**Question:** {{科学问题}}

**Motivation:** {{与 Story gap 的关系}}

**Method:** {{方法概要}}

**Comparisons:** {{对照}}

**Data / Setup:** {{数据与设置}}

**Runs:**

- R1: {{描述}}

**Code:** {{Codebase ID from RESOURCES}}

**Git:**

- Repository: {{path or remote}}
- Commit: {{hash}}
- Entry: {{experiments/EXP-001/run.py}}
- Config: {{experiments/EXP-001/run_config.yaml}}

**Results:** {{results location}}

**Main Findings:** {{客观结果}}

**Interpretation:** {{含义}}

**Discovery Impact:** {{→ DISCOVERY 要点}}

**Story Impact:** Level 0|1|2 — {{对 STORY 六段的影响}}

**Review:** {{none | pending | see REVIEWS.md EXP-xxx}}

**Next:** {{建议后续}}

---

<!-- 新实验在上方追加新 section，ID 递增：EXP-002, EXP-003, ... -->
