# Emkraan / .github

Org-level configuration for the Emkraan GitHub organization.

## Renovate preset

`renovate.json` is a shared [Renovate preset config](https://docs.renovatebot.com/config-presets/)
that fleet repos extend:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>Emkraan/.github//renovate.json"]
}
```

### What it does

| Setting | Value |
|---|---|
| Schedule | Monday before 6 AM ET |
| PR hourly limit | 4 |
| PR concurrent limit | 8 |
| devDependency minor/patch | automerge |
| GitHub Actions minor/patch | automerge |
| Vulnerability alerts | enabled, labeled `security` |

Groups: `react-ecosystem`, `vite`, `typescript`, `fastapi-uvicorn`, `github-actions`.

### Activating Renovate

The [Renovate GitHub App](https://github.com/apps/renovate) must be installed on the
Emkraan org for PRs to be opened. Once installed, add a `renovate.json` extending this
preset to any repo you want covered.

## Reusable automerge workflow

`.github/workflows/reusable-automerge.yml` is the single fleet implementation of PR
auto-merge. Every Emkraan repo carries only a thin `automerge.yml` caller that declares
its own triggers (its CI workflow names differ) and delegates everything else:

```yaml
jobs:
  automerge:
    uses: Emkraan/.github/.github/workflows/reusable-automerge.yml@main
    with:
      runs-on: '["self-hosted","forge"]'           # private repos; public repos: '"ubuntu-latest"'
      app-client-id: ${{ vars.DEPLOY_APP_ID }}      # required: every repo merges as emkraan-deploy-bot
    secrets:
      app-private-key: ${{ secrets.DEPLOY_APP_PRIVATE_KEY }}
```

The full caller template (triggers, permissions, concurrency) and the readiness rule are
in Apollo `docs/standards/github-repo-standard.md` Section 8. Change merge behaviour here,
never in a caller.
