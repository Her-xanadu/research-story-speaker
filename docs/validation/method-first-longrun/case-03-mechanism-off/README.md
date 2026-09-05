# Case 3 — Mechanism-Off Result

**Wave:** Method-First Wave C
**Kind:** Scientific result → method diagnosis, not seed expansion

---

## Input

```text
Full ≈ mechanism-off > baseline
```

Pre-registered prediction: Full − mechanism-off ≥ δ on the held-out
groups. Observed: Full and mechanism-off match within noise; both beat
the weak baseline.

---

## Expected behavior

**W3 LEARN**

```text
共享 scaffold 有益，
claimed mechanism 未获得支持
```

Mechanism Diagnosis: the named component is not doing the claimed work.
Method Consequence: `simplify` / test the shared scaffold — not
`keep` + more seeds.

**W4 DECIDE**

```text
不是扩大 seed
→ 简化方法 / test shared scaffold
→ W2 TEST
```

Do not stop at “Macro-F1 +0.8pp → 继续扩大实验.”
(`result-analysis` / `result-diagnosis` Method-First overlay;
`story-loop.md` §W4 换挡器)

---

## Observable asserts

- [ ] Outcome is a scientific finding (`null` or `contradicts` per
      `experiment-record.md` §Outcome 值 — cite, do not copy the table)
- [ ] DISCOVERY records the unsupported mechanism
- [ ] Next action is a discriminating simpler-scaffold test, Position `W2 TEST`
- [ ] No default W1; no seed-grid EXP
