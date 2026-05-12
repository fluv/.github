
This is a declarative GitHub repository configuration using the [`gh-infra`](https://babarot.me/gh-infra/) CLI.
It manages the repositories in this organisation, mainly to allow autonomous coding agents to create repositories without being able to delete them.

YAML manifests in `repos/` describe repository settings (visibility, labels, milestones, rulesets, merge strategies, Actions permissions).
Pushing to `main` triggers GitHub Actions to apply the manifests automatically.
Anything not listed is not managed. Configuration can drift between this repository and reality.

## Manifests

All GitHub repositories under `fluv` are managed in `repos/fluv.yaml`.

## Gotchas
* You cannot delete repositories. If a repository is no longer used, set `archive: true`.
* If a repository is private, `rulesets: []` must be set due to GitHub Free restrictions. This means they can't use the `fluv` RepositorySet.
