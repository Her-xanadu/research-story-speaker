---
name: monitor-experiment
description: >-
  After a run is launched, wait silently and probe progress with a minimum
  status check.   Use when EXP-xxx Status is running, the agent just launched a
  job, or the user asks 监控 / 还在跑吗 / 进度 / is it done / check results while
  the process is still alive. Main MUST run `sleep N; <probe>` in this
  conversation (training default N=300). Do not interpret science, do not
  keep thinking during the wait, do not end the turn after 1–2 minutes, and
  do not dispatch a subagent just to monitor.
---

# Monitor Experiment

Thin Skill for **token-cheap waiting** after `experiment-execution` has
launched a job. **Default owner: Main Agent.** Not a reason to open a
subagent. Not scientific interpretation. Not a new Workflow Stage:
Position stays `W2 TEST`.

Localized from ARIS `monitor-experiment` (wait/probe operator only). No ARIS
ledger, orchestrator, claim gate, Feishu, or `.aris/` control plane.

## When to use

- `experiment-execution` just launched a run that is still alive, queued, or
  remote — **Main continues into this Skill**.
- `EXPERIMENTS.md` Status is `running` and there is **no** terminal evidence
  yet.
- User asks whether a launched EXP is done, or to monitor progress.
- Cold start / `workspace-resume` finds Active EXP `running` without a
  finished result artifact.

Do **not** use for:

- designing (`experiment-design`) or implementing/launching (`experiment-execution`)
- interpreting Outcomes (`result-analysis`)
- independent Review (`experiment-review`)
- operator-supplied logs / pre-existing result files (already terminal —
  compact `result-analysis`)
- synchronous smoke that **finished in this turn** (skip this Skill)
- filling the wait with literature, audits, extra EXPs, or planning
- dispatching a subagent / Task / `experiment-agent` **in order to monitor**

## Goal

```text
job launched
→ silent wait
→ one minimum probe
→ still running? wait again
→ terminal? result-analysis
→ failed/suspect? experiment-execution bounded debug / support
```

**Running successfully is not scientific success.** Epoch/loss progress,
`exit 0` later, or a finite metric only become artifacts after
`result-analysis`. This Skill never assigns `Outcome`.

## Classify (every probe)

| Class | Meaning |
|-------|---------|
| `running_no_new_terminal_evidence` | Process/queue/host still active; no agreed terminal bundle |
| `terminal_unprocessed` | Terminal evidence exists and has not been handed to `result-analysis` |
| `failed_or_suspect` | Nonzero exit, crash, NaN/OOM, missing required outputs, path violation, or process gone without a result |

File existence alone is **not** terminal. A sentinel `EXIT_CODE` (for example
`255`) while the process is alive is still `running`. Require process exit /
queue completion **and** the declared result artifact (or an explicit crash
log) to agree.

## Compact default (this whole Skill)

Do **not** open `failure-diagnosis.md`, `experiment-record.md`, or
`result-diagnosis.md` while `running_no_new_terminal_evidence`.

Required reads: `.research/EXPERIMENTS.md` (**target EXP section / index
lookup only**), `.research/RESOURCES.md` Compute + the Codebase used by that
EXP, `.research/work/<EXP-ID>-monitor.md` if it already exists.

Do **not** cat entire `EXPERIMENTS.md`. Do **not** scan `.research/work/`.
Do **not** update `DISCOVERY.md` or `STORY.md`.

## Default flow

1. **Identify the job** from the EXP Runs field: host, pid/screen/tmux/job id,
   log path, results root, probe command if recorded. Compute **Access** from
   `RESOURCES.md` (local shell or `SSH <alias>`). Do not invent a backend.
2. **One minimum probe** (see below). Classify.
3. If `running_no_new_terminal_evidence`: overwrite the monitor work file,
   emit **at most one** progress line when something useful changed, then run
   **one** shell line `sleep ${sleep_seconds}; <probe>` and wait for it.
   Repeat in this conversation until terminal. Do not end the turn.
4. If `terminal_unprocessed`: stop waiting. Continue with compact
   `result-analysis` (Main; same turn). Do not interpret here.
5. If `failed_or_suspect`: stop waiting. Return to `experiment-execution`
   compact support or bounded debug on the **same** EXP. No new EXP-ID.

## Minimum probe

Use the backend recorded at launch. Typical checks (pick what exists; do not
run all):

- process / screen / tmux / queue still listed
- at most 50 lines of recent log
- `progress.jsonl` / epoch line / wrapper ETA if the runner writes one
- result-file count or hash (existence ≠ done)
- `run_state.json` / `EXIT_CODE` only as evidence, not as a claim

Do **not** read sealed predictions, full metrics JSON, or W&B history in
order to start science. Training curves are progress signals only.

Do **not** SSH to a host that was not used. Do not assume `screen` when the
Runs field says a different launcher.

## Adaptive wait

**Real training / remote GPU / ETA > 10 min** (default):

`5m → 10m → 15m(cap)` = `300 → 600 → 900` seconds. First wait is **5 minutes**,
not 1–2 minutes.

**Smoke / crash hunt only:**

`1m → 2m → 3m → 5m → 10m → 15m(cap)` = `60 → 120 → 180 → 300 → 600 → 900`.

- Known ETA: `sleep_seconds = max(floor, min(profile_seconds, eta_seconds + 30))`
  where floor is 300 for real training and 60 for smoke
- Unknown ETA: `sleep_seconds = profile_seconds`
- Never sleep `< 60s` unless the user asks for an immediate check, or an
  error/terminal artifact just appeared
- No-change probes **advance** the profile; do not reset to 1m because an
  epoch incremented
- Reset the interval to the profile start (5m for real training, 1m for
  smoke) only on: terminal evidence, new completed result artifact,
  error/NaN/OOM, user "check now", or ETA `< ~2m`

## Token-saving wait contract (the point of this Skill)

**Required Main command** (this conversation, one shell line):

```bash
sleep ${sleep_seconds}; <one minimum probe>
```

Example: `sleep 300; tail -n 50 /path/to/log`

Do **not** sleep in the model. Do **not** end the turn after one interval.
When the command returns, probe is already done (it is in the same line).
Classify; if still running, issue the next `sleep N; probe` immediately.

After each probe, the **only** normal foreground action is that one blocking
`sleep; probe`. The model must not think in between.

Forbidden:

- 1–2 minute think/report/stop cycles (short Codex turns)
- narrating "still 8 minutes" / "almost there" / countdown
- polling a sleeping command every 30s
- spawning a subagent, Task, or extra Agent to sit on the wait
- opening literature, Review, `research-loop`, extra EXPs, or audits to
  "use the wait"
- rewriting STATE, EXPERIMENTS narrative, or a fresh report each poll
- reloading a huge Parent context just to keep the conversation warm
- `sleep` without a probe on the same command, then stopping to wait for the user

Allowed:

- one timestamped progress line when progress **changed** (epoch, ETA, new
  error). Example: `04:07 EXP-704 119/200; ETA ~04:28; sleep=300; next=04:12`
- overwrite the same monitor work file
- if the host caps one wait shorter than `sleep_seconds`, chain the **fewest**
  silent waits using the maximum supported duration; no reasoning between them
- if keeping the foreground wait would reload a large context more than once
  before `next_poll_at`: persist `next_poll_at` in the work file, yield, and
  resume the **same** task at that time. Mechanical liveness is not a reason
  to keep the model active

Codex / any shell host: the wait **is** `sleep ${sleep_seconds}; <probe>` in
one command. Cursor may use an equivalent blocking wait of the same duration,
then the same probe. Do not invent a second monitor agent.

## Monitor work file

Create **at most one** file per EXP:

```text
.research/work/<EXP-ID>-monitor.md
```

Overwrite in place. Do not append a poll log. Do not create a new file per
probe. Fields (keep short):

```text
EXP-ID:
class: running_no_new_terminal_evidence | terminal_unprocessed | failed_or_suspect
backend: <from RESOURCES / Runs>
probe: <one command>
poll_attempt:
sleep_seconds:
last_poll_at:
next_poll_at:
eta:
progress_signature:
artifact: <path or none>
```

Do **not** write this into `STATE.md` on every poll. STATE Next may already
say `monitor EXP-xxx until terminal`. Current Focus stays the **scientific
question**, not "waiting for GPU".

Main may set EXPERIMENTS Status=`running` once at launch (execution Skill).
Monitor does not bump Status on no-change probes.

## Writes

| File | When |
|------|------|
| `.research/work/<EXP-ID>-monitor.md` | Create once; overwrite on probes / class change |
| `.research/EXPERIMENTS.md` (target section) | Only on class change to terminal/failed: mechanical Runs note + artifact path. Status `running`→ keep until execution/analysis records completion. Outcome stays `not-assessed` |
| `.research/STATE.md` | Only if Next is missing a monitor pointer; one line. No poll diary |

Forbidden writes: `STORY.md`, `DISCOVERY.md`, `REVIEWS.md`, new EXP-ID,
Review packets, canonical Outcome.

## Handoff

| Class | Next |
|-------|------|
| still running | this Skill on **Main** (same conversation) |
| terminal bundle complete | compact `result-analysis` |
| engineering crash / hang / unusable metrics | `experiment-execution` support / bounded debug, **same EXP** |
| scientific-contract would change to "fix" it | `experiment-design` (not Reviewer by default) |

Do not open Review because a job is still running or because an epoch ended.

## Parallel jobs

Several Runs that test the **same** focal scientific question may be one
monitor target (one work file, one progress line). Unrelated method B / audit
/ dataset qualification started "while waiting" is Parallel Drift — stop it.
See [story-loop.md](../../references/story-loop.md).

## Boundaries

- Not a 9th canonical file. Not a Workflow Stage. Not `research-loop`.
- No framework runtime scripts. No ARIS `.aris/` writes.
- Do not copy upstream Feishu / W&B / Vast / Modal control planes. If
  `RESOURCES.md` already names an extra progress source, it may be a probe
  signal only.
- **Main Agent runs this Skill.** Do not open `experiment-agent`, a Task, or
  any monitor-only subagent. If a subagent launched the job, it returns
  launch facts (job id, probe, log path); Main waits.

## Deviation allowed

- User says "don't wait, tell me later": persist `next_poll_at`, stop the
  turn, resume from the work file next time.
- Probe command missing: reconstruct once from RESOURCES + Runs, then persist
  it. If still unknown, ask once; do not poll blindly.
- Very short remaining ETA (`< 60s`): one short wait is allowed.
