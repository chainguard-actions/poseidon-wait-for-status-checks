<!-- markdownlint-disable -->

# Hardening Report: poseidon--wait-for-status-checks/v0.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **poseidon--wait-for-status-checks/v0.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files use mutable tag-based refs instead of pinned full SHA digests, making the action vulnerable to supply-chain attacks if the referenced action tag is moved or compromised.

Failing references in .github/workflows/test.yaml:
- `uses: actions/checkout@v4` (line 12)
- `uses: actions/checkout@v4` (line 23)
- `uses: actions/setup-node@v4.1.0` (line 26)

Failing references in .github/workflows/summary.yaml:
- `uses: actions/checkout@v4` (line 14)

All should be pinned to full 40-character commit SHAs, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/test.yaml:12`
- `.github/workflows/test.yaml:23`
- `.github/workflows/test.yaml:26`
- `.github/workflows/summary.yaml:14`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/test.yaml` has no top-level `permissions:` key, and neither of its two jobs (`test`, `dist`) defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. `write` on contents). A top-level `permissions: {}` or specific minimal scopes should be added.

Locations:

- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

1. Pinned all unpinned action references to full SHAs:
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4 (in both test.yaml and summary.yaml)
   - actions/setup-node@v4.1.0 → @39370e3970a6d050c480ffad4ff0ed4d3fdee5af # v4.1.0 (in test.yaml)
2. Added top-level `permissions: {}` to test.yaml to restrict the default token permissions to none, since the workflow only runs npm build/test commands and requires no GitHub API access.

