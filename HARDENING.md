<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.42.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.42.6** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All uses: references in build.yml use mutable tags instead of pinned 40-character SHA commits: actions/checkout@v6 (line 14), actions/setup-node@v4 (line 20), stefanzweifel/git-auto-commit-action@v4.9.2 (line 35). These are vulnerable to supply-chain attacks if the tag is moved.

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:20`
- `.github/workflows/build.yml:35`

### unpinned-uses (severity: high)

All uses: references in node.yml use mutable tags instead of pinned 40-character SHA commits: actions/checkout@v6 (line 14), actions/setup-node@v4 (line 17).

Locations:

- `.github/workflows/node.yml:14`
- `.github/workflows/node.yml:17`

### unpinned-uses (severity: high)

All uses: references in release.yml use mutable tags instead of pinned 40-character SHA commits: actions/checkout@v6 (line 15), actions/setup-node@v4 (line 21).

Locations:

- `.github/workflows/release.yml:15`
- `.github/workflows/release.yml:21`

### missing-permissions (severity: medium)

build.yml has no top-level permissions: key and the single job (release) also has no job-level permissions: key. Without explicit permissions, the workflow inherits the repository default (typically write-all for private repos), granting overly broad access. Add a top-level permissions: block with minimal required scopes.

Locations:

- `.github/workflows/build.yml:1`

### missing-permissions (severity: medium)

node.yml has no top-level permissions: key and the single job (release) also has no job-level permissions: key. Without explicit permissions, the workflow inherits the repository default, granting overly broad access. Add a top-level permissions: block with minimal required scopes.

Locations:

- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files: (1) build.yml — pinned actions/checkout@v6 to SHA d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 to SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2 to SHA be7095c202abcf573b09f20541e0ee2f6a3a9d9b, and added top-level 'permissions: contents: write' (required for the git-auto-commit-action to push commits); (2) node.yml — pinned both actions to full SHAs and added top-level 'permissions: contents: read' (minimal needed for checkout on non-master branches); (3) release.yml — pinned both actions to full SHAs (permissions block already existed with contents: write and id-token: write). All mutable tag references replaced with immutable commit SHAs with tag comments for readability.

