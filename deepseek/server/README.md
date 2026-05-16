DeepSeek review receiver
========================

GitHub webhook receiver that takes `pull_request` events from any fluv repo, builds the prompt by combining `../prompt-template.md` with the patch and repo contents, calls the DeepSeek API, and posts the result as a PR review under `fluv-deepseek[bot]`.

Deployed in `claude` namespace on the homelab cluster — see `fluv/kube/claude/webhook-receiver/`. Image: `ghcr.io/fluv/deepseek-receiver:main`.

## Layout

```
deepseek/server/
├── Dockerfile               # multi-stage build, python:3.13-slim base
├── pyproject.toml           # package metadata + runtime deps
├── README.md                # this file
└── src/
    └── deepseek_receiver/
        ├── __init__.py
        ├── __main__.py      # entry — runs server.main()
        └── server.py        # webhook handler, DS pipeline, prompt rendering
```

## Endpoints

- `POST /github/deepseek` — webhook target. HMAC-verified.
- `GET /healthz` — liveness.
- `GET /metrics` — Prometheus metrics.

## Environment

- `WEBHOOK_SECRET` (required) — HMAC shared secret with GitHub.
- `GITHUB_APP_ID`, `GITHUB_PRIVATE_KEY` — App credentials for posting reviews.
- `DEEPSEEK_API_KEY` — optional. Without it, DS reviews are skipped (the receiver still HMAC-verifies and logs).
- `BOT_LOGIN` — bot login used for "is this our own review?" checks. Default `deepseek-reviewer[bot]`; deployment overrides to `fluv-deepseek[bot]`.

## Image

Built and pushed to GHCR on every push to `main` and on tag `v*`. Multi-arch (`linux/arm64`, `linux/amd64`) — Pi runs arm64; future x86 nodes get amd64 from the same tag.

## History

Previously inlined as a 530-line Python script in a Kubernetes ConfigMap. Extracted to a real image so the source can be reviewed in version control and so DeepSeek itself can read its own runtime when reviewing changes to the prompt template.
