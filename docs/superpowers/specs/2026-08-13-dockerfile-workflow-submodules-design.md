# Submodules support for the Dockerfile workflows

## Problem

The Gradle and mise workflow families accept a `submodules` input that is forwarded to
`actions/checkout`, together with an optional `GH_PAT` secret used to read private
submodule repositories. The Dockerfile workflows check out code but expose neither, so a
repository whose Docker build needs content from a submodule cannot use them.

## Scope

Three files:

- `.github/workflows/dockerfile.yml` — the reusable workflow that performs the checkout
- `.github/workflows/dockerfile-build.yml` — wrapper, builds without pushing
- `.github/workflows/dockerfile-publish.yml` — wrapper, builds and pushes

The other workflows that lack submodules support (`play-service.*`, `sbt-library.*`,
`mise-release.yml`, `scan-for-secrets.yml`) are deliberately out of scope for this change.

## Approach

Mirror the pattern already used by `gradle-library.yml`, `gradle-service.yml` and
`mise.yml` verbatim, rather than inventing a new one. The expression is proven in
production and consistency across the workflow families is worth more than any marginal
improvement.

Two approaches were rejected:

- **`submodules` input without `GH_PAT`.** `github.token` cannot read other private
  repositories, so this would only work for public submodules. wetransform submodules are
  typically private, so the feature would mostly not work.
- **Boolean input instead of string.** Cleaner typing, but it loses `recursive` and
  diverges from every other workflow in this repository.

## Design

### `dockerfile.yml`

Add the input under `on.workflow_call.inputs`, after `checkout-ref`, and the secret under
`on.workflow_call.secrets` between `DOCKER_HUB_PASSWORD` and
`SLACK_NOTIFICATIONS_BOT_TOKEN` — the same ordering as `mise.yml`:

```yaml
on:
  workflow_call:
    inputs:
      # ... existing inputs, ending with checkout-ref
      submodules:
        # see https://github.com/actions/checkout
        default: "false"
        type: string
    secrets:
      DOCKER_HUB_USERNAME:
      DOCKER_HUB_PASSWORD:
      GH_PAT:
      SLACK_NOTIFICATIONS_BOT_TOKEN:
```

Extend the existing checkout step in the `run` job:

```yaml
jobs:
  run:
    steps:
      - uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
        with:
          ref: ${{ inputs.checkout-ref }}
          submodules: ${{ inputs.submodules }}
          # do not persist credentials so a submodule PAT does not end up in
          # .git/config inside the Docker build context
          persist-credentials: false
          # provide a token in case submodules are enabled and private (secret is expected to be present)
          token: ${{ inputs.submodules != 'false' && secrets.GH_PAT || github.token }}
```

The input is a string rather than a boolean so that `"recursive"` remains available and so
that the `!= 'false'` comparison behaves as in the other workflows.

`GH_PAT` is optional. When a caller sets `submodules` but omits the secret,
`secrets.GH_PAT` is the empty string, the `&&` short-circuits, and `||` falls back to
`github.token`. The workflow then still works for public submodules instead of failing on
an empty token.

### `persist-credentials: false`

This line is new relative to the current `dockerfile.yml`, which leaves the setting at its
default of `true`.

Once a `GH_PAT` is passed to `actions/checkout`, the default behaviour writes that token
into `.git/config` in the workspace. The workspace is also the Docker build context, so a
`Dockerfile` containing `COPY . .` without a `.dockerignore` entry for `.git` would bake
the token into an image layer. Disabling credential persistence removes that path.

It is safe here because `dockerfile.yml` never pushes back via git — it builds, pushes to a
registry, and scans. Credentials are still available during checkout itself;
`persist-credentials` only controls what remains afterwards.

### `dockerfile-build.yml` and `dockerfile-publish.yml`

Both wrappers gain the same `submodules` input after `checkout-ref` and an optional
`GH_PAT` secret, each forwarded unchanged:

```yaml
jobs:
  build: # "publish" in dockerfile-publish.yml
    uses: ./.github/workflows/dockerfile.yml
    with:
      # ... existing inputs
      submodules: ${{ inputs.submodules }}
    secrets:
      # ... existing secrets
      GH_PAT: ${{ secrets.GH_PAT }}
```

In `dockerfile-publish.yml` the existing secrets are declared `required: true`. `GH_PAT`
stays optional, so publishing without submodules is unaffected.

## Compatibility

The change is additive. With `submodules` left at its default of `"false"`, the token
expression resolves to `github.token`, which is what `actions/checkout` uses by default
today. No existing caller needs to change.

The one behavioural difference for existing callers is `persist-credentials: false`: git
credentials are no longer left in the workspace after checkout. Nothing in `dockerfile.yml`
runs git afterwards, so this affects only what a build could theoretically read out of
`.git/config`.

## Verification

This repository has no test harness for its workflows. Verification is:

1. Run the repository's git hooks over the changed files: `mise x -- hk run pre-commit`.
   This runs Prettier, which reformats and validates the YAML.
2. Inspect the hook output to see whether an `actionlint` step ran on the three workflows.
   If it did not, run `actionlint` on them directly.
3. Confirm the token expression by inspection against `gradle-service.yml` and `mise.yml`,
   from which it is copied verbatim.

## Commit

A single `feat:` commit, per the repository's conventional-commits and semantic-release
setup. Adding a workflow input is a minor version bump.
