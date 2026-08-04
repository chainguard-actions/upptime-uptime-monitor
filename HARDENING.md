<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.42.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.42.5** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in workflow files use mutable tags instead of full 40-character SHA commit digests, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

- `.github/workflows/build.yml`: `actions/checkout@v6`, `actions/setup-node@v4`, `stefanzweifel/git-auto-commit-action@v4.9.2`
- `.github/workflows/node.yml`: `actions/checkout@v6`, `actions/setup-node@v4`
- `.github/workflows/release.yml`: `actions/checkout@v6`, `actions/setup-node@v4`

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:17`
- `.github/workflows/build.yml:31`
- `.github/workflows/node.yml:13`
- `.github/workflows/node.yml:17`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:17`

### missing-permissions (severity: medium)

Workflow files `build.yml` and `node.yml` have no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions. Each workflow should declare minimal required permissions explicitly.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 action references to full SHA digests: actions/checkout@v6 → d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2 → be7095c202abcf573b09f20541e0ee2f6a3a9d9b. Added top-level permissions blocks: build.yml gets 'contents: write' (required for the git-auto-commit-action to push commits), node.yml gets 'contents: read' (minimal for checkout on non-master branches). release.yml already had explicit permissions (contents: write, id-token: write) and only needed action SHA pinning.

