<!-- markdownlint-disable -->

# Hardening Report: poseidon--wait-for-status-checks/v0.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **poseidon--wait-for-status-checks/v0.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tags instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved. Failing references:
- `.github/workflows/test.yaml`: `actions/checkout@v3` (×2), `actions/setup-node@v3.6.0`
- `.github/workflows/summary.yaml`: `actions/checkout@v3`

Locations:

- `.github/workflows/test.yaml:12`
- `.github/workflows/test.yaml:24`
- `.github/workflows/test.yaml:27`
- `.github/workflows/summary.yaml:13`

### missing-permissions (severity: medium)

`.github/workflows/test.yaml` has no top-level `permissions:` key and neither of its jobs (`test`, `dist`) defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal permissions block should be added at the top level or per job.

Locations:

- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

1. Pinned all unpinned action references to full commit SHAs: actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 (×3, in both test.yaml and summary.yaml) and actions/setup-node@v3.6.0 → @64ed1c7eab4cce3362f8c340dee64e5eaeef8f7c (×1 in test.yaml). Original tags preserved as inline comments. 2. Added a top-level `permissions: contents: read` block to test.yaml to restrict the default token permissions to the minimum needed. summary.yaml already had explicit job-level permissions (contents: read, checks: read).

