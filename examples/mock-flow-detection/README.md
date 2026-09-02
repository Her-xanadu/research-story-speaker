# MOCK example: Lightweight Flow-Feature Anomaly Detection

This directory is a **complete MOCK research instance**, not the live project.

Root `.research/` of this repository starts **UNINITIALIZED**. Copy files from here only when you want to replay the V0.1 closed loop as an example.

## Layout

```text
examples/mock-flow-detection/
├── README.md
└── .research/          # PROJECT / STORY / STATE / eight files + EXP-001 reviews
```

Companion toy code (not inside this repo):

```text
../../../story-research-code
```

On the original host that is `/Users/herxanadu/Documents/story-research-code`. Recovery source is `local-only` / `host-dependent`.

## What this MOCK closed

- Story: three flow statistics may suffice for attack detection.
- EXP-001: 3-feature LR vs toy Isolation Forest on synthetic flows.
- Method and result reviews as `method-review-r1.md` (raw Codex) and `*-r2.md` (synthesis).

## How to use as Test A

1. Do **not** overwrite a live project's `.research/`.
2. Open this example's `.research/` files, or copy them into a throwaway clone.
3. Cold-start: `AGENTS.md` → `PROJECT.md` → `STORY.md` → `STATE.md`.
4. Trace EXP-001 via `EXPERIMENTS.md` → code commit `b0621e2` → `results/EXP-001/metrics.json` → reviews.
