# spendwise-budget-service

Budgets and spending caps (budget_db). Consumes transaction events, publishes TransactionRejectedEvent on a HARD-cap breach, and serves budget summaries over gRPC (port 9090).

| | |
|---|---|
| Port | 8084 |
| Configuration | `budget-service*.yml` in [spendwise-config](https://github.com/PrathameshMax/spendwise-config) |
| Shared code | `com.spendwise:spendwise-parent / spendwise-common / spendwise-events` from [spendwise-shared](https://github.com/PrathameshMax/spendwise-shared) |
| Image | `ghcr.io/prathameshmax/spendwise-budget-service:<git-sha>` and `:main` |

## Build

Reading the shared packages needs a classic GitHub token with `read:packages` in
`GITHUB_PACKAGES_TOKEN` and a `<server>` with id `github` in `~/.m2/settings.xml`.

```bash
./mvnw -B verify
docker build --secret id=gh_token,env=GITHUB_PACKAGES_TOKEN -t spendwise-budget-service .
```

## CI

`.github/workflows/ci.yml`: every pull request builds, tests and builds the image; every push to
`main` also pushes the image to GHCR, tagged with the commit SHA and `main`. The repository secret
`PACKAGES_READ_TOKEN` (classic token, `read:packages`) lets CI read the shared packages.
