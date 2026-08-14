<!-- markdownlint-disable -->

# Hardening Report: Integral-Healthcare--robin-ai-reviewer/v1.7.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Integral-Healthcare--robin-ai-reviewer/v1.7.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of immutable full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced action is compromised or its tag is moved.

- `.github/workflows/labeler.yml`: `codelytv/pr-size-labeler@v1.10.0` (tag)
- `.github/workflows/lint.yml`: `actions/checkout@v3` (tag), `ludeeus/action-shellcheck@master` (branch)
- `.github/workflows/robin.yml`: `actions/checkout@v4` (tag)

All `uses:` references should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/labeler.yml:9`
- `.github/workflows/lint.yml:7`
- `.github/workflows/lint.yml:10`
- `.github/workflows/robin.yml:14`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no job within any of them defines a job-level `permissions:` block. Without explicit permissions, workflows run with the default repository permissions (which can be `write-all` on some repositories), granting unnecessary access. Each workflow should declare the minimal permissions required (e.g. `permissions: contents: read` or `pull-requests: write` as appropriate).

Affected files:
- `.github/workflows/labeler.yml`
- `.github/workflows/lint.yml`
- `.github/workflows/robin.yml`

Locations:

- `.github/workflows/labeler.yml:1`
- `.github/workflows/lint.yml:1`
- `.github/workflows/robin.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. labeler.yml: Pinned codelytv/pr-size-labeler@v1.10.0 → SHA 56f6f0fc35c7cc0f72963b8467729e1120cb4bed. Added permissions: contents: read, pull-requests: write (needed to label PRs).

2. lint.yml: Pinned actions/checkout@v3 → SHA f43a0e5ff2bd294095638e18286ca9a3d1956744 and ludeeus/action-shellcheck@master → SHA 00b27aa7cb85167568cb48a3838b75f4265f2bca. Added permissions: contents: read (only needs to read code for linting).

3. robin.yml: Pinned actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5. Added permissions: contents: read, pull-requests: write (needed to post review comments on PRs).

