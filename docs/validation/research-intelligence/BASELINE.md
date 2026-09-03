# V0.2 Phase 0 — Baseline Freeze

Do not move tag `v0.1.1`. Do not move tag `v0.1`.

| Item | Value |
|------|--------|
| Branch | `v0.2-research-intelligence` |
| HEAD SHA at freeze | `dd7e43f72db1883209baf7cfef151ac218a17b92` |
| HEAD subject | Rename project identity to research-story-speaker |
| `v0.1.1` tag peeled commit | `762deb4c9db896acb5c00066b8e6dc5a63732cfa` |
| `v0.1` tag peeled commit | `8db3b301f5bce878d6c2ee4a61bcb234c9609c3d` |
| Frozen date | 2026-09-04 |

Verification (2026-09-04, `/usr/bin/git` in `/Users/herxanadu/research-story-speaker`):

- Branch already existed at freeze; not created in this step.
- `git rev-parse v0.1.1^{commit}` = `762deb4c9db896acb5c00066b8e6dc5a63732cfa` (matches required).
- `git rev-parse v0.1^{commit}` = `8db3b301f5bce878d6c2ee4a61bcb234c9609c3d` (unchanged).
- Both tags are annotated. Tag *objects* differ from peeled commits and must not be treated as tag movement:
  - `v0.1.1` tag object `90a41d5dba39c2f82e8c963eb44d4c8ff339c7b1`
  - `v0.1` tag object `96340ad8b7f4a2712c924b0476d403c06ff64a45`
- Both peeled commits are ancestors of freeze HEAD.

Root `.research/` remains the UNINITIALIZED template. Wave A–H validation lives under `docs/validation/research-intelligence/` only.
