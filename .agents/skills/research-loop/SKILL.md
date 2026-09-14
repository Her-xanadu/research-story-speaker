---
name: research-loop
description: >-
  Top-level Story-driven research orchestrator. Use when continuing autonomous
  research, deciding the next scientific move, routing Literature vs Experiment
  vs Review, W1 FRAME reframing, or W4 DECIDE when next step is unclear. Skip
  when STATE is W2 TEST with a named ordinary EXP (inner loop), unless new
  evidence invalidates that Next's premises, or Next invents a module after
  an isolating miss (knowledge gap → W1 literature). Triggers include
  下一步研究什么, 继续科研循环, run research loop, what should we do next.
---

# Research Loop

Highest-level orchestrator. **Schedules only** — delegate literature, execution,
and review to matching Skills or Subagents.

Two-layer Workflow: outer `W1` reframes Story/mechanism/route; inner
`W2→W3→W4→W2` runs same-Story experiments. Full rules:
[story-loop.md](../../references/story-loop.md).

References: `story-loop.md`, `state-files.md`, `experiment-record.md`
(open at delegation time, not as a boot set).
Route-time operators (not a boot set): `scientific-reasoning.md`.
Selective gates: `idea-evaluation`, `evidence-verification`.

## When to use

- Autonomous research should advance one iteration, or `workspace-resume` left
  the route open.
- User asks next step, gap closure, or to keep going; new evidence arrived.
- **W1 FRAME** or **W4 DECIDE** when next step, Story route, or Level 2 change
  is unclear.
- Isolating miss still fails and the next change would invent a module
  without literature grounding (narrow knowledge-gap FRAME).
- Parallel Literature + Experiment + Review fits one gap.

Not for cold start (`workspace-resume`), compaction (`research-memory`), or
**inner loop** when STATE is `W2 TEST` and Recommended Next Action already
names an ordinary sanity/exploratory EXP — unless new evidence invalidates
that Next's premises
([story-loop.md](../../references/story-loop.md) §方法转移的失败解释), or
that Next invents a new module / mechanism / loss after an isolating miss
(knowledge gap: stay here, route light literature; do not skip to compact
design) — otherwise use compact `experiment-design` /
`experiment-execution` / `monitor-experiment` (if still running) /
`result-analysis` instead.

## Goal

One iteration when FRAME or full DECIDE is needed:

```text
Read STATE Workflow Position + Story → Method Loop or W4 five questions
→ Literature / Experiment / Review → invoke Skill → update memory → set next Position
```

Single current Story in `STORY.md`. Prefer experiments that **change judgment**,
not parameter sweeps ([story-loop.md](../../references/story-loop.md)).
Principle (lens only, not a STATE field):

```text
only investigate uncertainty that changes a decision
```

## Default flow

### 0. Method First Routing

If **Workflow Position is `W2 TEST` / `W3 LEARN` / `W4 DECIDE`**, ask first:

```text
当前科学问题是否仍然清楚？
```

If yes: **do not** hunt a new route. Continue the current Method Loop
([story-loop.md](../../references/story-loop.md) §Method-First Inner Loop).
Return to `W1 FRAME` when the mechanism or problem itself needs
reconstruction, **or** when the next change would invent a module
without literature grounding for this failure condition (narrow
knowledge-gap consult, then back to W2).

W4 default exit is **`W2 TEST`**. The inner loop may continue for many
rounds. If the method did not work, apply method consequence (simplify /
delete / change of a **named existing** piece) and stay on **`W2 TEST`**
to test the changed method. Do **not** `keep` the failing method and mint
the same Question / same rival again. Do **not** treat “guess a new
module” as that method consequence.

Go **`W1 FRAME`** when you cannot name a deletion / isolation, the
problem/route itself needs reconstruction, **or** the next change would
be an invented module / mechanism / loss without unused Suggests /
closest-work in `LITERATURE.md` for **this failure condition**
([story-loop.md](../../references/story-loop.md) §停滞处理). That last
case is a **narrow** knowledge-gap FRAME: light `literature-research`
(local vault first), pick **one** successor or closest contrast, return
to `W2 TEST`. Do not re-run「最大 gap」and do not change the project
goal. If unused Suggests already name a successor, use them (**0** find).
If closest-work already implements the same mechanism axis, REVISE / PARK
— do not mint a cosmetic-difference EXP.

Answer `What does this result imply for the method?` before picking the
next Position.

Same-mechanism `W2 → W3 → W4 → W2` does **not** redo literature, and
`monitor-experiment` wait does **not** consult the vault. New literature
on W1 reframe, knowledge-gap after isolating miss, novelty threat, a
required new baseline, or explicit user freshness. Deep literature stays
optional expensive.

### 1. Read Position + Story

Read `STATE.md` **Workflow Position** first, then `STORY.md` (六段见
`story-maintenance`). Unreadable or contradictory → `research-memory` first.

### 2. Inner-loop bypass (critical)

If **Workflow Position is `W2 TEST`** and **Recommended Next Action** already
names a concrete ordinary sanity/exploratory EXP (or continues the current
mechanism-isolation line):

- **Usually stop this Skill.** Do not re-run「最大 gap」or full W1 FRAME.
- Route compact `experiment-design` → `experiment-execution` →
  (`monitor-experiment` while Status=`running`) →
  `result-analysis` per [AGENTS.md](../../../AGENTS.md).
  Do not keep `research-loop` thinking during a live run.
- After `result-analysis`, let compact W4 set next Position (usually stay
  `W2 TEST`).
- **Exception:** if new evidence invalidates that Next's premises, **or**
  this Next is `keep` plus the same failing contrast, this Skill may stay
  long enough to rewrite Next as an EXP that tests the **changed** method
  (still `W2 TEST`). If this Next invents a new module / mechanism / loss
  after an isolating miss, do **not** execute it: escalate to **`W1 FRAME`**
  for a narrow literature consult (or pick one unused Suggests successor
  already on disk). Escalate to a full reframe only if no deletion /
  isolation can be named **and** no failure condition can be written for
  consult. A numbered Next is not a permanent ban
  ([story-loop.md](../../references/story-loop.md) §方法转移的失败解释,
  §停滞处理).

If Position is **`W3 LEARN`** → delegate `result-analysis` only, then stop.

### 3. Anti-duplication check

Judge per [story-loop.md](../../references/story-loop.md) §反重复. This Skill
only routes.

### 4. FRAME: largest Story gap (W1)

When Position is `W1 FRAME`, `W4 DECIDE` with unclear next step **and no
named deletion / isolation**, knowledge-gap (invented module after
isolating miss), or A/B/C reframe → W1 per story-loop. A no-yield round
that already has a named deletion / isolation stays `W2 TEST`. A
knowledge-gap W1 is **not** 「最大 gap」: consult for **this failure
condition** only, then return to W2. Judge per §Gap 优先级 only on true
reframe. One focal gap per iteration unless parallel subagents warranted.
**Do not** rewrite `PROJECT.md` Research Goal.

### 5. Internal route stage

Pick **one** internally. Do **not** write these names into `STATE.md`.

```text
scout   — is there a signal worth continuing?
focus   — which mechanism actually produces the effect?
confirm — can results independently reproduce and support Story?
```

Use only to choose the next Skill under W1 FRAME. Not Protocol, Outcome,
Verdict, or Workflow Position.

### 6. Choose route

Selective — **not** a default chain. Ordinary exploratory EXP stays light.

| Gap nature | Route | Delegate |
| --- | --- | --- |
| Prior work, novelty, lit conflict, or isolating-miss knowledge gap | Literature | `literature-research` / scout — **not** every EXP; W1 / novelty / required new baseline / user freshness / isolating miss + ungrounded new module. Light default. Unused Suggests on disk → pick **one**, **0** find |
| Untested mechanism, empirical answer | Experiment | `experiment-design` → `experiment-execution` → (`monitor-experiment` if still running) → `result-analysis` |
| Major new idea (Core Idea, route competition, mechanism replacement, expensive successor) | Idea-gate | `idea-evaluation` ([prompt](../../prompts/idea-evaluation.md)); then `experiment-design` only if the gate says continue |
| High-stakes evidence (Story Evidence candidate, surprising strong result, Story-core change) | Evidence then Review | `evidence-verification` ([prompt](../../prompts/evidence-verification.md)) after `result-analysis` → `experiment-review` |
| High-stakes method, anomaly, big Story change | Review | `experiment-review` / reviewer |
| Wording only | Story | `story-maintenance` |

Ordinary exploratory EXP: `experiment-design` → `experiment-execution` →
(`monitor-experiment` if still running) →
`result-analysis` **without** `idea-evaluation`, `evidence-verification`,
`experiment-review` / reviewer, or `result-analyst` by default.

Parallel Experiment work: use `experiment-agent` / `result-analyst`; handoff via
[subagent-handoff.md](../../prompts/subagent-handoff.md). One **focal scientific
question**; multiple EXPs serving it are allowed. No unbounded multi-route
parallelism after W2 focus. Support may parallel but must not become the main axis.

### 7. Invoke and integrate

- Simple: run Skill in context (workhorse unless `AGENTS.md` §模型分档 says
  strongest). Parallel/heavy: named Subagent per `AGENTS.md` — not generic
  worker/explore.
- Independent next-step judgment: dispatch `research-lead` (**strongest**)
  when Position is not `W2 TEST` with a named next EXP, **or** when new
  evidence invalidates that Next's premises
  ([story-loop.md](../../references/story-loop.md) §方法转移的失败解释).
  Optional: [next-research-move.md](../../prompts/next-research-move.md).
- After evidence, follow [state-files.md](../../references/state-files.md) §更新顺序.

### 8. W4 DECIDE + next Position

When this Skill owns the shift (Level 2, A/B/C back-to-W1, completion, or
unclear next step), answer story-loop §W4 五问:

```text
1. 结果可靠？
2. 改变对 Story 的相信？
3. 下一步同一科学问题？
4. What does this result imply for the method?
   keep / simplify / delete component / change mechanism / change control / abandon
5. 下一阶段？（默认 W2 TEST）
```

Write **one** next Workflow Position in STATE:

| Judgment | Position |
| --- | --- |
| Story stable, next EXP clear (including: method changed, now test it) | `W2 TEST` |
| Result unclear | `W3 LEARN` |
| Failing method + `keep` same contrast | do not mint; rewrite Next, stay `W2 TEST` |
| Isolating miss + next change is an ungrounded new module | `W1 FRAME` (narrow light literature → one successor → `W2 TEST`) |
| Core Idea / gap / route needs reframe (A/B/C); cannot name a deletion / isolation or a failure condition to consult | `W1 FRAME` |
| Story complete per PROJECT | `W5 HANDOFF` |

Do **not** force `W1→W2→W3→W4→W1` every Experiment.

Level 0/1 with clear Next may be handled by `result-analysis` / `story-maintenance`
without loading this Skill.

### 9. Continue, stagnate, or stop

Stop on user blocker, Story completion (`W5 HANDOFF`), or Reviewer
`ATTENTION_REQUIRED` ([reviewer.md](../../subagents/reviewer.md)).

Stagnation is **many rounds with no yield** (method still failing and
still `keep`), not “too many rounds.” Apply Method Complexity Rule and
**continue `W2 TEST`** on a named deletion / isolation / simpler
explanation ([story-loop.md](../../references/story-loop.md) §停滞处理).
Do **not** register the same failing contrast again. Do **not** invent a
new module in lieu of consult. Knowledge gap after isolating miss →
narrow `W1 FRAME` + light literature, then back to W2. Full `W1 FRAME`
reframe only if no deletion / isolation can be named and no failure
condition can be written for consult.

## Reads

**Minimum:** `STATE.md` (Workflow Position), `STORY.md`.

**As needed:** `PROJECT.md`, targeted `DISCOVERY.md` / `EXPERIMENTS.md`
sections (EXP-ID or keyword lookup — **do not** load the entire ledger),
`LITERATURE.md`, `REVIEWS.md`, `RESOURCES.md`. Load delegated Skills at
delegation time. Expand history only on true `W1 FRAME`.

## Updates

Ensure executors updated per [state-files.md](../../references/state-files.md)
§更新顺序. May set STATE Workflow Position and next focus when owning W4.

## Deviation allowed

- User-specified Skill → re-enter at integrate.
- Parallel scout + experiment + reviewer for one gap.
- Defer Literature when a cheap named deletion / isolation still exists.
  Do **not** defer after an isolating miss when the next change would be
  an invented module. Do **not** consult during `monitor-experiment` wait.
- Skip iteration after Level 0 with no Story change.
- Pause for `research-memory` when routing blocked.
- Skip Idea-gate / Evidence-gate / Reviewer for ordinary exploratory EXP.

Do **not** use a fixed per-EXP state machine, hard-code EXP IDs, write
`scout`/`focus`/`confirm` into STATE, re-frame on every inner-loop pass,
or force every EXP through idea-eval → evidence → reviewer.
