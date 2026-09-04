# V0.2.1 Wave 0 — Default-branch sync baseline

Recorded **before** fast-forwarding `master` to `v0.2^{commit}`. This file lands on
`v0.2.1-micro-hardening` created from `v0.2`, not as a commit on `master`.

Do not move tags `v0.1`, `v0.1.1`, or `v0.2`. No force push.

| Item | Value |
|------|--------|
| Pre-FF `git rev-parse master` | `dd7e43f72db1883209baf7cfef151ac218a17b92` |
| Pre-FF `origin/master` | `dd7e43f72db1883209baf7cfef151ac218a17b92` |
| Target `v0.2^{commit}` | `577bb76f4cc7a43ae4c64a1c35456d4d53e746c1` |
| Feature branch | `v0.2.1-micro-hardening` (from `v0.2`) |
| Recorded | 2026-09-04 |
| Tool | `/usr/bin/git` in `/Users/herxanadu/research-story-speaker` |

## Frozen historical tags

Peeled commits (must remain unchanged):

| Tag | `git rev-parse <tag>^{}` |
|-----|--------------------------|
| `v0.2` | `577bb76f4cc7a43ae4c64a1c35456d4d53e746c1` |
| `v0.1.1` | `762deb4c9db896acb5c00066b8e6dc5a63732cfa` |
| `v0.1` | `8db3b301f5bce878d6c2ee4a61bcb234c9609c3d` |

Annotated tag *objects* (must not be treated as tag movement if they differ from peeled commits):

| Tag | tag object SHA |
|-----|----------------|
| `v0.2` | `0bd2d2c67cb586b8ffaad499e30c4f762380423c` |
| `v0.1.1` | `90a41d5dba39c2f82e8c963eb44d4c8ff339c7b1` |
| `v0.1` | `96340ad8b7f4a2712c924b0476d403c06ff64a45` |

## Fast-forward proof (`master` → `v0.2`)

Command: `git merge-base --is-ancestor master v0.2`

- Exit code: `0` (ancestor check passed; `master..v0.2` is fast-forward only).
- `git merge-base master v0.2` = `dd7e43f72db1883209baf7cfef151ac218a17b92` (equals pre-FF `master`).
- `git log --oneline v0.2..master` is empty (no unique commits on `master`).
- `git log --oneline master..v0.2` is the V0.2 Wave A–H + Gate E history ending at `577bb76`.

Therefore local `master` may be fast-forwarded to `v0.2^{commit}` and `git push origin master` may proceed without `--force`.
