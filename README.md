# .github

Organization-level automation, prompts, and skills.

## Reusable Workflows

- `.github/workflows/remove-blocking-on-close.yml`: org-facing reusable entrypoint (called by repos).
- `.github/workflows/remove-blocking-on-close-action.yml`: internal implementation workflow called by the entrypoint.
- `.github/workflows/auto-remove-blocking-on-close.yml`: automatic caller in this repo (triggers on issue close).

### Caller Example

Use from any repository in the org:

```yaml
name: Auto remove blocking relationships on issue close

on:
	issues:
		types:
			- closed

jobs:
	remove-blocking-links:
		uses: askclara/.github/.github/workflows/remove-blocking-on-close.yml@main
		with:
			issue-number: ${{ github.event.issue.number }}
			repo: ${{ github.repository }}
		# Required org PAT secret
		secrets:
			AC_AUTOMATION_PAT: ${{ secrets.AC_AUTOMATION_PAT }}
```
