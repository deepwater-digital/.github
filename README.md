# deepwater-digital/.github

Two jobs:

1. **Org profile.** `profile/README.md` is the page people see at github.com/deepwater-digital.
2. **Shared CI.** `.github/workflows/ci.yml` holds the checks every DWD repo runs. Each repo's own `ci.yml` calls it, pinned to a commit, and passes its own lint and test command. A fix here reaches every repo when Dependabot bumps the pin. `.github/workflows/db-live.yml` adds live database checks for repos with Supabase.

This repo is public, because GitHub only shows a profile from a public `.github` repo. Nothing secret goes in it.

Rules and the reasons behind them live in `dwd-kb`: CONVENTIONS.md section 11 and DECISIONS.md.
