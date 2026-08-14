<!-- markdownlint-disable -->

# Hardening Report: Integral-Healthcare--robin-ai-reviewer/v2.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Integral-Healthcare--robin-ai-reviewer/v2.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use `uses:` references pinned to version tags instead of full 40-character SHA digests, making them vulnerable to supply-chain attacks if the upstream tag is moved or overwritten.

**docker.yml**: `actions/checkout@v6`, `docker/setup-qemu-action@v4.0.0`, `docker/setup-buildx-action@v4.0.0`, `docker/login-action@v4.1.0`, `docker/metadata-action@v6.0.0`, `docker/build-push-action@v7.1.0`

**labeler.yml**: `codelytv/pr-size-labeler@v1.10.4`

**lint.yml**: `actions/checkout@v6`, `ludeeus/action-shellcheck@2.0.0`, `hadolint/hadolint-action@v3.3.0`

**robin.yml**: `actions/checkout@v6`

**test.yml**: `actions/checkout@v6`

Locations:

- `.github/workflows/docker.yml:14`
- `.github/workflows/docker.yml:16`
- `.github/workflows/docker.yml:18`
- `.github/workflows/docker.yml:20`
- `.github/workflows/docker.yml:24`
- `.github/workflows/docker.yml:30`
- `.github/workflows/labeler.yml:8`
- `.github/workflows/lint.yml:7`
- `.github/workflows/lint.yml:11`
- `.github/workflows/lint.yml:19`
- `.github/workflows/lint.yml:23`
- `.github/workflows/robin.yml:18`
- `.github/workflows/test.yml:7`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and no job-level `permissions:` block on any of their jobs. Without explicit permissions, the default GITHUB_TOKEN permissions (which can be broad depending on repository settings) are used, violating the principle of least privilege.

Locations:

- `.github/workflows/labeler.yml:1`
- `.github/workflows/lint.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 13 unpinned action references across 5 workflow files by replacing tag/branch references with full 40-character SHA digests (preserving original tags as comments). Added top-level permissions blocks to labeler.yml (contents: read, pull-requests: write), lint.yml (contents: read), and test.yml (contents: read). docker.yml already had job-level permissions and robin.yml already had a top-level permissions block — only their action references needed pinning.

