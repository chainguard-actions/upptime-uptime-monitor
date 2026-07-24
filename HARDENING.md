<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.10

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.10** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or version strings instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the repository is compromised.

.github/workflows/build.yml:
  - uses: actions/checkout@v6
  - uses: actions/setup-node@v4
  - uses: stefanzweifel/git-auto-commit-action@v4.9.2

.github/workflows/node.yml:
  - uses: actions/checkout@v6
  - uses: actions/setup-node@v4

.github/workflows/release.yml:
  - uses: actions/checkout@v6
  - uses: actions/setup-node@v4

All of these should be pinned to their full 40-character commit SHA (e.g. uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4).

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:19`
- `.github/workflows/build.yml:31`
- `.github/workflows/node.yml:14`
- `.github/workflows/node.yml:19`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:18`

### missing-permissions (severity: medium)

build.yml and node.yml have no top-level 'permissions:' key and no job-level 'permissions:' key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. A minimal permissions block (e.g. 'permissions: read-all' or specific scopes) should be added to restrict the token's access.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 unpinned action references to full 40-character commit SHAs across build.yml, node.yml, and release.yml: actions/checkout@v6→d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v4→49933ea5288caeca8642d1e84afbd3f7d6820020, stefanzweifel/git-auto-commit-action@v4.9.2→be7095c202abcf573b09f20541e0ee2f6a3a9d9b. Added top-level permissions blocks to build.yml (contents: write, required for the git-auto-commit-action step) and node.yml (contents: read, minimal for checkout/build/test). release.yml already had a permissions block and was left unchanged except for pinning the action SHAs.

