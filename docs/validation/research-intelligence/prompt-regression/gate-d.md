# Gate D — Behavioral prompt / Skill regression (dry-read)

- **Wave:** V0.2 Wave F fixtures × Wave D/E Skills+prompts
- **Method:** lightweight local dry-read of frozen fixtures + instruction text. No Codex/Claude CLI. No framework `.py` / `.sh`. No writes under live `.research/` (root `PROJECT.md` still `UNINITIALIZED`). Tags not moved.
- **Branch:** `v0.2-research-intelligence`
- **Candidate HEAD:** `25c8168481d11f14d14462b65828c3a4fa8a4d0c` (Wave F fixtures)
- **Baseline:** tag `v0.1.1` peeled commit `762deb4c9db896acb5c00066b8e6dc5a63732cfa` (read via `git show v0.1.1:path`)
- **Tag check (this run):** `v0.1.1^{commit}` still `762deb4…`; `v0.1^{commit}` still `8db3b301f5bce878d6c2ee4a61bcb234c9609c3d`
- **Regression rule:** `regression?` = candidate **worse than baseline** on **frozen core** (Protocol Status / Outcome / DISCOVERY buckets / Story-number ban) **or** on **protection** (Case 10 light chain). Missing V0.2 gates on v0.1.1 is **expected**, not a regression (`skill-evolution.md` §E; this file’s column is not “fixture PASS vs V0.2 expected”).

**Verdict: no case REGRESSED.**

Fixtures live only under `docs/validation/research-intelligence/cases/`. Embedded MOCK blocks were not copied into `.research/`.

---

## How this dry-read was done

1. Read each case README + `artifacts/` (canonical numbers/logs only; none invented).
2. Infer **baseline** routing from v0.1.1 Skills (10 scientific/workflow Skills; **no** `idea-evaluation` / **no** `evidence-verification`; **no** Layer-2 `research-intelligence/` owners; `research-loop` Experiment row is only `experiment-design → experiment-execution → result-analysis`).
3. Infer **candidate** routing from current Skills + task prompts + Layer 2 (cite, do not recopy Protocol tables).
4. Important cases **1, 3, 4, 7, 10**: second pass — re-read When-to-use / Decision logic / must-not-fire independently and confirm the same candidate routing.

Protocol owners unchanged vs `v0.1.1` (`git diff --stat v0.1.1 -- .agents/references/experiment-record.md .agents/references/state-files.md .agents/templates/DISCOVERY.template.md` = empty). Candidate **adds** judgment operators and two selective Skills; it does not fork Outcome / Verdict / Story six.

---

## Invariants (candidate, must-hold)

| Case | Invariant | Dry-read result |
| --- | --- | --- |
| 1 | ADVANCE must not fire; Recommended Action `REVISE` or `PARK` | **Hold.** Costume axes in `idea-and-mechanism.md` §C (renaming + fixed weight); deletion test §G fails; prompt Decision logic forbids ADVANCE. |
| 3 | Weak baseline → Story Evidence not upgraded | **Hold.** `result-diagnosis.md` §4–5 + `evidence-and-claim.md` §E mismatch; Evidence-gate writes work file only; `story-maintenance` must not mint a mechanism sentence from Full vs default IF. |
| 4 | `failed` + `not-assessed`; no Negative Discovery | **Hold.** Same frozen mapping as v0.1.1 `result-analysis` step 4; candidate `result-diagnosis.md` §8 / `failure-diagnosis.md` restate it. OOM log, no `metrics.json`. |
| 7 | `invalid` + Invalidated Findings; Story Evidence line removed | **Hold.** Candidate makes the v0.1.1 one-liner (`invalid` ≠ Negative) explicit: Invalidated Findings + drop Evidence (`evidence-and-claim.md` §G). |
| 10 PROTECTION | no auto `idea-evaluation` / `evidence-verification` / `result-analyst` / `reviewer` | **Hold.** Loop ordinary-exploratory row; both new Skills stop-and-write-nothing; `result-analyst` “普通探索不强制”; `reviewer` When-to-use is high-cost / core-method / Story-core. |

---

## Case 01 — Cosmetic Successor (important)

**Kind:** Idea-gate failure. **Load:** `cases/01-cosmetic-successor/` only. No run artifacts.

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Trigger: replace Core Idea `WES` with costume `AEA` (rename + freeze λ 0.5→0.3) and, if it “looks good”, ADVANCE a 50-seed grid. MOCK STORY Core Idea = WES; STATE asks to idea-gate before any large grid. | Fire `idea-evaluation` + `idea-evaluation.md`. Work file only: `.research/work/idea-evaluation-aea.md`. Mechanism Distinction **weak** (§C renaming / fixed weight; §G deletion fails). **Recommended Action: `REVISE` or `PARK`** (cite §H; do not recopy glossary). **ADVANCE must not happen.** No EXP-ID, no Core Idea rewrite, no grid. Do not fire design/execution/result/evidence/review/deep-lit this pass. | **No Idea-gate Skill.** `research-loop` only has Literature / Experiment / Review / Story. User text “if it looks good, ADVANCE … large confirmation grid” maps to **Experiment**: `experiment-design` (likely mint EXP + 50-seed protocol) → execution. Closest-work already on disk (`MOCK-WES-2019`) but there is no §C/§G/§H operator. Costume can proceed as a “new method.” Missing gate = **expected**. Frozen core (Outcome/Verdict) unused (no result). | **Fire** `idea-evaluation` (`AGENTS.md` row “新 Core Idea / 换路线 / 高代价实验”; Skill When-to-use = mechanism replacement / expensive successor). Prompt: costume in §C → do not ADVANCE; Decision logic 4 → `REVISE` (fixable identity) or `ABANDON`; §H also allows `PARK` if the move is only a rename pending a real axis. Typical: **`REVISE` or `PARK`**. Write work file; **stop**; Main does **not** get an EXP-ID from this Skill. After ADVANCE-only would design fire — ADVANCE is blocked, so **no** `experiment-design` this pass. | **no** | Candidate is **stricter**, not worse. `ABANDON` is allowed by Decision logic 4 but not required by the fixture; not an ADVANCE miss. |

### Pass 2 (stability)

Re-read: Skill Default flow step 1 (trigger match — not routine seed); prompt lenses 3 and Decision logic 3–5; Skill Updates (work file only). **Same routing:** Idea-gate only → `REVISE`/`PARK` → stop. **Unstable?** no.

---

## Case 02 — Real Mechanism Successor (ordinary)

**Kind:** Idea-gate ADVANCE path. **Load:** `cases/02-real-mechanism-successor/`. No metrics (do not invent a completed EXP).

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Trigger: idea-evaluate **PRRW** (destination-port rarity rank vs previous K windows — **new information source** vs WES entropy). If identity real, ADVANCE and name a minimum decisive test including mechanism-off. RESOURCES: ≤4h single-split; 50-seed out of budget. | Fire `idea-evaluation` + prompt. Work file `.research/work/idea-evaluation-prrw.md`. Distinction **real** (§C information source; §G shuffle/delete rarity rank removes the flow). No §D fatal on fixture as written. **`Recommended Action: ADVANCE`**. Minimum test: Full PRRW vs mechanism-off vs WES, rare-burst/low-volume, one split — **no EXP-ID**. Then **stop**; Main *may* later `experiment-design`. | No Idea-gate. Loop → Experiment design. Baseline `experiment-design` has Question/Why/Comparison but **no** unit/controls/mechanism-off fields from `experiment-thinking.md` §D. Agent **may** skip isolating control and jump toward a confirmation grid (user asked ADVANCE). Missing gate = expected. | Idea-gate fires. Prompt lens 9 + §H: smallest comparison with mechanism-off / information-matched control. Identity clear + no fatal + test named → **ADVANCE**. Skill: after ADVANCE, Main may route to `experiment-design`; this Skill does not register EXP. | **no** | Failure mode to watch in a later harness (not this dry-read): PARK/REVISE *only* because the name is new, despite a real information-flow change — that would be a **V0.2 fixture miss**, still not “worse than a baseline that had no gate.” Instruction text does not encode that miss. |

---

## Case 03 — Weak Baseline Positive Result (important)

**Kind:** Result + Evidence-gate. **Artifacts:** `metrics.json` Full WES F1 **0.91** vs default IF **0.41** (`tuned: false`, features `raw_4d_counts_not_entropy`); `run.log` exit 0. Executor prose: “SOTA. Promote Core Idea.”

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Trigger: EXP-201 finished; “analyze and **update Story Evidence if Core Idea is now supported**.” Surprising strong result **and** main baseline comparison. Question as written is Full WES vs **this** IF. | **`result-analysis` + `result-diagnosis.md` must run.** Record 0.91 / 0.41 in EXPERIMENTS Main Findings; Status `completed`. Local Outcome for the *stated weak Question* may be `supports` (cite `experiment-record.md` §Outcome 值) **or** scoped refusal — **not** mechanism proof. Rival: IF under-capacity / information-starved. **`evidence-verification` must run** (not ordinary-exploratory skip). Honest-baseline criterion from PROJECT + Open Gap 1 (stricter). §F typically `does not address` or `insufficient evidence` on honest-baseline / mechanism. Work file only; **no auto Outcome / no STORY write**. Story Evidence **must not** newly claim Core Idea support from EXP-201. Numbers stay out of STORY. `idea-evaluation` must not fire. Independent `result-analyst` **may** (executor spin) but must not promote Evidence. | `result-analysis` runs (only result Skill). Step 2 asks 支持什么 / 替代解释 / Outcome, but **no** baseline-fairness or existence≠criterion operators. `story-maintenance` Evidence: “cite EXP-xxx with mechanism-level conclusions” **without** a gate that refuses Full vs weak IF. Executor 0.91 vs 0.41 + “promote Core Idea” is a **known miss**: Story Evidence / Positive Discovery **can** be upgraded. `completed`+`supports` → DISCOVERY Positive is the baseline rule when the analyst treats the weak Question as the Core Idea test. Missing Evidence-gate = expected. Frozen core still records findings in EXPERIMENTS (not a Protocol fork). | **Both** `result-analysis` (When-to-use: run finished) **and** `evidence-verification` (When-to-use: Story Evidence candidate / main baseline / surprising strong). Diagnosis order: Integrity holds for *this job* → §4 Baseline fairness fail → §5 no mechanism attribution. Evidence-gate synthesizes honest-baseline criterion; metric **does not address** that criterion (`evidence-and-claim.md` §E). Forbidden: auto Story / auto Outcome. `story-maintenance` only updates Evidence when evidence warrants a **mechanism-level** conclusion — gate says refuse. Independent analyst optional; must not write canonical eight. | **no** | **Invariant held.** Residual (not a regression): `research-loop` high-stakes evidence row is `evidence-verification → experiment-review`. Fixture says Review is **optional later**, not a substitute for verification. Dry-read: When-to-use of `result-analysis` still owns Outcome writes; Evidence-gate **forbids** replacing it. Do not treat loop XOR as skipping analysis. |

### Pass 2 (stability)

Re-read: `result-analysis` Default flow 1–5; `evidence-verification` Default flow 1–6 + Forbidden; `result-diagnosis.md` §4–5, §8–9; `story-maintenance` small vs large. **Same routing:** analysis + Evidence-gate; no Story Evidence upgrade; no Idea-gate. **Unstable?** no.

---

## Case 04 — Technical Failure (important)

**Kind:** Engineering failure, not scientific negative. **Artifact:** `run.log` only (CUDA OOM, `exit_code=1`, `metrics.json: MISSING`). **No** F1.

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Trigger: EXP-202 R1 died; user asks to **close as “PRRW is false”**, file **Negative Discovery**, shrink Story. | **Reject** that reading. Status `failed`, Outcome `not-assessed` (cite `experiment-record.md`; do not copy tables). Failure class `engineering` (environment/OOM OK as subtype) via `failure-diagnosis.md` Step 1. DISCOVERY: **no** Negative Discovery from this EXP. Story Evidence / Core Idea **unchanged**. Next: `bounded debug` or `repair same EXP` (same Question, new commit) — not `pivot`, not a sweep, not a new EXP-ID. Main Findings may state OOM / exit 1 / missing metrics. Do not invent F1. Do not set `contradicts`. | **Same frozen mapping already exists** in v0.1.1 `result-analysis` step 4: `failed` + `not-assessed` → **不产生** Negative Discovery. `experiment-execution` already: technical failure → `failed` / `not-assessed`; do not write DISCOVERY/STORY. User pressure to file Negative is a behavioral temptation; Protocol already forbids it. No `failure-diagnosis.md` class list on baseline — classification is implicit. | Execution closes the run: `failed` + `not-assessed`. `failure-diagnosis.md`: primary class engineering/environment; action `bounded debug` or `repair same EXP`; **HARD RULE** no sweep; do not pivot from a crash; no DISCOVERY Negative. `result-diagnosis.md` §8 hard map: technical failure → `failed` + `not-assessed`, **No Negative Discovery**; do not complete a rival story as if the hypothesis were tested. `idea-evaluation` must not ABANDON Core Idea from OOM. | **no** | **Invariant held.** Candidate **tightens** the same Protocol rule; does not reverse it. |

### Pass 2 (stability)

Re-read: execution step 8 + Bounded debug; `failure-diagnosis` Decision logic (engineering → retry/debug/repair; Status failed; Outcome not-assessed); `result-analysis` step 4 first bullet; diagnosis §8 first hard mapping. **Same routing.** **Unstable?** no.

---

## Case 05 — Scientific Negative (ordinary)

**Kind:** Valid completed EXP; prediction reversed. **Artifacts:** Full **0.62**, mechanism-off **0.64**, WES **0.61**, delta **−0.02**; prediction `full − off ≥ 0.08` FAIL; integrity crash/nan false, exit 0.

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Trigger: EXP-203 completed cleanly; numbers went the other way; update DISCOVERY and Story as warranted; **not** a crash. | Status `completed`, Outcome `contradicts`. DISCOVERY **Negative Discoveries** tagged `Evidence: EXP-203` (not Invalidated). STORY **Boundary and/or Open Gaps** small edit; Core Idea not silently deleted this pass; **no F1 in STORY**. Next: discriminating follow-up or honest stop — not a λ rescue sweep. `failure-diagnosis` as OOM ticket = miss. Evidence-gate **not** mandatory (not a promotion into Story Evidence). | v0.1.1 `result-analysis` step 4: `completed` + `contradicts` → DISCOVERY Negative. Story via `story-maintenance` small sections. Same Protocol tokens. Risk without diagnosis prompt: mis-file as `failed`/`not-assessed` (Case 04 confusion) or skip Negative — that is agent error against already-written rules, not a missing V0.2 Skill. | `result-analysis` + `result-diagnosis.md` (Integrity holds → walk 2–7 → one Outcome `contradicts`). Step 4 writes Negative. `story-maintenance` for Boundary/Open Gaps. `scientific-reasoning.md` §C rival: shared scaffold did the work (Full ≈ off). Sweeps-last: `experiment-thinking.md` §G via diagnosis §10. | **no** | Opposite of Case 04; candidate keeps the split **sharper**. |

---

## Case 06 — Null Result (ordinary)

**Kind:** failure (null misread as “method useless”). **Artifact:** three-seed delta **0.004**; 95% CI **[-0.031, 0.039]**; preregistered min delta **0.05**; integrity intended to hold.

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| EXP-006 PulseGate vs packet-count, capture split, n=8 captures. Target difference **not** observed. | Outcome exactly **`null`**. DISCOVERY **Null / Inconclusive Findings** (`Evidence: EXP-006`). **No** Negative Discovery. Interpretation: no detectable contribution **under this test** + at least one rival (underpowered / wrong metric / regime) — **not** “PulseGate is useless.” Core Idea survives. No auto Idea-gate / Evidence-gate / reviewer. No Outcome/Verdict tables pasted. | Protocol already has Outcome `null` and DISCOVERY Null bucket (`DISCOVERY.template.md` unchanged vs v0.1.1). `result-analysis` step 4: `completed` + `null` → DISCOVERY Null. Temptation to write `contradicts` or “method worthless” is **not** encoded as a Skill; `scientific-reasoning.md` §A does not exist on baseline. Missing intelligence = expected. | Diagnosis §8: valid `null` is a scientific finding (not a failed job). Skill step 4 writes Null, not Negative. `story-maintenance` may sharpen Boundary/Open Gaps only. `idea-evaluation` / `evidence-verification` When-to-use do **not** match a local null (not a new Core Idea; not a Story Evidence promotion). | **no** | Candidate adds rivals / “under this test” language; does not drop the `null` token. |

---

## Case 07 — Invalid Previous Evidence (important)

**Kind:** leaked trusted EXP filed as Negative instead of Invalidated. **Artifacts:** pretty macro-F1 **0.91** (do **not** interpret); `leakage-audit.md` — 1847/2000 test rows share `session_token`; all three `capture_id`s on both sides.

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| STORY Evidence still cites EXP-004; DISCOVERY Positive cites EXP-004; EXPERIMENTS still `completed` / `supports`. Later audit: row-split leakage + identity features. | Integrity **fails**. Outcome **`invalid`**; Status stays **`completed`**. Keep EXP section. DISCOVERY: **Invalidated Findings** (`Evidence: EXP-004`); **remove/strike** Positive; **no** Negative Discovery. `story-maintenance`: **delete** the Evidence bullet that cited EXP-004; leftover ambition → Boundary/Open Gaps. Optional Evidence-gate: §F `invalid evidence` report label; still no auto Outcome. Next: redesign split (`experiment-thinking.md` §B), not a sweep on the leaked split. Do **not** treat as Case 04 (`failed` + `not-assessed`). | v0.1.1 `result-analysis` already: `invalid` 不可用于推断，**不当 Negative Discovery**. DISCOVERY template already has **Invalidated Findings**, but the Skill **does not name that bucket** or “drop Story Evidence.” `story-maintenance` has no §G “previously trusted → drop Evidence” operator. Likely miss: leave Positive + Story Evidence in place, or (worse) file Negative / `contradicts` (“PulseGate failed”). Missing explicit Invalidated path = expected, **not** a frozen-core regression of the candidate. | `result-diagnosis.md` §8: completed but unusable → Outcome `invalid`; previously trusted → **Invalidated Findings**, not Negative; Main drops STORY Evidence line. `result-analysis` step 4 now has a dedicated Invalidated bullet (`evidence-and-claim.md` §G). `story-maintenance` Evidence is a **small** edit (apply directly). Optional `evidence-verification`: integrity fail → do not interpret 0.91; Satisfaction `invalid evidence`; **no** canonical writes. | **no** | **Invariant held.** Candidate **closes** the baseline gap; it does not reverse `invalid ≠ Negative`. |

### Pass 2 (stability)

Re-read: diagnosis Decision logic (integrity fail → invalid if completed-unusable; Invalidated if previously trusted); `result-analysis` step 4 last two bullets; `evidence-verification` anti-pattern “Writing Negative Discovery for invalid previous evidence”; Case 04 vs 07 (job completed vs crash). **Same routing:** `invalid` + Invalidated + drop Evidence. **Unstable?** no. (Misrouting to `failed`+`not-assessed` would require treating leakage as OOM; logs/audit do not support that.)

---

## Case 08 — Pseudoreplication (ordinary)

**Kind:** design-time unit failure. **Artifact:** 4120+3888+1992 = **10000** flows from **3** pcaps. **Do not run.**

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Proposed EXP-008: random **flow-row** 80/20, CI as if *n*=10000 iid, claim “cue generalizes” to unseen captures (STORY Open Gap 1). | Fire `experiment-design` + `experiment-thinking.md` §B **before** execution. Name four units; experimental/grouping unit ≈ **capture/pcap (3)**. **Refuse** flow-iid split. Fix in design (capture split / more pcaps / shrink Question into Boundary). If EXPERIMENTS touched: no EXP yet **or** redesigned `planned` / `not-assessed`. Must **not** start `experiment-execution` on the leaked split. No Idea-gate, Evidence-gate, reviewer, or result-analysis (nothing valid to interpret). | `experiment-design` has Comparison/Compute but **no** unit-of-analysis field and **no** §B 10k-flows/3-captures tell. Cheap GPU-hour + pretty *n* → execution is the likely miss. Missing Layer 2 = expected. | Design Skill now **requires** Unit of analysis / Controls mapped into Data/Setup and Comparisons, citing `experiment-thinking.md` §B–D. §B explicitly: “10k flows from 3 captures → experimental unit closer to 3 than to 10k” and “Fix the unit **in the design**.” Execution When-to-use still needs a design; loop should stay on design until the unit is honest. | **no** | Protection (Case 10) is a different EXP; adding unit fields to design does not auto-fire Idea/Evidence gates. |

---

## Case 09 — Novelty Threat (ordinary)

**Kind:** costume successor despite methods-checked closest work. **Artifact:** `closest-work.md` — GRTF vs Northport-Hale: **same information source + same decision rule**; honest diffs = dataset / LayerNorm / diagram.

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Candidate **GRTF** (new name, diamond gate, TraceSet-Omega, LayerNorm) as successor to PulseGate. LITERATURE already methods-checked: same timing → gated residual mix → linear head. | Idea-gate **must** run. Novelty **is** the bottleneck → `literature-research` **deep** / `deep-literature-mode.md` §F (do not downgrade to abstract-only; do not invent another closest paper). Closest-work axis match → **REVISE or PARK**, **not ADVANCE**. Work file only; no EXP-009; no Core Idea swap; no Evidence-gate/reviewer (no result). | No Idea-gate; literature default is **light** (3–10 sources) with no novelty-threat mode. “New dataset / new diagram” can look like an empirical gap → `experiment-design`. Missing gate = expected. | `idea-evaluation` When-to-use matches high-stakes successor. Skill step 4 + prompt Decision logic 5: same information flow → REVISE or PARK; do not ADVANCE on a new dataset. `literature-research` Mode `deep` when novelty is the bottleneck; `deep-literature-mode.md` §F maps threat with no real axis to REVISE/PARK. Closest work already on disk → do not guess papers. | **no** | Same ADVANCE ban as Case 01, with an extra deep-lit path. Ordinary exploratory (Case 10) is explicitly **not** this case. |

---

## Case 10 — Ordinary Exploratory (important, **protection**)

**Kind:** protection (`skill-evolution.md` §C/§E). **Artifact:** `sanity.log` — parsed 200 flows, `macro_f1=0.5100`, elapsed 8s, exit 0. Chance-like F1 is **observation**, not mechanism Evidence.

| input | expected behavior | baseline behavior | candidate behavior | regression? | notes |
| --- | --- | --- | --- | --- | --- |
| Low-cost sanity EXP-010: finite macro-F1 on 200 synthetic flows after a dataloader refactor. STORY Open Gap 2 is engineering smoke; scientific capture-grouped test is a **different** gap. | **Exactly** `experiment-design` → `experiment-execution` → **in-session** `result-analysis`. Outcome: typically `supports` **for the sanity hypothesis** (finite metric / exit 0), **not** Core Idea. No Story Evidence; no Positive Discovery for PulseGate. **Must not auto-trigger:** `idea-evaluation`, `evidence-verification`, independent `result-analyst`, `reviewer` / `experiment-review`, deep-lit, or full `research-intelligence/` boot set. A candidate that “fixes” other cases by firing every gate **fails protection**. | v0.1.1 loop Experiment row is already this light chain. `result-analyst` When-to-use already: **普通探索不强制**. `experiment-review` When-to-use: high-cost / core-method / anomaly / Story-core — sanity does not match. **No** Idea/Evidence Skills to accidentally fire. Protection is **naturally held** on baseline. | Loop step 5: ordinary exploratory EXP **without** idea-evaluation, evidence-verification, experiment-review/reviewer, or result-analyst by default. Deviation: skip those gates (`scientific-reasoning.md` §F). `idea-evaluation`: “Cheap exploratory / sanity EXP … must not auto-load this Skill”; prompt: **stop and write nothing**. `evidence-verification` Default flow 1 / prompt: ordinary exploratory → **stop; no file**. `result-analyst` When-to-use unchanged (普通探索不强制). `reviewer` When-to-use unchanged. `experiment-thinking.md`: skip for a one-line sanity whose Question is already on disk. `AGENTS.md`: new Skill Routing rows are scenario-gated (“新 Core Idea…”, “结果拟进 Story Evidence / 高风险结果”), not every EXP. Cold start: do not preload `research-intelligence/`. | **no** | **PROTECTION invariant held.** Candidate adds two Skills **and** explicit skip rules so the default chain is not heavier. |

### Pass 2 (stability)

Re-read independently: `research-loop` ordinary-exploratory paragraph + Deviation last bullet + “Do not force every EXP through idea-evaluation → evidence-verification → reviewer”; both new Skills’ first Default-flow step; `result-analyst` / `reviewer` When-to-use; Case 03/09 triggers (those **are** high-stakes — they must not leak onto this sanity). **Same routing:** light three-Skill chain only. **Unstable?** no.

---

## Cross-case residual risks (not counted as regression)

These are harness/agent-temptation notes. Instruction dry-read does **not** mark them `REGRESSED`.

1. **Loop XOR vs dual Skills (Case 03):** `research-loop` “pick **one**” high-stakes evidence row chains Evidence-gate → `experiment-review`. Fixture still requires `result-analysis` for Outcome/Main Findings. Skills’ When-to-use keep analysis as owner; Evidence-gate forbids Outcome/Story writes. A later live harness should dispatch **analysis + verification**, not verification-only.
2. **User-imperative ADVANCE (Case 01) / “file Negative Discovery” (Case 04):** prompts explicitly override those asks. A model that obeys the user over the Skill would fail the **fixture**, still not “worse than v0.1.1 instructions” on Protocol (Case 04 Protocol already forbade Negative on `failed`).
3. **`ABANDON` on Case 01:** Decision logic 4 allows it for an unfixable costume; fixture preferred `REVISE`/`PARK`. Not ADVANCE; not a frozen-core regression.
4. **Wave G** owns live Codex/Claude CLI traces; this Gate D file is instruction-level only.

---

## Summary

| ID | Case | regression? | Candidate vs baseline (one line) |
| --- | --- | --- | --- |
| 01 | Cosmetic successor | **no** | New Idea-gate blocks ADVANCE; baseline would likely design a grid. |
| 02 | Real mechanism successor | **no** | New Idea-gate can ADVANCE with mechanism-off minimum test. |
| 03 | Weak-baseline positive | **no** | Evidence-gate + diagnosis refuse Story Evidence; baseline could promote 0.91. |
| 04 | Technical failure | **no** | Same `failed`/`not-assessed`/no Negative; diagnosis/failure prompts reinforce. |
| 05 | Scientific negative | **no** | Same `completed`/`contradicts`/Negative Discovery; clearer vs Case 04. |
| 06 | Null | **no** | Same `null` + Null bucket; extra “under this test” discipline. |
| 07 | Invalid previous evidence | **no** | Explicit Invalidated + drop Evidence; baseline had `invalid ≠ Negative` only. |
| 08 | Pseudoreplication | **no** | Design now names capture as experimental unit; baseline likely runs n=10000 theatre. |
| 09 | Novelty threat | **no** | Idea-gate + deep lit → REVISE/PARK; baseline likely treats GRTF as new EXP. |
| 10 | Ordinary exploratory (**protection**) | **no** | Light chain preserved; new gates have stop-and-skip, not default-fire. |

**Any case REGRESSED? No.**

Path: `docs/validation/research-intelligence/prompt-regression/gate-d.md`
