<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.15

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.15** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files are pinned to mutable tags or version strings instead of immutable 40-character SHA commit hashes. This exposes the action to supply-chain attacks where a tag is moved to point to malicious code. Failing references:
- `.github/workflows/build.yml`: `actions/checkout@v6`, `actions/setup-node@v4`, `stefanzweifel/git-auto-commit-action@v4.9.2`
- `.github/workflows/node.yml`: `actions/checkout@v6`, `actions/setup-node@v4`
- `.github/workflows/release.yml`: `actions/checkout@v6`, `actions/setup-node@v4`

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:19`
- `.github/workflows/build.yml:32`
- `.github/workflows/node.yml:13`
- `.github/workflows/node.yml:17`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:19`

### missing-permissions (severity: medium)

`build.yml` and `node.yml` have no top-level `permissions:` key and their single job also has no `permissions:` key. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (often `write` for all scopes), granting broader access than necessary. Each workflow should declare minimal required permissions.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

**unpinned-uses** (7 locations):
- `actions/checkout@v6` → pinned to `@d23441a48e516b6c34aea4fa41551a30e30af803 # v6` in all 3 files
- `actions/setup-node@v4` → pinned to `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4` in all 3 files
- `stefanzweifel/git-auto-commit-action@v4.9.2` → pinned to `@be7095c202abcf573b09f20541e0ee2f6a3a9d9b # v4.9.2` in build.yml

**missing-permissions** (2 locations):
- `build.yml`: Added `permissions: contents: write` (needed for the git-auto-commit-action to push commits)
- `node.yml`: Added `permissions: contents: read` (only needs to read code for building and testing)
- `release.yml` already had a `permissions:` block (`contents: write`, `id-token: write`) so no change was needed there

