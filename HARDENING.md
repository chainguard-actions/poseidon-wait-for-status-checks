<!-- markdownlint-disable -->

# Hardening Report: poseidon--wait-for-status-checks/v0.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **poseidon--wait-for-status-checks/v0.4.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable tags instead of pinned full-length SHA commits. An attacker who compromises the referenced action repository could push malicious code to the same tag and have it execute in this workflow.

Failing references in .github/workflows/test.yaml:
- uses: actions/checkout@v4 (line ~12)
- uses: actions/checkout@v4 (line ~23)
- uses: actions/setup-node@v4.0.2 (line ~26)

Failing references in .github/workflows/summary.yaml:
- uses: actions/checkout@v4 (line ~14)

All should be pinned to a full 40-character hex commit SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4

Locations:

- `.github/workflows/test.yaml:12`
- `.github/workflows/test.yaml:23`
- `.github/workflows/test.yaml:26`
- `.github/workflows/summary.yaml:14`

### missing-permissions (severity: medium)

The workflow file .github/workflows/test.yaml has no top-level `permissions:` block and neither of its jobs (`test`, `dist`) defines a job-level `permissions:` block. Without explicit permissions, the workflow runs with the default token permissions (which may include write access to repository contents), violating the principle of least privilege. A minimal permissions block such as `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving full commit SHAs: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4.0.2 → @60edb5dd545a775178f52524783378180af0d1f8. Applied to both test.yaml (3 references) and summary.yaml (1 reference). Added top-level `permissions: contents: read` block to test.yaml to enforce least privilege. summary.yaml already had job-level permissions defined.

