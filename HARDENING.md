<!-- markdownlint-disable -->

# Hardening Report: Integral-Healthcare--robin-ai-reviewer/v.2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Integral-Healthcare--robin-ai-reviewer/v.2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across workflow files use mutable version tags instead of full 40-character SHA commit digests, making the workflows vulnerable to supply-chain attacks if a tag is moved or a dependency is compromised.

Failing references:
- docker.yml: `actions/checkout@v6`, `docker/setup-qemu-action@v4.0.0`, `docker/setup-buildx-action@v4.0.0`, `docker/login-action@v4.1.0`, `docker/metadata-action@v6.0.0`, `docker/build-push-action@v7.1.0`
- labeler.yml: `codelytv/pr-size-labeler@v1.10.4`
- lint.yml: `actions/checkout@v6`, `ludeeus/action-shellcheck@2.0.0`, `hadolint/hadolint-action@v3.3.0`
- robin.yml: `actions/checkout@v6`
- test.yml: `actions/checkout@v6`

Locations:

- `.github/workflows/docker.yml:18`
- `.github/workflows/docker.yml:20`
- `.github/workflows/docker.yml:22`
- `.github/workflows/docker.yml:24`
- `.github/workflows/docker.yml:29`
- `.github/workflows/docker.yml:37`
- `.github/workflows/labeler.yml:8`
- `.github/workflows/lint.yml:7`
- `.github/workflows/lint.yml:11`
- `.github/workflows/lint.yml:18`
- `.github/workflows/lint.yml:21`
- `.github/workflows/robin.yml:18`
- `.github/workflows/test.yml:8`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and no job-level `permissions:` on any of their jobs. Without explicit permissions, workflows inherit the default repository token permissions, which may be overly broad (write access to contents by default on many repositories).

- `labeler.yml`: no permissions at top level or job level
- `lint.yml`: no permissions at top level or job level (two jobs: shellcheck, hadolint)
- `test.yml`: no permissions at top level or job level

Locations:

- `.github/workflows/labeler.yml:1`
- `.github/workflows/lint.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 13 `uses:` references across docker.yml, labeler.yml, lint.yml, robin.yml, and test.yml to full 40-character SHA digests with original tags preserved as comments. Added top-level `permissions:` blocks to labeler.yml (contents: read, pull-requests: write), lint.yml (contents: read), and test.yml (contents: read). docker.yml and robin.yml already had permissions blocks and only needed SHA pinning.

