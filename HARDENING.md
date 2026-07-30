<!-- markdownlint-disable -->

# Hardening Report: poseidon--wait-for-status-checks/v0.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **poseidon--wait-for-status-checks/v0.7.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tag/version refs instead of pinned full-length SHA commit hashes, making the workflow vulnerable to supply-chain attacks if the referenced tag is moved or compromised.

.github/workflows/test.yaml:
  - Line 12: uses: actions/checkout@v7 (tag ref)
  - Line 25: uses: actions/checkout@v7 (tag ref)
  - Line 27: uses: actions/setup-node@v7.0.0 (version tag ref)

.github/workflows/summary.yaml:
  - Line 12: uses: actions/checkout@v7 (tag ref)

All should be pinned to a full 40-character hex commit SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.

Locations:

- `.github/workflows/test.yaml:12`
- `.github/workflows/test.yaml:25`
- `.github/workflows/test.yaml:27`
- `.github/workflows/summary.yaml:12`

### missing-permissions (severity: medium)

The workflow file .github/workflows/test.yaml has no top-level permissions: key, and neither of its jobs (test or dist) defines a permissions: block. Without explicit permissions, the workflow inherits the default repository permissions (which may include write access to contents and other scopes), violating the principle of least privilege. A top-level permissions: {} or per-job permissions blocks with only the required scopes should be added.

Locations:

- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full SHA commits: actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1 (in both test.yaml and summary.yaml), actions/setup-node@v7.0.0 → @820762786026740c76f36085b0efc47a31fe5020 (in test.yaml). Added top-level `permissions: {}` to test.yaml plus per-job `permissions: { contents: read }` for both the `test` and `dist` jobs. summary.yaml already had appropriate per-job permissions defined.

