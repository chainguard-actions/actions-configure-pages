<!-- markdownlint-disable -->

# Hardening Report: actions--configure-pages/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--configure-pages/v5.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (e.g. @v4, @v3, @v6, @v0.3.0) instead of immutable full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: check-dist.yml: actions/checkout@v4, actions/setup-node@v4; check-formatting.yml: actions/checkout@v4, actions/setup-node@v4; codeql-analysis.yml: actions/checkout@v4, github/codeql-action/init@v3, github/codeql-action/autobuild@v3, github/codeql-action/analyze@v3; draft-release.yml: actions/checkout@v4, release-drafter/release-drafter@v6; lint.yml: actions/checkout@v4, actions/setup-node@v4; rebuild-dependabot-prs.yml: actions/checkout@v4, actions/setup-node@v4; release.yml: actions/publish-action@v0.3.0; test.yml: actions/checkout@v4, actions/setup-node@v4.

Locations:

- `.github/workflows/check-dist.yml:30`
- `.github/workflows/check-formatting.yml:22`
- `.github/workflows/codeql-analysis.yml:36`
- `.github/workflows/draft-release.yml:13`
- `.github/workflows/lint.yml:22`
- `.github/workflows/rebuild-dependabot-prs.yml:21`
- `.github/workflows/release.yml:24`
- `.github/workflows/test.yml:21`

### script-injection (severity: high)

Sub-rule (a): In rebuild-dependabot-prs.yml, the expression ${{ github.ref_name }} is interpolated directly inside a run: shell command. The offending lines are: `echo "Pushing branch ${{ github.ref_name }}"` and `git push origin ${{ github.ref_name }}`. Although this workflow only triggers on pushes to dependabot branches (reducing attacker control), github.ref_name is still a GitHub context value that flows through YAML template substitution before the shell sees it. Any ${{ ... }} expression inside a run: block is a script-injection risk and must be moved to an env: variable and then double-quoted in the shell.

Locations:

- `.github/workflows/rebuild-dependabot-prs.yml:42`
- `.github/workflows/rebuild-dependabot-prs.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned all action references to full SHA digests with tag comments: actions/checkout@v4 → 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, github/codeql-action/{init,autobuild,analyze}@v3 → 02c5e83432fe5497fd85b873b6c9f16a8578e1d9, release-drafter/release-drafter@v6 → 6a93d829887aa2e0748befe2e808c66c0ec6e4c7, actions/publish-action@v0.3.0 → f784495ce78a41bac4ed7e34a73f0034015764bb. Fixed script injection in rebuild-dependabot-prs.yml by moving ${{ github.ref_name }} into an env: block as REF_NAME and referencing it as "$REF_NAME" in the shell script.

