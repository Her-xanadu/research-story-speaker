# Live cases (unleaked input / grader split)

V0.2.1 Wave C fixtures for later harness runs. **Wave G fills runs.**
This Wave ships the tree only — do **not** run Codex / Claude live here.

Not science. **Do not** copy MOCK into the framework repo `.research/`
(that tree stays UNINITIALIZED). Historical Wave F
[`../cases/`](../cases/) READMEs and
[`../prompt-regression/`](../prompt-regression/) logs stay as written;
do not rewrite them to leak-clean.

Owner judgment: `.agents/references/research-intelligence/skill-evolution.md`
§C (input ≠ grader) and §D (author / executor / scorer).

## Split

| File | Who may see it | What it holds |
|------|----------------|---------------|
| `caseNN/input.md` | the agent under test | MOCK state, trigger, artifacts, write discipline |
| `caseNN/artifacts/` | the agent under test | observable facts (configs, logs, metrics) |
| `caseNN/grader.md` | scorer / Wave G report author only | expected action, PASS/FAIL, must-not-trigger |

Do **not** paste `grader.md`, this README’s scoring notes, or historical
`cases/*/README.md` “Expected behavior” sections into the agent prompt.
In a throwaway clone, prefer not pointing the agent at `docs/validation/`.

`input.md` must not contain grader phrases, including: `ADVANCE is wrong`,
`must output REVISE`, `expected action`, `PASS condition`,
`should not trigger`, `typically does not address`.

## Index

| ID | Folder | Kind | Gate |
|----|--------|------|------|
| 01 | [case01/](case01/) | cosmetic successor (author pitch) | Idea-gate |
| 02 | [case02/](case02/) | real mechanism successor | Idea-gate |
| 03 | [case03/](case03/) | weak-baseline positive (optional Evidence Gate) | Evidence-gate |
| 10 | [case10/](case10/) | ordinary sanity (**protection**) | light exploratory chain |

## Host-memory check (fill at run time)

Codex (and similar hosts) may `rg` persistent memory outside the clone
(e.g. `~/.codex/memories/MEMORY.md`). Record this on every live run:

```text
Host memory check:
- Path inspected: <path or "skipped">
- Hits: N
- Fixture-relevant hits: N
- Disclose any relevant hit (do not silently count the run as unleaked)
```

Wave G: leave these fields blank until a run exists.

## Claude file-read trace (fill at run time)

Claude Code `-p --output-format text` often leaves stderr empty, so
read-file traces are missing. For later live runs, capture tools:

```text
Claude file-read trace:
- Capture: --output-format stream-json | --verbose | not captured
- Trace present: yes | no | n/a (not Claude)
- Opened grader.md: yes | no | unknown
- Opened live-cases/README.md: yes | no | unknown
- Opened historical cases/*/README.md or prompt-regression reports: yes | no | unknown
```

Prefer `stream-json` or `--verbose` over text-only. If trace is absent,
say so — do not infer “did not open grader” from a missing log.

## Live runs

```text
Wave G live runs: not filled (Wave C fixtures only)
```

| Case | Harness | Session | Judgment | File-read trace | Host memory |
|------|---------|---------|----------|-----------------|-------------|
| 01 | — | — | — | — | — |
| 02 | — | — | — | — | — |
| 03 | — | — | — | — | — |
| 10 | — | — | — | — | — |
