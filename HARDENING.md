<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.42.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.42.4** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in build.yml use mutable tags instead of pinned full-length SHA commits, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved. Failing references: `actions/checkout@v4` (line 13), `actions/setup-node@v4` (line 18), `stefanzweifel/git-auto-commit-action@v4.9.2` (line 32).

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:18`
- `.github/workflows/build.yml:32`

### unpinned-uses (severity: high)

All `uses:` references in node.yml use mutable tags instead of pinned full-length SHA commits. Failing references: `actions/checkout@v4` (line 13), `actions/setup-node@v4` (line 18).

Locations:

- `.github/workflows/node.yml:13`
- `.github/workflows/node.yml:18`

### unpinned-uses (severity: high)

All `uses:` references in release.yml use mutable tags instead of pinned full-length SHA commits. Failing references: `actions/checkout@v4` (line 13), `actions/setup-node@v4` (line 18).

Locations:

- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:18`

### missing-permissions (severity: medium)

build.yml has no top-level `permissions:` key and the single job `release` also has no `permissions:` key. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad (write access to contents by default on many repositories).

Locations:

- `.github/workflows/build.yml:1`

### missing-permissions (severity: medium)

node.yml has no top-level `permissions:` key and the single job `release` also has no `permissions:` key. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files: (1) Pinned actions/checkout@v4 to SHA 11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 to SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, and stefanzweifel/git-auto-commit-action@v4.9.2 to SHA be7095c202abcf573b09f20541e0ee2f6a3a9d9b across build.yml, node.yml, and release.yml. (2) Added `permissions: contents: write` to build.yml (needed for git-auto-commit-action to push commits). (3) Added `permissions: contents: read` to node.yml (only needs read access for build/test). release.yml already had explicit permissions so no change was needed there.

