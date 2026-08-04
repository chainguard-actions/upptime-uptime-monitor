<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.42.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.42.1** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in build.yml use mutable version tags instead of full 40-character SHA commit hashes, making the workflow vulnerable to supply-chain attacks if those tags are moved. Failing references: `actions/checkout@v4` (line 14), `actions/setup-node@v4` (line 21), `stefanzweifel/git-auto-commit-action@v4.9.2` (line 36).

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:21`
- `.github/workflows/build.yml:36`

### unpinned-uses (severity: high)

All `uses:` references in node.yml use mutable version tags instead of full 40-character SHA commit hashes. Failing references: `actions/checkout@v4` (line 14), `actions/setup-node@v4` (line 17).

Locations:

- `.github/workflows/node.yml:14`
- `.github/workflows/node.yml:17`

### unpinned-uses (severity: high)

All `uses:` references in release.yml use mutable version tags instead of full 40-character SHA commit hashes. Failing references: `actions/checkout@v4` (line 14), `actions/setup-node@v4` (line 20).

Locations:

- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:20`

### missing-permissions (severity: medium)

build.yml has no top-level `permissions:` key and the single job (`release`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad.

Locations:

- `.github/workflows/build.yml:1`

### missing-permissions (severity: medium)

node.yml has no top-level `permissions:` key and the single job (`release`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad.

Locations:

- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 findings across 3 workflow files:

1. build.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2 → @be7095c202abcf573b09f20541e0ee2f6a3a9d9b. Added top-level `permissions: contents: write` (required for the git-auto-commit-action to push commits).

2. node.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Added top-level `permissions: contents: read` (minimal permissions for a build/test-only workflow).

3. release.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Permissions were already present (contents: write, id-token: write) so no change needed there.

