This is the GitHub Organisation repository for the "fluv" organisation.
The `profile/README.md` file is displayed on the organisation's profile page at https://github.com/fluv.
The `repos` directory contains [gh-infra](https://babarot.me/gh-infra/) code to declaratively manage the "fluv" GitHub organisation.

DeepSeek is configured to automatically run code reviews against PRs in all fluv GitHub repositories.
The main prompt is in `deepseek/prompt-template.md`. Do not rename this file; it is hardcoded into the orchestrating script.
The DeepSeek script is in the `zuzak/kube` repository.

To configure a ruleset to require DeepSeek review before Claude can merge it, set the required_approving_review_count to 1.
To configure a ruleset so that Claude can only merge if there is also approval from a human, set the required_approving_review_count to 2. 

