<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.11

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.11** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable tag-based action references instead of pinned full-length SHA commit hashes, making the workflows vulnerable to supply-chain attacks if the referenced tags are moved or overwritten.

build.yml:
  - uses: actions/checkout@v6 (line 15)
  - uses: actions/setup-node@v4 (line 21)
  - uses: stefanzweifel/git-auto-commit-action@v4.9.2 (line 36)

node.yml:
  - uses: actions/checkout@v6 (line 15)
  - uses: actions/setup-node@v4 (line 18)

release.yml:
  - uses: actions/checkout@v6 (line 15)
  - uses: actions/setup-node@v4 (line 21)

All references should be pinned to a full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:21`
- `.github/workflows/build.yml:36`
- `.github/workflows/node.yml:15`
- `.github/workflows/node.yml:18`
- `.github/workflows/release.yml:15`
- `.github/workflows/release.yml:21`

### missing-permissions (severity: medium)

build.yml and node.yml have no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, workflows inherit the default repository permissions (which may include write access), violating the principle of least privilege. Each workflow should declare a minimal top-level `permissions:` block (e.g. `permissions: read-all` or specific scopes) or per-job permissions.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 unpinned action references to full 40-character commit SHAs across build.yml, node.yml, and release.yml: actions/checkout@v6 → d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2 → be7095c202abcf573b09f20541e0ee2f6a3a9d9b. Added top-level permissions blocks to build.yml (contents: write, required for the git-auto-commit step) and node.yml (contents: read, only needs to read code for CI). release.yml already had a permissions block and was left unchanged except for pinning its action references.

