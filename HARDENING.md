<!-- markdownlint-disable -->

# Hardening Report: actions--configure-pages/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--configure-pages/v5.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: `${{ github.ref_name }}` is directly interpolated inside a `run:` shell command string. This allows YAML template substitution to inject arbitrary content into the shell before it is executed. Offending lines:
  - `echo "Pushing branch ${{ github.ref_name }}"`
  - `git push origin ${{ github.ref_name }}`
Fix: move the value into an `env:` variable and double-quote it in the shell: `env: { REF_NAME: "${{ github.ref_name }}" }` then use `"$REF_NAME"` in the script.

Locations:

- `.github/workflows/rebuild-dependabot-prs.yml:43`
- `.github/workflows/rebuild-dependabot-prs.yml:44`

### unpinned-uses (severity: high)

All `uses:` references across workflow files use mutable tag-based refs instead of immutable full 40-character SHA commit hashes, making the workflows vulnerable to supply-chain attacks if the referenced tags are moved or compromised. Failing references include:
- `actions/checkout@v4` (check-dist.yml, check-formatting.yml, lint.yml, rebuild-dependabot-prs.yml, test.yml, draft-release.yml)
- `actions/setup-node@v4` (check-dist.yml, check-formatting.yml, lint.yml, rebuild-dependabot-prs.yml, test.yml)
- `github/codeql-action/init@v3` (codeql-analysis.yml)
- `github/codeql-action/autobuild@v3` (codeql-analysis.yml)
- `github/codeql-action/analyze@v3` (codeql-analysis.yml)
- `release-drafter/release-drafter@v6` (draft-release.yml)
- `actions/publish-action@v0.3.0` (release.yml)

Locations:

- `.github/workflows/check-dist.yml:28`
- `.github/workflows/check-dist.yml:32`
- `.github/workflows/check-formatting.yml:21`
- `.github/workflows/check-formatting.yml:26`
- `.github/workflows/codeql-analysis.yml:32`
- `.github/workflows/codeql-analysis.yml:37`
- `.github/workflows/codeql-analysis.yml:47`
- `.github/workflows/codeql-analysis.yml:55`
- `.github/workflows/draft-release.yml:12`
- `.github/workflows/draft-release.yml:13`
- `.github/workflows/lint.yml:21`
- `.github/workflows/lint.yml:26`
- `.github/workflows/rebuild-dependabot-prs.yml:21`
- `.github/workflows/rebuild-dependabot-prs.yml:26`
- `.github/workflows/release.yml:22`
- `.github/workflows/test.yml:21`
- `.github/workflows/test.yml:26`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in rebuild-dependabot-prs.yml by moving `${{ github.ref_name }}` into an env var `REF_NAME` and using `"$REF_NAME"` in the shell. Fixed unpinned-uses across all 7 workflow files: actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, github/codeql-action/{init,autobuild,analyze}@v3 → SHA b7351df727350dca84cb9d725d57dcf5bc82ba26, release-drafter/release-drafter@v6 → SHA 6a93d829887aa2e0748befe2e808c66c0ec6e4c7, actions/publish-action@v0.3.0 → SHA f784495ce78a41bac4ed7e34a73f0034015764bb. Original tags preserved as inline comments.

