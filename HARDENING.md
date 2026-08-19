<!-- markdownlint-disable -->

# Hardening Report: poseidon--wait-for-status-checks/v0.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **poseidon--wait-for-status-checks/v0.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable tags instead of full 40-character SHA commit hashes, making the workflow vulnerable to supply-chain attacks if the tag is moved. Failing references in test.yaml: `actions/checkout@v4` (line 12), `actions/checkout@v4` (line 25), `actions/setup-node@v4.0.2` (line 28). Failing reference in summary.yaml: `actions/checkout@v4` (line 14).

Locations:

- `.github/workflows/test.yaml:12`
- `.github/workflows/test.yaml:25`
- `.github/workflows/test.yaml:28`
- `.github/workflows/summary.yaml:14`

### missing-permissions (severity: medium)

The workflow file .github/workflows/test.yaml has no top-level `permissions:` block and neither of its jobs (`test`, `dist`) defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to contents, etc.).

Locations:

- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving them to full 40-character SHA hashes: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 (applied in both test.yaml and summary.yaml), actions/setup-node@v4.0.2 → @60edb5dd545a775178f52524783378180af0d1f8 (applied in test.yaml). Added a top-level `permissions: contents: read` block to test.yaml to enforce least-privilege token access. The summary.yaml already had job-level permissions so no change was needed there.

