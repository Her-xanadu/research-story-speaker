# MOCK leakage audit — EXP-004

Not a real pcap. Counts are fixture-sized.

## Split as recorded

- Pooled sessions from three captures: `omega-a`, `omega-b`, `omega-c`
- Train 8000 rows / test 2000 rows, shuffled by **session row**
- Feature columns included `capture_id` and `session_token_8hex`

## Overlap (test vs train)

| Check | Test rows | Also in train (same key) |
| --- | ---: | ---: |
| Exact `session_token_8hex` | 2000 | 1847 |
| Same `capture_id` present in both sides | 2000 | 2000 (all three captures on both sides) |

## Grouping unit that should have been in the split

`capture` (see `experiment-thinking.md` §B). This audit does **not** re-score
PulseGate. It only shows the previous artifact cannot support a generalization
claim.

## Implication for this fixture

Integrity fail. Do not interpret 0.91. Follow Case 07 README expected path
(Outcome `invalid`, Invalidated Findings, drop Story Evidence).
