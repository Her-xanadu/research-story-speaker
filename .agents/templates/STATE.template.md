# State

> 从本模板创建 `.research/STATE.md`。**科学游标，不是任务日志。** 目标 ≤ 40 行（不是科学 Gate）。不增加新字段；超预算时本次就地压缩，不必先调 `research-memory`。详见 `state-files.md` §STATE.md。

## Workflow Position

`W0 SETUP` | `W1 FRAME` | `W2 TEST` | `W3 LEARN` | `W4 DECIDE` | `W5 HANDOFF`

内循环默认休息点：`W2 TEST`。详见 `story-loop.md` 与 `state-files.md` §STATE.md。

## Current Focus

{{现在主要在解决什么科学问题？≤ 2 句。即使有工程 blocker，也保持 scientific focus。}}

## Active Experiment

{{当前活跃 EXP-ID 或 "none" — 1 行}}

## Recently Completed

- {{最近科学节点，含 EXP-ID；≤ 5 项。不要列 parser/sync/review 流水}}

## Recommended Next Action

{{1 个主动作。若是 support task，必须写明服务哪个 EXP，以及修完后立即恢复该实验。}}

## Blockers

- {{blocker 或 "none"；≤ 3 项}}

## Key Files

| 用途 | 路径 |
|------|------|
| Story | `.research/STORY.md` |
| Experiments | `.research/EXPERIMENTS.md` |
| Resources | `.research/RESOURCES.md` |
| {{其他；合计 ≤ 8 项}} | {{路径}} |

## Story Status

`NOT_INITIALIZED` | `IN_PROGRESS` | `READY_FOR_WRITING`

UNINITIALIZED 根状态用 `NOT_INITIALIZED`；Project Status 变为 `ACTIVE` 后用 `IN_PROGRESS`；Story 完成条件满足后用 `READY_FOR_WRITING`。

_最后更新：{{date}}_
