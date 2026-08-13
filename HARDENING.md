<!-- markdownlint-disable -->

# Hardening Report: BobAnkh--auto-generate-changelog/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **BobAnkh--auto-generate-changelog/v1.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of pinned 40-character commit SHAs. This exposes the pipeline to supply-chain attacks where a compromised or updated tag could execute malicious code.

Failing references:
- changelog.yml: `actions/checkout@v4`, `BobAnkh/auto-generate-changelog@master`
- contributors.yml: `actions/checkout@v2`, `BobAnkh/add-contributors@master`
- lock.yml: `dessant/lock-threads@v4`
- mdl.yml: `actions/checkout@v2`, `nosborn/github-action-markdown-cli@v1.1.1`
- pytest.yml: `actions/checkout@v4`, `actions/setup-python@v5`, `codecov/codecov-action@v1`, `actions/checkout@v2`
- test.yml: `actions/checkout@v2`, `BobAnkh/auto-generate-changelog@dev`

All `uses:` references should be pinned to a full 40-character hex commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/changelog.yml:16`
- `.github/workflows/changelog.yml:18`
- `.github/workflows/contributors.yml:8`
- `.github/workflows/contributors.yml:9`
- `.github/workflows/lock.yml:13`
- `.github/workflows/mdl.yml:9`
- `.github/workflows/mdl.yml:10`
- `.github/workflows/pytest.yml:14`
- `.github/workflows/pytest.yml:17`
- `.github/workflows/pytest.yml:27`
- `.github/workflows/pytest.yml:36`
- `.github/workflows/test.yml:12`
- `.github/workflows/test.yml:14`

### missing-permissions (severity: medium)

None of the 6 workflow files define a top-level `permissions:` key, and no job within any of these files defines job-level `permissions:` either. Without explicit permission scoping, workflows run with the default repository permissions (which can be `write` for the GITHUB_TOKEN), granting broader access than necessary and violating the principle of least privilege. Each workflow should declare the minimal permissions required (e.g. `permissions: contents: read`).

Locations:

- `.github/workflows/changelog.yml:1`
- `.github/workflows/contributors.yml:1`
- `.github/workflows/lock.yml:1`
- `.github/workflows/mdl.yml:1`
- `.github/workflows/pytest.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 workflow files:

1. **unpinned-uses**: Replaced all mutable tag/branch references with pinned 40-character commit SHAs:
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262
   - actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e
   - BobAnkh/auto-generate-changelog@master → @dcade405ec2e83773ccc70fec02574ec835c2cf8
   - BobAnkh/auto-generate-changelog@dev → @7fb9808705671023e7c526b531ab1a61c0d41ca6
   - BobAnkh/add-contributors@master → @8bd02ce413bee09225ff2f27791ecb78ea273743
   - dessant/lock-threads@v4 → @be8aa5be94131386884a6da4189effda9b14aa21
   - nosborn/github-action-markdown-cli@v1.1.1 → @9fc95163471d6460c35cccad13645912aaa25d5f
   - actions/setup-python@v5 → @a26af69be951a213d495a4c3e4e4022e16d87065
   - codecov/codecov-action@v1 → @29386c70ef20e286228c72b668a06fd0e8399192

2. **missing-permissions**: Added top-level `permissions:` blocks to all 6 workflows with minimal required permissions:
   - changelog.yml: `contents: write` (needs to commit changelog)
   - contributors.yml: `contents: write` (needs to commit README)
   - lock.yml: `issues: write, pull-requests: write` (needs to lock threads)
   - mdl.yml: `contents: read` (read-only lint check)
   - pytest.yml: `contents: read` (read-only test/build)
   - test.yml: `contents: write` (needs to commit changelog)

