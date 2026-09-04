---
name: literature-research
description: >-
  Local-first literature for the focal Story gap: paper-consult on Obsidian
  vault, four-dimension adequacy (coverage, access depth, closest-work,
  freshness), paper-find only when inadequate or freshness required, bounded
  NEW ingest via paper-library, enrich Gap-relevant subset, optional
  paper-nutrients, Main writes LITERATURE.md. Light default; deep optional.
  Use for 查文献, prior work, novelty checks. No independent web/arXiv/S2
  search in this Skill. Do not update STORY directly.
---

# Literature Research

**本地库优先；联网只为补库和 freshness。** 联网唯一入口：`paper-find`（`~/.agents/skills/paper-find/`）。稳态读库：`paper-consult`。沉淀：`paper-library mode: ingest`（bounded NEW 全量）；精读：`mode: enrich`（Gap subset）。可选 `paper-nutrients`（非阻塞）。

交棒合同：[`paper-nutrients/references/consumers.md`](/Users/herxanadu/.agents/skills/paper-nutrients/references/consumers.md)

Thin Skill：把文献证据写入 `.research/LITERATURE.md`。格式见 [state-files.md](../../references/state-files.md) § LITERATURE.md。Gap 路由见 [story-loop.md](../../references/story-loop.md)。

**Vault 单写者：** 仅 Main 跑 `paper-find` / `paper-library`。`literature-scout` 只 `paper-consult` 只读；不足时返回 `NEEDS_REFRESH`，不写 Vault。

## Mode

`light` | `deep`. **Default is light.**

- **Light** — 3–10 highly relevant sources **after** local consult + optional find pass. If they already name a discriminating test, stop and hand off to `experiment-design`.
- **Deep** — novelty unclear, new core mechanism, Story-changing conflict, or evaluation convention bottleneck. Operators: [deep-literature-mode.md](../../references/research-intelligence/deep-literature-mode.md) §G：**Pass 1 / Pass 2 = 是否再开 `paper-find` bounded 队列**，不是本 Skill 内自建 arXiv/S2/web 步骤。Main 仍写 `LITERATURE.md`；scout 只写 `.research/work/`。

## Four-dimension adequacy (all must hold to skip find)

Not a score — four booleans:

| Dimension | Question |
| --- | --- |
| **coverage** | Does the local vault answer the current scientific question? |
| **access depth** | Are key claims blocking the Gap readable at sufficient depth? |
| **closest-work** | When novelty/baseline matters, is the nearest line covered? |
| **freshness** | When time-sensitive, is the vault fresh enough? |

**local-first ≠ permanently local-only.** Re-run `paper-find` when freshness is a scientific requirement (novelty audit, new Core Idea, user asks latest, READY_FOR_WRITING prior-art, field may have moved) even if other dimensions pass.

## When to use

- Open Gap blocked by prior work or novelty risk.
- Method conflict needs literature grounding.
- `research-loop` routed to Literature.
- `experiment-design` needs baseline papers.

Do **not** use for: `experiment-execution`, `result-analysis`, `story-maintenance`, `research-memory`.

## Five lenses (synthesis)

| Lens | Question |
| --- | --- |
| **Known** | What does the field establish? |
| **Conflicts** | Where do papers disagree with our Story? |
| **Supports** | What backs our direction? |
| **Suggests** | What methods/baselines/experiments do papers imply? |
| **Novelty** | What appears done; what gap remains? |

Persist only **valuable** entries in `LITERATURE.md` — not search logs.

## Default flow

1. **Anchor gap** — `.research/STORY.md`, `.research/STATE.md` focus.
2. **Scan project memory** — `.research/LITERATURE.md` for duplicates.
3. **paper-consult** — four-part query; read `consult_status` (`hits` | `no_hits` | `unavailable`).
4. **Adequacy** — four dimensions + freshness triggers (above). If adequate **and** not freshness-forced → **0** `search.sh` calls; synthesize from vault cards.
5. **If inadequate or freshness** — `paper-find` (`search.sh` + bounded ALL NEW acquire) → `paper-library ingest` for every NEW → `paper-consult` again.
6. **Rank** — relevance to Gap; subset for enrich only.
7. **paper-library enrich** — closest-work / novelty / mechanism subset (not entire queue).
8. **Optional paper-nutrients** — `--trigger-kind literature_research`; catch non-zero exits; do not treat generator crash as empty.
9. **Write LITERATURE.md** — per [LITERATURE.template.md](../../templates/LITERATURE.template.md); include **paper_id**, **vault path**, Identifier, Access when local object exists.
10. **STATE** — brief next step if routing changed.
11. **Hand off** — `experiment-design` | `story-maintenance` | `research-loop`.

### Degraded mode (no Obsidian vault)

`consult_status: unavailable` → `paper-find` candidates only; LITERATURE entries `abstract-only` + STATE notes not ingested. **Do not** treat abstract-only as methods-checked or close novelty from metadata alone.

### Pending institutional PDFs

Do not block `research-loop`. If pending item is closest-work / novelty-killer → keep Gap open; lower claim confidence.

### Ingest vs enrich

```text
Acquisition: ALL NEW in bounded queue → ingest
Understanding: Gap-relevant subset → enrich
```

## Deep flow (§G → find passes)

1. RQ freeze ([deep-literature-mode.md](../../references/research-intelligence/deep-literature-mode.md) §A).
2. `paper-consult` + adequacy.
3. **Find Pass 1** (landscape) if needed → ingest all NEW → consult.
4. Closest-work / contradiction still open? **Find Pass 2** (targeted) → ingest → consult.
5. Stop when RQs actionable; at most one justified extension pass ([§G](../../references/research-intelligence/deep-literature-mode.md)).
6. Enrich subset; optional nutrients; Main → LITERATURE.

**No** independent web / arXiv / Semantic Scholar / Zotero search steps in this Skill or scout.

## Reads

| Priority | Files |
| --- | --- |
| Required | `.research/STORY.md`, `.research/LITERATURE.md` |
| Often | `.research/STATE.md`, `.research/PROJECT.md`, `.research/DISCOVERY.md`, `.research/RESOURCES.md` |
| External skills | `~/.agents/skills/paper-consult`, `paper-find`, `paper-library`, `paper-nutrients` |
| Deep only | [deep-literature-mode.md](../../references/research-intelligence/deep-literature-mode.md), [literature-synthesis.md](../../prompts/literature-synthesis.md) |
| Subagent | `.agents/subagents/literature-scout.md` |

## Updates

| File | What |
| --- | --- |
| `.research/LITERATURE.md` | Valuable paper sections only (Main) |
| `.research/STATE.md` | Routing change (brief) |

Do **not** update `EXPERIMENTS.md`, `DISCOVERY.md`, or `STORY.md` here.

## Deviation allowed

- Delegate parallel reading to `literature-scout` (consult-only); integrator writes LITERATURE from `.research/work/`.
- Light when 3–10 sources already name discriminating EXP.
- Skip low-relevance papers — note scope in STATE.
- Verify methods before treating abstract claims as findings.
- When LITERATURE already covers gap and vault adequate+fresh → update Relation only; **0** find.

Boundaries:

- No literature dumps in STORY.
- No scout Vault writes.
- nutrients operational failure ≠ empty packet.
