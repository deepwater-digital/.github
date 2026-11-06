# Architecture

## What is here

| Path | Job |
|---|---|
| `profile/README.md` | Org profile page |
| `.github/workflows/ci.yml` | Shared CI: the repo's own checks, then a secret scan of the whole history. Runs on calls from other repos and on this repo's own PRs, pushes and nightly |
| `.github/workflows/db-live.yml` | Live database checks for repos with Supabase: migration history, schema drift and the security advisor. Runs only on calls from other repos |
| `.github/dependabot.yml` | Weekly updates for pinned actions |

## Why

- **Shared workflows, called by pin.** Each repo keeps at most three small workflow files that call these, so a tool change is fixed once. Pinning to a commit means a change here reaches a repo only through a reviewed update PR.
- **Live checks are a separate workflow.** `ci.yml` passes no secrets, and that should stay true for code that installs packages. `db-live.yml` takes GitHub secrets from its caller, sets them only on the steps after install, and checks out only the base branch, so a PR's code never runs beside them.
- **A PR's drift check reads the base branch's migrations.** The PR's own migration is not applied until it merges, so comparing against it would always fail.
- **Secret scan without a third-party action.** The gitleaks binary is downloaded and checked against a pinned hash, so no outside code runs with the repo's token.
- **Public repo.** GitHub shows an org profile only from a public `.github` repo, so this repo stays public and holds nothing sensitive.

## Gotchas

- A calling repo's required check is named `checks / checks` (the caller's job, then this job), not `checks`.
- `db-live.yml` cannot run on this repo's own PRs, since it needs a caller's Supabase project. Test a change by pointing a caller's branch at the change's commit SHA.
- Callers should skip `db-live.yml` on pushes to `main`, since it would race the deploy, and on Dependabot PRs, which get no secrets. The nightly run covers both.
