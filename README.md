# Organization `.github` baseline

This repository centralizes default community health files, reusable workflow templates, and versioned organization ruleset recipes for public/open-source repositories.

## What applies automatically

When this repository is named `.github` and is **public**, GitHub can use the community health files in this repository as organization-wide defaults for repositories that do not define their own equivalents.

This includes files such as:

- `CONTRIBUTING.md`
- `SECURITY.md`
- `CODE_OF_CONDUCT.md`
- `SUPPORT.md`
- `.github/ISSUE_TEMPLATE/*`
- `.github/PULL_REQUEST_TEMPLATE.md`

Workflow templates in `workflow-templates/` are exposed to repositories in the organization when creating a new GitHub Actions workflow.

## What does NOT apply automatically

The JSON files in `rulesets/` are version-controlled recipes. Publishing this repository does **not** activate them.

Import them from:

`Organization settings -> Repository -> Rulesets -> New ruleset -> Import a ruleset`

Recommended order:

1. Import `rulesets/org-default-branch-baseline.json` and review the targeted repositories.
2. Create the custom repository property `security-tier` with values such as `standard` and `critical`.
3. Configure CodeQL on repositories that should be considered critical.
4. Set `security-tier=critical` on those repositories.
5. Import `rulesets/org-critical-projects.json`, review it, then enable it.
6. Add repository-specific required CI checks using one of the examples in `examples/`.

## Baseline policy

The default organization ruleset requires:

- pull requests before changes reach the default branch;
- one approval;
- stale approvals dismissed after new reviewable commits;
- review conversations resolved;
- linear history;
- no force pushes;
- no deletion of the default branch;
- squash or rebase merges only.

## Critical repository policy

Repositories marked with the custom property `security-tier=critical` additionally require:

- CODEOWNERS approval;
- CodeQL results;
- no High or Critical security alert before the protected branch can be updated.

The critical ruleset is shipped with `enforcement: disabled` intentionally. Enable it only after CodeQL and CODEOWNERS are configured for the targeted repositories.

## Required CI checks

Required status check names are intentionally not placed in the global baseline because check contexts vary by repository and language.

Use the examples in `examples/` as starting points and ensure the configured `context` values exactly match the GitHub Actions job/check names emitted by the repository.

## CODEOWNERS

A CODEOWNERS file in this central `.github` repository does not become a universal CODEOWNERS file for all repositories. Add a repository-specific `.github/CODEOWNERS` to every repository that is marked `security-tier=critical`.

See `examples/CODEOWNERS`.

## Dependabot

Dependabot configuration is repository-specific. The example in `examples/dependabot.yml` must be copied into each repository as `.github/dependabot.yml` and adjusted to its ecosystems.

## License

Community/default files in this repository are provided under the MIT License. Individual source repositories should carry their own appropriate license; a license stored here is not inherited by other repositories.
