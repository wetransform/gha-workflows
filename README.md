# gha-workflows

Common GitHub Actions workflows used in wetransform projects.

## Jira release sync

`jira-release.yml` is a reusable workflow (`workflow_call`) that, on creation of a GitHub
release, creates (or reuses) a Jira **Fix Version** for the release and adds it to the
`fixVersions` field of every Jira issue referenced in the commits contained in the release.

- Issue keys are read from the commit message **body** (matching the footer convention for
  ticket references) and filtered to a single Jira project.
- Version creation is **idempotent**: an existing version with the same name is reused, so
  re-running the release event does not create duplicates.

### Enabling it

Repositories opt in via the `jira-release` preset in their `.wetf-repo.yml`
(managed by [`github-bootstrap`](https://github.com/wetransform/github-bootstrap), which
generates the caller workflow) — no per-repository workflow file is maintained by hand:

```yaml
presets:
  - jira-release
```

The Jira project defaults to `ING`. To sync a different project, override the input in
`.wetf-repo.yml`:

```yaml
workflows:
  jira:
    inputs:
      jira-project-key: SVC
```

### Inputs / secrets

| Input                 | Default                             | Description                                           |
| --------------------- | ----------------------------------- | ----------------------------------------------------- |
| `jira-project-key`    | `ING` (via preset)                  | Jira project whose issues are synced                  |
| `release-name-prefix` | repository name                     | Prefix for the version name, e.g. `my-service: 1.2.3` |
| `jira-base-url`       | `https://wetransform.atlassian.net` | Jira instance base URL                                |

Requires the organization secrets `JIRA_EMAIL` and `JIRA_API_TOKEN` (passed via
`secrets: inherit`).

### Credentials

Use a **dedicated Jira service account** for these secrets — not a personal user's API
token. This mirrors the GitHub side, where releases run under the "wetransform Bot" GitHub
App rather than a personal token.

- **Account:** a dedicated Atlassian account (e.g. `jira-bot@wetransform.de`). `JIRA_EMAIL`
  is that account's address, `JIRA_API_TOKEN` a **scoped API token with an expiry** created
  for it (avoid the classic unscoped token).
- **Least privilege:** grant only, and only on the synced projects (`ING` plus any
  overrides):
  - _Administer Projects_ — required to create/edit fix versions.
  - _Edit Issues_ — required to set the `fixVersions` field.
- **Secret scope:** store `JIRA_EMAIL` / `JIRA_API_TOKEN` as organization secrets limited to
  the opted-in repositories (not "all repositories"), and rotate the token periodically.

Using a personal token instead couples the automation to one person's account lifecycle,
attributes all changes to that person, and grants the workflow that person's full
permissions — all of which a dedicated account avoids.
