<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.13

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.13** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in build.yml use mutable tag refs instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved or compromised. Failing references: `actions/checkout@v6` (line 15), `actions/setup-node@v4` (line 21), `stefanzweifel/git-auto-commit-action@v4.9.2` (line 35).

Locations:

- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:21`
- `.github/workflows/build.yml:35`

### unpinned-uses (severity: high)

All `uses:` references in node.yml use mutable tag refs instead of pinned 40-character commit SHAs. Failing references: `actions/checkout@v6` (line 14), `actions/setup-node@v4` (line 17).

Locations:

- `.github/workflows/node.yml:14`
- `.github/workflows/node.yml:17`

### unpinned-uses (severity: high)

All `uses:` references in release.yml use mutable tag refs instead of pinned 40-character commit SHAs. Failing references: `actions/checkout@v6` (line 15), `actions/setup-node@v4` (line 21).

Locations:

- `.github/workflows/release.yml:15`
- `.github/workflows/release.yml:21`

### missing-permissions (severity: medium)

build.yml has no top-level `permissions:` key and the single job (`release`) also has no `permissions:` key. Without explicit permissions, the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/build.yml:1`

### missing-permissions (severity: medium)

node.yml has no top-level `permissions:` key and the single job (`release`) also has no `permissions:` key. Without explicit permissions, the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 findings across 3 workflow files:

**build.yml**: Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2 → @be7095c202abcf573b09f20541e0ee2f6a3a9d9b. Added `permissions: contents: write` (required for git-auto-commit-action to push commits).

**node.yml**: Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Added `permissions: contents: read` (only reads code for build/test).

**release.yml**: Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Already had a permissions block (`contents: write`, `id-token: write`), so no permissions change needed.

