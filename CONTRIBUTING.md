# Contributing to AIGIS

## Branches
- main is protected. No direct pushes after bootstrap.
- One short-lived branch per task: feat/..., fix/..., docs/..., chore/...

## Commits
Conventional Commits, for example:
- feat(api): add tenant-scoped alert endpoint
- fix(ingestion): handle empty Wazuh payload
- docs(adr): add ADR-001 architecture decision

## Pull requests
- Link the issue the PR closes.
- CI must pass before merge.
- Describe how the change was tested.

## Security
- Never commit secrets, keys, tokens or .env files.
- Every data model carries tenant_id.
