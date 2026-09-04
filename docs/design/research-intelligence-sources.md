# Research Intelligence Sources

Principle: borrow ideas, rephrase locally. Do not copy upstream skill
bodies, procedure lists, or long prompts.

- **CC-BY-4.0 / MIT / Apache-2.0** (when uniquely identified): ideas may
  be restated locally; still no verbatim skill text in this repo.
- **CC-BY-NC-SA-4.0 / NC / SA / unknown / license conflict**: abstract
  thought only; no copied procedures.

License re-check date: 2026-09-04 (V0.2.1 Wave F). Historical Phase 0
snapshot (absolute paths left as evidence):
`docs/validation/research-intelligence/source-audit/2026-09-04-license-recheck.md`.

This registry records **URL, specific file, license evidence, access
date, and concept adapted**. Licenses are taken from fetched `LICENSE`
/ `LICENSE.md` files or SKILL.md `license:` frontmatter. **Never
guessed.** If the source file cannot be uniquely identified:

```text
License: unknown
Use: abstract idea only
Copied text: no
```

| Source | Repository URL | Specific file | License | Access date | Concept adapted | Local adaptation | Copied text | Notes |
|--------|----------------|---------------|---------|-------------|-----------------|------------------|-------------|-------|
| Status vs scientific interpretation | this repo (V0.1.1) | `.agents/references/story-loop.md`, `.agents/references/experiment-record.md`, `.agents/skills/result-analysis/` | this repo | 2026-09-04 | Engineering Status is not scientific Outcome; a crash is not a negative result | Restate as scientific objects in `scientific-reasoning.md` | no | Protocol enums stay in existing Protocol files |
| Experiments that change judgment | this repo (V0.1.1) | `.agents/references/story-loop.md` | this repo | 2026-09-04 | Prefer work that can change Problem, Core Idea, or route | Cost / information and must / nice / cut thinking in `experiment-thinking.md` | no | Local Story loop remains owner |
| Direct evidence over executor narrative | this repo (V0.1.1) | `.agents/subagents/reviewer.md` | this repo | 2026-09-04 | Reviews read artifacts; work reports are hypotheses | Integrity-first claim handling in `evidence-and-claim.md` | no | Verdict list stays in `reviewer.md` |
| Kill weak ideas before expensive work | [HKUSTDial/Supervisor-Skills](https://github.com/HKUSTDial/Supervisor-Skills) | [`skills/idea-evaluator/SKILL.md`](https://github.com/HKUSTDial/Supervisor-Skills/blob/main/skills/idea-evaluator/SKILL.md) | **conflict** — SKILL.md frontmatter `CC-BY-4.0`; repo [`LICENSE`](https://github.com/HKUSTDial/Supervisor-Skills/blob/main/LICENSE) is **CC-BY-NC-SA-4.0**. Wave F does not guess which governs. | 2026-09-04 | Early fatal-flaws audit before investment | Mechanism-identity and Story-anchored audit in `idea-and-mechanism.md` | no | **Use: abstract idea only.** Did not import Higher/Faster/Stronger/Cheaper/Broader, paper-type taxonomy, or Strong-Accept / Reject verdicts |
| Closest-work / novelty threat | [HKUSTDial/Supervisor-Skills](https://github.com/HKUSTDial/Supervisor-Skills) | `idea-evaluator` (above); [`skills/deep-research/SKILL.md`](https://github.com/HKUSTDial/Supervisor-Skills/blob/main/skills/deep-research/SKILL.md) | `deep-research`: SKILL.md frontmatter **CC-BY-NC-SA-4.0**, matches repo LICENSE. `idea-evaluator`: see conflict row | 2026-09-04 | Ask what already exists; look for disconfirming literature | Closest-work as one mechanism-difference axis, not a paper quota | no | NC-SA / conflict → ideas only, fully rephrased |
| Freeze questions before surveying | [HKUSTDial/Supervisor-Skills](https://github.com/HKUSTDial/Supervisor-Skills) | [`skills/deep-research/SKILL.md`](https://github.com/HKUSTDial/Supervisor-Skills/blob/main/skills/deep-research/SKILL.md) | **CC-BY-NC-SA-4.0** (frontmatter + repo LICENSE, both fetched) | 2026-09-04 | Pin research questions first; search from more than one angle; check citations | Optional expensive literature mode (`deep-literature-mode.md`), not default literature | no | **Use: abstract idea only.** No MECE survey-paper deliverable; no copied Phase/Step procedure text |
| Complexity budget / deletion | V0.2 plan + common research practice | — | n/a | 2026-09-04 | reuse / new / excluded; delete-to-test identity | Mechanism identity (`idea-and-mechanism.md`) | no | Not taken from a single copyrighted skill body |
| Control taxonomy / high-information tests | [wanshuiyin/Auto-claude-code-research-in-sleep](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep) (ARIS) | [`skills/experiment-plan/SKILL.md`](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/blob/main/skills/experiment-plan/SKILL.md) | **MIT** — repo [`LICENSE`](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/blob/main/LICENSE) fetched 2026-09-04. SKILL.md has **no** `license:` frontmatter | 2026-09-04 | mechanism-off vs baseline; tests that change beliefs | Controls and prediction matrix (`experiment-thinking.md`) | no | Concept only. Skill body not quoted. Codex copy exists at `skills/skills-codex/experiment-plan/` |
| Criterion ≠ artifact | [zjunlp/AutoSciRub](https://github.com/zjunlp/AutoSciRub) | [`plugins/autoscirub/skills/criterion-level-verification/`](https://github.com/zjunlp/AutoSciRub/tree/main/plugins/autoscirub/skills/criterion-level-verification); paper [arXiv:2608.31076](https://arxiv.org/abs/2608.31076) | **MIT** — repo [`LICENSE`](https://github.com/zjunlp/AutoSciRub/blob/main/LICENSE) fetched 2026-09-04 | 2026-09-04 | A file existing is not claim support | Four-layer inequality (`evidence-and-claim.md`) | no | No `RUBRIC.md` state file. Skill bodies not quoted |
| Hypothesis / rival / falsifiability objects | [K-Dense-AI/scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills) (likely analogue; also common scientific method) | [`skills/hypothesis-generation/SKILL.md`](https://github.com/K-Dense-AI/scientific-agent-skills/blob/main/skills/hypothesis-generation/SKILL.md); [`skills/scientific-critical-thinking/SKILL.md`](https://github.com/K-Dense-AI/scientific-agent-skills/blob/main/skills/scientific-critical-thinking/SKILL.md) | **MIT** — repo [`LICENSE.md`](https://github.com/K-Dense-AI/scientific-agent-skills/blob/main/LICENSE.md) fetched via GitHub license API 2026-09-04. SKILL.md frontmatter on those skills reports `MIT` / `MIT license` | 2026-09-04 | Keep hypothesis, rival, prediction, and claim distinct; critique evidence quality | Scientific objects in `scientific-reasoning.md` | no | **Use: restated locally.** No GRADE/Cochrane procedure lists imported. V0.2 did not treat these files as copied sources |
| Bounded skill change | skill-doctor / skill-upper / experience-to-skill *ideas* | **not uniquely identified** — several public projects share these names | **unknown** | 2026-09-04 | Repeated friction; atomic change; protection cases; no auto-deploy | Framework-maintenance mode (`skill-evolution.md`), not an 11th Skill | no | **License: unknown / Use: abstract idea only / Copied text: no.** Public names exist but V0.2 opened none of them, so Wave F does not attach another repo's LICENSE |
| Decision frontier | [mattpocock/skills](https://github.com/mattpocock/skills) | [`skills/productivity/grilling/SKILL.md`](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md) | **MIT** — repo [`LICENSE`](https://github.com/mattpocock/skills/blob/main/LICENSE) fetched 2026-09-04. SKILL.md has **no** `license:` frontmatter | 2026-09-04 | Ask only preference or authorization; batch; default + reason | Wave D `workspace-resume` only; not a new Skill | no | Concept only. Interview procedure text not copied |

## Attribution (CC / named skills)

`idea-evaluator` and `deep-research` are from HKUSTDial Supervisor-Skills
(Yuyu Luo, methodology, and contributors). Local files rephrase *ideas*
only. They do not reproduce skill bodies.

Because `idea-evaluator` SKILL.md still says CC-BY-4.0 while the
repository LICENSE file is CC-BY-NC-SA-4.0, this repo treats that skill
as **abstract idea only**.

## Out of scope

- Historical harness logs under `docs/validation/` keep host absolute
  paths as evidence. Wave F does not rewrite them.
- Wave E meta-rule dedup across Skills/prompts is a separate change.
