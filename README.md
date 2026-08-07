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
