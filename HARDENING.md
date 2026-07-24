<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use Actions pinned to mutable tags or version strings instead of immutable 40-character SHA digests, making them vulnerable to supply-chain attacks if the tag is moved.

.github/workflows/build.yml:
  - uses: actions/checkout@v6 (line 14)
  - uses: actions/setup-node@v4 (line 20)
  - uses: stefanzweifel/git-auto-commit-action@v4.9.2 (line 34)

.github/workflows/node.yml:
  - uses: actions/checkout@v6 (line 14)
  - uses: actions/setup-node@v4 (line 17)

.github/workflows/release.yml:
  - uses: actions/checkout@v6 (line 14)
  - uses: actions/setup-node@v4 (line 20)

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:20`
- `.github/workflows/build.yml:34`
- `.github/workflows/node.yml:14`
- `.github/workflows/node.yml:17`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:20`

### missing-permissions (severity: medium)

build.yml and node.yml have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (often `write-all` for older repositories), granting the GITHUB_TOKEN broader access than necessary.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 unpinned action references across 3 workflow files to their full 40-character SHA digests (actions/checkout@v6 → d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2 → be7095c202abcf573b09f20541e0ee2f6a3a9d9b). Added top-level permissions blocks to build.yml (contents: write, required for the git-auto-commit-action to push commits) and node.yml (contents: read, sufficient for checkout and build/test). release.yml already had a permissions block and only needed action pinning.

