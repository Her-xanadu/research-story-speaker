# MOCK Test Scenarios (T3 — Tests F, G, J)

这是 instruction-only 场景说明，不构成 Test F/G/J 的执行证据。

> Instruction-only fixtures for maintenance verification. Not real experiments.

## Test F — Negative Experiment (pattern)

When an experiment fails:

1. Keep full EXP section in `EXPERIMENTS.md` with Main Findings = failure facts
2. Add **Negative Discovery** in `DISCOVERY.md` with `Evidence: EXP-xxx`
3. Shrink STORY Boundary or Open Gaps — do **not** delete the experiment section
4. Update STATE with blocker or pivot

Example narrative: EXP-002 ablation shows one attack family needs extra features → negative discovery, Story Boundary expanded.

## Test G — Story Evolution (pattern)

When Core Idea is overturned:

1. Record invalidating experiment in EXPERIMENTS (permanent section)
2. Add **Invalidated Findings** + **Research Evolution** in DISCOVERY
3. Call `experiment-review` before changing Problem / Key Observation / Core Idea
4. Rewrite STORY six sections; STATE notes new Open Gaps
5. Continue loop with new experiments

## Test J — Long History (pattern)

With many EXP sections:

1. New agent uses `workspace-resume` → PROJECT + STORY + STATE first
2. `research-memory` compacts old EXP summaries in EXPERIMENTS (shorter, not deleted)
3. DISCOVERY holds synthesized understanding; agent reads recent/relevant EXP only
4. STATE links to active EXP and key files

Do not require full linear read of 100+ experiment sections on every cold start.
