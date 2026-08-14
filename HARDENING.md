<!-- markdownlint-disable -->

# Hardening Report: upptime--uptime-monitor/v1.43.14

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upptime--uptime-monitor/v1.43.14** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Failing references: build.yml — actions/checkout@v6, actions/setup-node@v4, stefanzweifel/git-auto-commit-action@v4.9.2; node.yml — actions/checkout@v6, actions/setup-node@v4; release.yml — actions/checkout@v6, actions/setup-node@v4.

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:19`
- `.github/workflows/build.yml:34`
- `.github/workflows/node.yml:13`
- `.github/workflows/node.yml:17`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:19`

### missing-permissions (severity: medium)

build.yml has no top-level permissions: key and its only job ('release') also has no job-level permissions: key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to contents by default). Minimal explicit permissions should be declared.

Locations:

- `.github/workflows/build.yml:1`

### missing-permissions (severity: medium)

node.yml has no top-level permissions: key and its only job ('release') also has no job-level permissions: key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. Minimal explicit permissions should be declared.

Locations:

- `.github/workflows/node.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 findings across 3 workflow files:

1. **unpinned-uses** (build.yml, node.yml, release.yml): Pinned all 7 action references to full 40-character commit SHAs:
   - actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 # v6
   - actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4
   - stefanzweifel/git-auto-commit-action@v4.9.2 → @be7095c202abcf573b09f20541e0ee2f6a3a9d9b # v4.9.2

2. **missing-permissions** (build.yml): Added top-level `permissions: contents: write` — write access is needed because the workflow commits built artifacts via git-auto-commit-action.

3. **missing-permissions** (node.yml): Added top-level `permissions: contents: read` — minimal read-only permission sufficient for a build/test-only workflow on non-master branches.

release.yml already had a permissions block (contents: write, id-token: write) so no change was needed there.

