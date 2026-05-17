
This is a declarative GitHub repository configuration using the [`gh-infra`](https://babarot.me/gh-infra/) CLI.
It manages the repositories in this organisation, mainly to allow autonomous coding agents to create repositories without being able to delete them.

YAML manifests in `repos/` describe repository settings (visibility, labels, milestones, rulesets, merge strategies, Actions permissions).
Pushing to `main` triggers GitHub Actions to apply the manifests automatically.
Anything not listed is not managed. Configuration can drift between this repository and reality.

## Manifests

Repositories are split by oversight level:

| File | Oversight | `required_approving_review_count` | Use for |
|---|---|---|---|
| `fluv.yaml` | Human | 2 | Production infrastructure (e.g. `kube`) |
| `fluv-robot.yaml` | DeepSeek only | 1 | Low-risk app repos |
| `fluv-archived.yaml` | None | — | Archived public repos |

To add a repo, append it under `repositories` in the appropriate file. Only add a `spec:` block when overriding a default from that file's `RepositorySet`.

## Gotchas
* You cannot delete repositories. If a repository is no longer used, set `archived: true` and move it to `fluv-archived.yaml`.
* Private repos must set `rulesets: []` due to GitHub Free restrictions — they cannot use the shared RepositorySet rulesets.
