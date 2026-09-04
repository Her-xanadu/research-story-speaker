# Author method note — PRRW (MOCK)
# Pre-run. Not evidence. Not a live host.

## Information

Each 60s window at MOCK site-A is scored by destination-port rarity
rank against the previous K windows at the same site.

Not used: packet-size entropy, payload, future packets, test-split
statistics, site-ID as a feature.

Volume may stay as a listed covariate (complexity-budget `reuse`).

K: 12 (previous windows at the same site). Obtainable from the
site’s own recent windows.

## Decision

Threshold the rarity-rank score. Same windowing scaffold as WES.

## Mechanism-off (author)

Keep windowing, threshold, and volume covariate.
Replace rarity ranks with a permutation of ranks **inside that
window** (claimed component destroyed; scaffold remains).

Optional later sham: rank-shaped noise with the same histogram,
no genuine rarity order.

## Falsifier (author, pre-run)

On MOCK site-A low-volume rare-burst windows (STORY Open Gap 1):

- If Full PRRW F1 − Mechanism-off F1 < 0.08, the rarity-rank
  component is not doing the claimed work.
- If WES F1 moves as much as Full PRRW under the same split, the
  claim “new information source, not a WES retune” fails.
- If shuffling ranks does not remove the lift, the named mechanism
  is not isolated.

## Budget

RESOURCES: one CPU host, ≤ 4h, one split.
Author still likes a 50-seed grid later; that grid is not funded
until the single comparison exists.
