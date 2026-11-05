# Architecture

## What is here

| Path | Job |
|---|---|
| `profile/README.md` | Org profile page |
| `.github/workflows/ci.yml` | Shared CI: the repo's own checks, then a secret scan of the whole history. Runs on calls from other repos and on this repo's own PRs, pushes and nightly |
| `.github/dependabot.yml` | Weekly updates for pinned actions |

## Why

- **One shared workflow, called by pin.** Each repo keeps at most three small workflow files that call this one, so a tool change is fixed once. Pinning to a commit means a change here reaches a repo only through a reviewed update PR.
- **Secret scan without a third-party action.** The gitleaks binary is downloaded and checked against a pinned hash, so no outside code runs with the repo's token.
- **Public repo.** GitHub shows an org profile only from a public `.github` repo, so this repo stays public and holds nothing sensitive.

## Gotchas

- A calling repo's required check is named `checks / checks` (the caller's job, then this job), not `checks`.
