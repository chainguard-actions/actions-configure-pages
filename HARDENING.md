<!-- markdownlint-disable -->

# Hardening Report: actions--configure-pages/v6.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--configure-pages/v6.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Commit any differences present in the dist/ directory' run: block in rebuild-dependabot-prs.yml directly interpolates ${{ github.ref_name }} into shell commands without routing through an env: variable or any quoting. The offending lines are: `echo "Pushing branch ${{ github.ref_name }}"` and `git push origin ${{ github.ref_name }}`. An attacker who controls the branch name (e.g. via a crafted Dependabot PR branch) could inject arbitrary shell commands.

Locations:

- `.github/workflows/rebuild-dependabot-prs.yml:44`
- `.github/workflows/rebuild-dependabot-prs.yml:45`

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (e.g. @v4, @v3, @v0.3.0, @0.0.3) instead of immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks if the referenced tags are moved or compromised. Affected references: check-dist.yml — actions/checkout@v4 (line 30), actions/setup-node@v4 (line 33); check-formatting.yml — actions/checkout@v4 (line 23), actions/setup-node@v4 (line 26); codeql-analysis.yml — actions/checkout@v4 (line 38), github/codeql-action/init@v3 (line 42), github/codeql-action/autobuild@v3 (line 50), github/codeql-action/analyze@v3 (line 62); draft-release.yml — actions/checkout@v4 (line 14); lint.yml — actions/checkout@v4 (line 22), actions/setup-node@v4 (line 25); publish-immutable-actions.yml — actions/checkout@v4 (line 10), actions/publish-immutable-action@0.0.3 (line 13); rebuild-dependabot-prs.yml — actions/checkout@v4 (line 21), actions/setup-node@v4 (line 26); release.yml — actions/publish-action@v0.3.0 (line 25); test.yml — actions/checkout@v4 (line 22), actions/setup-node@v4 (line 25).

Locations:

- `.github/workflows/check-dist.yml:30`
- `.github/workflows/check-dist.yml:33`
- `.github/workflows/check-formatting.yml:23`
- `.github/workflows/check-formatting.yml:26`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/codeql-analysis.yml:42`
- `.github/workflows/codeql-analysis.yml:50`
- `.github/workflows/codeql-analysis.yml:62`
- `.github/workflows/draft-release.yml:14`
- `.github/workflows/lint.yml:22`
- `.github/workflows/lint.yml:25`
- `.github/workflows/publish-immutable-actions.yml:10`
- `.github/workflows/publish-immutable-actions.yml:13`
- `.github/workflows/rebuild-dependabot-prs.yml:21`
- `.github/workflows/rebuild-dependabot-prs.yml:26`
- `.github/workflows/release.yml:25`
- `.github/workflows/test.yml:22`
- `.github/workflows/test.yml:25`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script injection in rebuild-dependabot-prs.yml by moving github.ref_name into an env: variable (REF_NAME) and referencing it safely as "$REF_NAME" in the shell script. Pinned all 18 unpinned action references across 8 workflow files (check-dist.yml, check-formatting.yml, codeql-analysis.yml, draft-release.yml, lint.yml, publish-immutable-actions.yml, rebuild-dependabot-prs.yml, test.yml, release.yml) to their full 40-character commit SHAs with original tags preserved as comments.

