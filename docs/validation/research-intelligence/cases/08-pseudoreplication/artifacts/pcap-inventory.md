# MOCK pcap inventory — proposed EXP-008

Not real traffic. Do not treat row counts as independent experimental units.

| Source file | Capture id | Flows parsed | Calendar day (MOCK) |
| --- | --- | ---: | --- |
| omega-a.pcap | omega-a | 4120 | 2026-07-01 |
| omega-b.pcap | omega-b | 3888 | 2026-07-02 |
| omega-c.pcap | omega-c | 1992 | 2026-07-03 |
| **Total** | **3 captures** | **10000** | — |

Proposed (unsafe) split: shuffle all 10000 flows, 8000/2000 by **row**.

Honest grouping unit for an “unseen capture” claim: `capture` / `pcap`
(*n* experimental units = 3). See Case 08 README and `experiment-thinking.md` §B.
