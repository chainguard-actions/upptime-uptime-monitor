<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.44.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.44.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of pinned full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the tag is moved. Failing references:
- build.yml: actions/checkout@v6, actions/setup-node@v4, stefanzweifel/git-auto-commit-action@v4.9.2
- node.yml: actions/checkout@v6, actions/setup-node@v4
- release.yml: actions/checkout@v6, actions/setup-node@v4

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:18`
- `.github/workflows/build.yml:33`
- `.github/workflows/node.yml:13`
- `.github/workflows/node.yml:18`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:18`

### missing-permissions (severity: medium)

build.yml and node.yml have no top-level 'permissions:' key and no job-level 'permissions:' key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions. Each workflow should declare minimal required permissions.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 3 workflow files by pinning to full SHA hashes: actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2 → @be7095c202abcf573b09f20541e0ee2f6a3a9d9b. Added top-level permissions blocks to build.yml (contents: write, needed for git-auto-commit-action) and node.yml (contents: read, minimal for checkout only). release.yml already had a permissions block and only needed the action SHAs pinned.

