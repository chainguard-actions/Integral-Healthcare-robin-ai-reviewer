<!-- markdownlint-disable -->

# Hardening Report: Integral-Healthcare--robin-ai-reviewer/v2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Integral-Healthcare--robin-ai-reviewer/v2.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags or semver strings instead of pinned 40-character commit SHAs. This exposes the pipeline to supply-chain attacks if a tag is moved or a dependency is compromised.

docker.yml: actions/checkout@v6, docker/setup-qemu-action@v4.0.0, docker/setup-buildx-action@v4.0.0, docker/login-action@v4.1.0, docker/metadata-action@v6.0.0, docker/build-push-action@v7.1.0
labeler.yml: codelytv/pr-size-labeler@v1.10.4
lint.yml: actions/checkout@v6, ludeeus/action-shellcheck@2.0.0, hadolint/hadolint-action@v3.3.0
robin.yml: actions/checkout@v6
test.yml: actions/checkout@v6

Locations:

- `.github/workflows/docker.yml:14`
- `.github/workflows/docker.yml:16`
- `.github/workflows/docker.yml:19`
- `.github/workflows/docker.yml:22`
- `.github/workflows/docker.yml:27`
- `.github/workflows/docker.yml:34`
- `.github/workflows/labeler.yml:9`
- `.github/workflows/lint.yml:8`
- `.github/workflows/lint.yml:11`
- `.github/workflows/lint.yml:19`
- `.github/workflows/lint.yml:22`
- `.github/workflows/robin.yml:21`
- `.github/workflows/test.yml:8`

### missing-permissions (severity: medium)

Three workflow files have no top-level 'permissions:' key and no job-level 'permissions:' key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, violating the principle of least privilege.

- labeler.yml: no permissions block at top level or job level
- lint.yml: no permissions block at top level or job level
- test.yml: no permissions block at top level or job level

Locations:

- `.github/workflows/labeler.yml:1`
- `.github/workflows/lint.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 9 unique action references (13 total occurrences) to full 40-character commit SHAs with original tags preserved as comments. Added top-level permissions blocks to labeler.yml (contents: read, pull-requests: write), lint.yml (contents: read), and test.yml (contents: read). docker.yml and robin.yml already had permissions blocks and only needed SHA pinning.

