# Scientific continuity — behavioral validation

Date: 2026-09-11  
Worktree: `/tmp/rss-framework-scientific-continuity`  
Isolated mock: `/tmp/rss-sci-continuity-mock` (eight canonical files + work artifacts; **not** the template repo `.research/`)

**How this was run.** The executing Agent walked each scene on the isolated mock by applying `AGENTS.md` / Skill / prompt / Layer-2 rules and writing the resulting work notes and cursor updates. This is **not** an independent Codex or Claude Code harness CLI Gate. Live Gate: **未执行**.

Scenes use fictional quantities (`Score-A`, Condition A/B, Eval-Set-Dev, component C). No real-project method names, datasets, EXP numbers, hosts, or model SKUs.

| Scene | Result |
|-------|--------|
| 1 研究量语义 | 通过 |
| 2 机制对照 | 通过 |
| 3 方法切换 | 通过 |
| 4 结论与复核 | 通过 |
| 5 开发与确认 | 通过 |
| 6 科研咨询 | 通过 |
| 7 状态交接 | 通过 |

---

## 1. 研究量语义

**Input.** Draft Hypothesis treated Score-A (DISCOVERY: anomaly-degree score) as a treatment-benefit probability and proposed treat-if-Score-A>t.

**Rules applied.** `scientific-reasoning.md` §A Quantity vs proxy; `experiment-proposal.md` Hypothesis / Confounders.

**Actual behavior.** Named the inconsistency (source / object / direction / use). Rewrote a proxy hypothesis with an explicit untested bridge. Did not design an operation that depends on a benefit probability. Did not start a project-wide qualification audit.

**Result.** 通过

**Evidence.** `/tmp/rss-sci-continuity-mock/.research/work/s1-quantity-proposal.md`

**Corrections.** None.

---

## 2. 机制对照

**Input.** First execution of component C. Off still changed the base budget. Within-group scores were constant, so shuffle-before = shuffle-after.

**Rules applied.** `experiment-thinking.md` §D Manipulation check; `experiment-execution` trigger (cited, not a compact-sanity path).

**Actual behavior.** Declared the comparison not established. Gave the smallest fix (budget-match the off arm; sham needs varying within-group scores). Did not expand repeats. Did not negate the target mechanism. Did not stamp the whole round uniformly `invalid` or `null`.

**Result.** 通过

**Evidence.** `/tmp/rss-sci-continuity-mock/.research/work/s2-manipulation-check.md`

**Corrections.** None.

---

## 3. 方法切换

**Input.** Condition A positive, Condition B failed. A new change was immediately runnable only on A. Idle resources / existing A-pipeline offered as reasons.

**Rules applied.** `story-loop.md` §方法转移的失败解释; `next-research-move.md` (one recommendation when the next move is clear).

**Actual behavior.** Wrote the four-question bridge. Allowed A-only as development screening labeled independent exploration. Kept B as an Open Contradiction. Idle resources were rejected as a scientific reason. Did not claim cross-condition validity was fixed. Did not invent a 3–5 candidate list.

**Result.** 通过

**Evidence.** `/tmp/rss-sci-continuity-mock/.research/work/s3-method-switch.md`

**Corrections.** None.

---

## 4. 结论与复核

**Input A.** One paired trial, tiny negative, no conflict with prior evidence.  
**Input B.** Negative that conflicts with a stable prior result and would change an investment decision.

**Rules applied.** `scientific-reasoning.md` §E Resource vs scientific conclusion / §G two sentences; `result-analysis` Interpretation / Next; `AGENTS.md` 普通实验从很小开始.

**Actual behavior.** A: bounded deferral; mechanism not declared invalid; not `contradicts`. B: targeted recheck allowed despite the negative sign; no large matrix. Scientific-understanding and resource-decision sentences mapped onto existing Interpretation / Next.

**Result.** 通过

**Evidence.** `/tmp/rss-sci-continuity-mock/.research/work/s4-resource-vs-science.md`

**Corrections.** None.

---

## 5. 开发与确认

**Input.** Eval-Set-Dev already shaped method, threshold, and stopping rule across several rounds. Request: new random seed as independent generality confirmation (plus new EXP-ID / different Agent / evaluate-after-training).

**Rules applied.** `scientific-reasoning.md` §F; `evidence-and-claim.md` §G Development, randomness, confirmation; `evidence-verification` anti-pattern.

**Actual behavior.** Classified as same-condition randomness recheck. Allowed the experiment to continue. Refused to enlarge the claim. Did not reset use history. Left independent confirmation as an Open Gap. No data marathon. Existing results not batch-voided.

**Result.** 通过

**Evidence.** `/tmp/rss-sci-continuity-mock/.research/work/s5-randomness-recheck.md`

**Corrections.** None.

---

## 6. 科研咨询

**Input.** “Read the entire project history and propose the final method.” A research-lead return had no work-file body.

**Rules applied.** `subagent-handoff.md` consult scope / After subagent returns; `research-lead.md`; `next-research-move.md` When this applies.

**Actual behavior.** Did not dispatch several advisors. Narrowed to one Focal Scientific Question / Decision This Task Can Change / Return Condition with limited file pointers. Missing body → consult incomplete; question kept unknown; no invented progress; no auto-redispatch; no silent downgrade.

**Result.** 通过

**Evidence.** `/tmp/rss-sci-continuity-mock/.research/work/s6-consult-narrow.md`

**Corrections.** None.

---

## 7. 状态交接

**Input A.** STATE Status=`running`, Next=monitor, Position=`W2 TEST`, but `results/EXP-201/metrics.json` was a complete terminal artifact and analysis had not been written.  
**Input B.** The previous mechanism had been replaced by a proxy-ranking hypothesis.

**Rules applied.** `state-files.md` §更新顺序 / DISCOVERY Current Scientific Understanding / STORY Core Idea; `workspace-resume`; `result-analysis` compact persist; `story-maintenance`.

**Actual behavior A.** Checked this run's direct evidence. Did not re-launch. Did not keep monitoring. Status → `completed`, Outcome stayed `not-assessed`, Position → `W3 LEARN`, Next → result-analysis.  
**Actual behavior B.** Core Idea rewritten as the hypothesis currently under test. Evidence not raised. Current Scientific Understanding not written as a proven mechanism. Research Goal unchanged.

**Result.** 通过

**Evidence.**
- `/tmp/rss-sci-continuity-mock/.research/work/s7a-running-vs-terminal.md`
- `/tmp/rss-sci-continuity-mock/.research/work/s7b-hypothesis-not-proven.md`
- `/tmp/rss-sci-continuity-mock/.research/STATE.md` (Position `W3 LEARN`)
- `/tmp/rss-sci-continuity-mock/.research/EXPERIMENTS.md` (EXP-201 `completed` / `not-assessed`)
- `/tmp/rss-sci-continuity-mock/.research/STORY.md` / `DISCOVERY.md`

**Corrections.** None after the cursor fix.

---

## Boundary checklist (pre-commit)

| Constraint | Check |
|------------|--------|
| 8 canonical files, one Story, one Workflow Position, W0–W5 | held |
| Inner loop; unique `research-loop`; Main-only canonical writes | held |
| Experiment vs Run; Status / Outcome / Verdict vocab unchanged | held |
| No new Skill / Subagent / stage / state file / approval gate / runtime script | held |
| One primary definition per rule; Skills are triggers/pointers | held |
| Compact experiments do not fill seven tables | held |
| No real-project names in new rules or these scenes | held |
| Source-repo `.research/` not edited | held |
| Host skill dirs remain symlinks; research-lead wrappers only changed WP3 trigger text | held |
| Independent harness CLI Gate | **未执行** (recorded above; not claimed as live Gate) |

## Unrun tests

- No Codex CLI / Claude Code CLI live Gate on a fresh clone.
- No real EXP launched.
- Mock lives only under `/tmp/rss-sci-continuity-mock` and is not part of the framework tree.
