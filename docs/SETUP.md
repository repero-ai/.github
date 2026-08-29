# Setup checklist

## 1. Create the organization repository

Create a **public** repository named exactly `.github` in the organization and publish the contents of this template at its root.

## 2. Import the baseline ruleset

Organization settings -> Repository -> Rulesets -> New ruleset -> Import a ruleset

Import:

`rulesets/org-default-branch-baseline.json`

Review the target scope before creating it because the recipe targets all repositories and their default branch.

## 3. Create the security tier property

Create a custom organization repository property named:

`security-tier`

Recommended values:

- `standard`
- `critical`

Do not mark a repository `critical` until its CODEOWNERS and CodeQL configuration are ready.

## 4. Configure critical repositories

For each critical repository:

1. Add `.github/CODEOWNERS` based on `examples/CODEOWNERS`.
2. Enable CodeQL / code scanning.
3. Ensure CodeQL has successfully produced results on the default branch.
4. Set `security-tier=critical`.

Then import:

`rulesets/org-critical-projects.json`

The file is intentionally disabled. Review it and switch enforcement to Active only when the target repositories are ready.

## 5. Configure CI checks per repository

Copy/adapt one of:

- `examples/repo-required-checks-python.json`
- `examples/repo-required-checks-go.json`

The check contexts must exactly match the status check/job names produced by that repository.

Import repository-level rulesets under:

`Repository settings -> Rules -> Rulesets -> New ruleset -> Import a ruleset`

## 6. Add Dependabot per repository

Copy `examples/dependabot.yml` to `.github/dependabot.yml` in each repository and remove ecosystems that are not used.

## 7. Review merge settings

Because the organization baseline permits only `squash` and `rebase`, disable merge commits in repository merge settings for a consistent UI and contributor experience.
