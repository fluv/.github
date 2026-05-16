DeepSeek PR Review
==================

This directory governs an AI code review on pull requests using DeepSeek.

The `prompt-template.md` in this directory is the canonical base template and always loads.

To add repo-specific guidance (project conventions, filing instructions, areas to focus on),
create files in (by default) `.github/deepseek`.
Their contents are concatenated in sort order and injected at the `{{REPO_CONTEXT}}`
placeholder in the base template. If no files match, `{{REPO_CONTEXT}}` is replaced with an empty string.

Placeholders substituted at runtime:

| Placeholder | Content |
|---|---|
| `{{REPO_CONTEXT}}` | Concatenated repo-specific context files (empty if none match) |
| `{{TRIGGER}}` | `first-or-push` or `recheck` |
| `{{PR_LABELS}}` | Comma-separated PR labels |
| `{{PRIOR_REVIEW_THREAD}}` | All PR comments in chronological order |
| `{{REPO_CONTENTS}}` | Full HEAD snapshot (minus `exclude-paths`) |
| `{{PATCH}}` | `git diff` against the base branch |

For more information and to see the underlying code see [`server/`](./server/). The image is deployed via the `webhook-receiver` Deployment in [fluv/kube](https://github.com/fluv/kube/tree/main/claude/webhook-receiver).
