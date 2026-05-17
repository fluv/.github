This is the GitHub Organisation repository for the "fluv" organisation.
The `profile/README.md` file is displayed on the organisation's profile page at https://github.com/fluv.
The `repos` directory contains [gh-infra](https://babarot.me/gh-infra/) code to declaratively manage the "fluv" GitHub organisation.

DeepSeek is configured to automatically run code reviews against PRs in all fluv GitHub repositories.
The main prompt is in `deepseek/prompt-template.md`. Do not rename this file; it is hardcoded into the orchestrating script.
The DeepSeek script is in the `zuzak/kube` repository.

Repositories are split across three manifest files by oversight level:

* `repos/fluv.yaml` — human-reviewed repos (`required_approving_review_count: 2`). Use for anything that reaches production infrastructure (e.g. `kube`).
* `repos/fluv-robot.yaml` — DS-reviewed repos (`required_approving_review_count: 1`). Use for low-risk app repos where robot review is sufficient to merge.
* `repos/fluv-archived.yaml` — archived public repos. No rulesets, no active development.

To add a new repo, append it under `repositories` in the appropriate file. Add a `spec:` block only when overriding a default.

