<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.12

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.12** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files use mutable tags instead of full 40-character SHA commit hashes, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

- build.yml: `actions/checkout@v6`, `actions/setup-node@v4`, `stefanzweifel/git-auto-commit-action@v4.9.2`
- node.yml: `actions/checkout@v6`, `actions/setup-node@v4`
- release.yml: `actions/checkout@v6`, `actions/setup-node@v4`

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:18`
- `.github/workflows/build.yml:33`
- `.github/workflows/node.yml:13`
- `.github/workflows/node.yml:17`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:17`

### missing-permissions (severity: medium)

`build.yml` and `node.yml` have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows run with the default (often overly broad) GITHUB_TOKEN permissions, which can allow unintended write access to repository contents, packages, or other resources.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

**unpinned-uses** (7 locations):
- actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 # v6 (in all 3 files)
- actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4 (in all 3 files)
- stefanzweifel/git-auto-commit-action@v4.9.2 → @be7095c202abcf573b09f20541e0ee2f6a3a9d9b # v4.9.2 (in build.yml)

**missing-permissions** (2 locations):
- build.yml: Added `permissions: contents: write` (needed for git-auto-commit-action to push commits)
- node.yml: Added `permissions: contents: read` (only needs to checkout code for CI build/test)
- release.yml already had `permissions: contents: write` and `id-token: write` defined

