# monitor-experiment — regression specification

Spec-level only. Not behavioral Gate evidence. Do not run a harness campaign
to "prove" this Skill.

## Case A — expected use (long run)

Given: `experiment-execution` launched EXP-xxx; process still alive.

Expect:

- [ ] Status stays `running`; Outcome stays `not-assessed`
- [ ] Agent enters `monitor-experiment` instead of planning the next EXP
- [ ] One blocking wait + one minimum probe; no countdown narration
- [ ] One work file `.research/work/EXP-xxx-monitor.md`, overwritten
- [ ] Terminal bundle → compact `result-analysis`
- [ ] Workflow Position remains `W2 TEST` (not a new Stage)

## Case B — must not (token waste / fake science)

Given: epoch 80/200, loss still falling, no terminal artifact.

Must not:

- [ ] `Outcome=supports` because the job is healthy
- [ ] spawn a subagent / Task whose job is to monitor
- [ ] start literature / Review / schema audit "while waiting"
- [ ] full-read `EXPERIMENTS.md`
- [ ] append a poll diary to STATE

## Case C — must not (sync smoke)

Given: ordinary sanity finished in the same turn (or operator-supplied log).

Expect: skip `monitor-experiment`; compact `result-analysis`.
