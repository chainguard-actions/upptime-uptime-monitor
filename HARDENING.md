<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.7** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files use mutable tag-based action references instead of pinned full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced tags are moved or overwritten.

Failing references:
- build.yml: `actions/checkout@v6`, `actions/setup-node@v4`, `stefanzweifel/git-auto-commit-action@v4.9.2`
- node.yml: `actions/checkout@v6`, `actions/setup-node@v4`
- release.yml: `actions/checkout@v6`, `actions/setup-node@v4`

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:20`
- `.github/workflows/build.yml:33`
- `.github/workflows/node.yml:13`
- `.github/workflows/node.yml:17`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:19`

### missing-permissions (severity: medium)

build.yml has no top-level `permissions:` key and no job-level `permissions:` key on its only job. This means the workflow runs with the default (overly broad) GITHUB_TOKEN permissions. Similarly, node.yml has no top-level or job-level `permissions:` block.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across build.yml, node.yml, and release.yml by pinning to full 40-character SHA hashes with original tags preserved as comments. Added top-level permissions blocks to build.yml (contents: write, required for git-auto-commit-action) and node.yml (contents: read, minimal for checkout-only workflow). release.yml already had a permissions block and was only updated for the unpinned-uses finding.

