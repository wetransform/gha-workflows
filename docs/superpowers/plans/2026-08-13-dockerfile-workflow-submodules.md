# Dockerfile Workflow Submodules Support — Implementation Plan

> **Status:** Executed. Completed in commit `3cdd187` (`feat(dockerfile): support submodules configuration`).

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Let callers of the Dockerfile reusable workflows check out git submodules, including private ones, by adding a `submodules` input and an optional `GH_PAT` secret.

**Architecture:** Copy the pattern already used by `gradle-library.yml`, `gradle-service.yml` and `mise.yml` verbatim: a string `submodules` input forwarded to `actions/checkout`, plus a conditional `token:` expression that swaps in `GH_PAT` only when submodules are enabled. The two wrapper workflows declare the same input and secret and pass them straight through. `dockerfile.yml` also sets `persist-credentials: false`, so the PAT (or default `GITHUB_TOKEN`) stays out of the Docker build context; `gradle-library.yml` and `gradle-service.yml` already carry that same setting, but for an unrelated reason (it clashes with the semantic-release action), and `mise.yml` remains the outlier that still lacks it.

**Tech Stack:** GitHub Actions reusable workflows (YAML), `actions/checkout` v7, validated by `actionlint` 1.7.12 and Prettier 3.9.6, both run via the repo's `hk` pre-commit hook.

**Spec:** `docs/superpowers/specs/2026-08-13-dockerfile-workflow-submodules-design.md`

## Global Constraints

- The `submodules` input is **type `string`**, default the quoted string `"false"` — not a boolean. A boolean would lose the `"recursive"` option that string inputs support, and would diverge from every other workflow in this repository, which all use a string.
- `GH_PAT` is **always optional** — never add `required: true` to it, in any of the three files. `dockerfile-publish.yml` marks its other secrets required; `GH_PAT` must not follow suit.
- Do **not** change the pinned `actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1` SHA or any other action pin.
- Comment lines are copied verbatim from the snippets below, including the `# see https://github.com/actions/checkout` line above the input.
- The `submodules` input goes immediately after `checkout-ref` in all three files. `GH_PAT` goes immediately after `DOCKER_HUB_PASSWORD` in all three files.
- Conventional commits are enforced (semantic-release). This change ships as a single `feat:` commit.
- Out of scope: `play-service.*`, `sbt-library.*`, `mise-release.yml`, `scan-for-secrets.yml`, and the managed `tf-*` workflows. Do not touch them.

**Reading the YAML snippets below:** every snippet is rooted at the top level of the file (`on:` or `jobs:`) and shows surrounding context, so the indentation you see is the indentation to write. Lines marked `# ... unchanged` stand for existing content you leave alone — do not delete it, and do not paste that comment into the file.

---

## File Structure

Three files, all under `.github/workflows/`. No new files.

| File                     | Responsibility                                                                 | Change                                                                                                           |
| ------------------------ | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| `dockerfile.yml`         | The reusable workflow that actually performs the checkout and the Docker build | Declare `submodules` input + `GH_PAT` secret; wire both into the checkout step; add `persist-credentials: false` |
| `dockerfile-build.yml`   | Thin wrapper — builds without pushing                                          | Declare `submodules` + `GH_PAT`; forward both                                                                    |
| `dockerfile-publish.yml` | Thin wrapper — builds and pushes                                               | Declare `submodules` + `GH_PAT`; forward both                                                                    |

The wrappers are near-identical; the only differences are `push`/`notify-failure` values and that `dockerfile-publish.yml` also handles `SLACK_NOTIFICATIONS_BOT_TOKEN` and marks its secrets required.

---

## How this is tested

This repo has **no unit-test harness for workflows**. The real test is `actionlint`, which validates calls to local reusable workflows: if a wrapper passes an input or secret that `dockerfile.yml` does not declare, actionlint fails with a specific error. That gives a genuine red/green cycle — edit the wrappers first (the "test"), watch actionlint fail, then implement `dockerfile.yml` and watch it pass.

Both tools are already configured in `hk.pkl` via `wetransform/hk-config`. actionlint's glob covers `.github/workflows/*.yml` and excludes only `tf-*`, so all three files are checked.

The exact commands and their expected output below were verified against a scratch copy of these three workflows before this plan was written. The output is real, not illustrative.

---

## Task 1: Add submodules support to the Dockerfile workflows

**Files:**

- Modify: `.github/workflows/dockerfile-build.yml` — inputs block, `with:` block, `secrets:` blocks
- Modify: `.github/workflows/dockerfile-publish.yml` — inputs block, `with:` block, `secrets:` blocks
- Modify: `.github/workflows/dockerfile.yml` — inputs block, `secrets:` block, checkout step
- Test: none — validated by `actionlint`, see "How this is tested" above

**Interfaces:**

- Consumes: nothing from earlier tasks (this is the only task).
- Produces: `dockerfile.yml` gains input `submodules` (string, default `"false"`) and optional secret `GH_PAT`. Both wrappers expose the identical input and secret names to their own callers.

---

- [x] **Step 1: Confirm the starting point is green**

Run:

```bash
mise x actionlint@1.7.12 shellcheck@0.11.0 -- actionlint
```

Expected: exit 0, no output. If there are pre-existing errors in unrelated workflows, note them — they are not yours to fix, but you need to know them so you can tell them apart from the errors Step 3 expects.

---

- [x] **Step 2: Write the failing test — declare and forward in `dockerfile-build.yml`**

Edit `.github/workflows/dockerfile-build.yml`. Declare the input and the secret:

```yaml
on:
  workflow_call:
    inputs:
      # ... image, tag, context unchanged
      checkout-ref:
        description: Custom ref to check out
        type: string
        default: ""
      submodules:
        # see https://github.com/actions/checkout
        default: "false"
        type: string
    secrets:
      DOCKER_HUB_USERNAME:
      DOCKER_HUB_PASSWORD:
      GH_PAT:
```

Then forward both to the called workflow:

```yaml
jobs:
  build:
    uses: ./.github/workflows/dockerfile.yml
    with:
      # ... image, tag, context unchanged
      checkout-ref: ${{ inputs.checkout-ref }}
      submodules: ${{ inputs.submodules }}
      push: false
      notify-failure: false
    secrets:
      DOCKER_HUB_USERNAME: ${{ secrets.DOCKER_HUB_USERNAME }}
      DOCKER_HUB_PASSWORD: ${{ secrets.DOCKER_HUB_PASSWORD }}
      GH_PAT: ${{ secrets.GH_PAT }}
```

Leave the existing `# secrets: inherit` comment block above `secrets:` in place.

---

- [x] **Step 3: Write the failing test — declare and forward in `dockerfile-publish.yml`**

Edit `.github/workflows/dockerfile-publish.yml`. This file marks its secrets `required: true`; `GH_PAT` does **not** get that, and it sits between `DOCKER_HUB_PASSWORD` and `SLACK_NOTIFICATIONS_BOT_TOKEN`:

```yaml
on:
  workflow_call:
    inputs:
      # ... image, tag, context unchanged
      checkout-ref:
        description: Custom ref to check out
        type: string
        default: ""
      submodules:
        # see https://github.com/actions/checkout
        default: "false"
        type: string
    secrets:
      DOCKER_HUB_USERNAME:
        required: true
      DOCKER_HUB_PASSWORD:
        required: true
      GH_PAT:
      SLACK_NOTIFICATIONS_BOT_TOKEN:
        required: true
```

Then forward both:

```yaml
jobs:
  publish:
    uses: ./.github/workflows/dockerfile.yml
    with:
      # ... image, tag, context unchanged
      checkout-ref: ${{ inputs.checkout-ref }}
      submodules: ${{ inputs.submodules }}
      push: true
      notify-failure: true
    secrets:
      DOCKER_HUB_USERNAME: ${{ secrets.DOCKER_HUB_USERNAME }}
      DOCKER_HUB_PASSWORD: ${{ secrets.DOCKER_HUB_PASSWORD }}
      GH_PAT: ${{ secrets.GH_PAT }}
      SLACK_NOTIFICATIONS_BOT_TOKEN: ${{ secrets.SLACK_NOTIFICATIONS_BOT_TOKEN }}
```

---

- [x] **Step 4: Run actionlint to verify it fails**

Run:

```bash
mise x actionlint@1.7.12 shellcheck@0.11.0 -- actionlint
```

Expected: exit 1, with exactly these four errors (line numbers may shift by a line or two):

```
.github/workflows/dockerfile-build.yml:37:7: input "submodules" is not defined in "./.github/workflows/dockerfile.yml" reusable workflow. defined inputs are "checkout-ref", "context", "image", "notify-failure", "push", "tag" [workflow-call]
.github/workflows/dockerfile-build.yml:46:7: secret "GH_PAT" is not defined in "./.github/workflows/dockerfile.yml" reusable workflow. defined secrets are "DOCKER_HUB_PASSWORD", "DOCKER_HUB_USERNAME", "SLACK_NOTIFICATIONS_BOT_TOKEN" [workflow-call]
.github/workflows/dockerfile-publish.yml:41:7: input "submodules" is not defined in "./.github/workflows/dockerfile.yml" reusable workflow. defined inputs are "checkout-ref", "context", "image", "notify-failure", "push", "tag" [workflow-call]
.github/workflows/dockerfile-publish.yml:50:7: secret "GH_PAT" is not defined in "./.github/workflows/dockerfile.yml" reusable workflow. defined secrets are "DOCKER_HUB_PASSWORD", "DOCKER_HUB_USERNAME", "SLACK_NOTIFICATIONS_BOT_TOKEN" [workflow-call]
```

This is the red phase, and it is the reason for editing the wrappers first — it proves the wrappers really are wired to `dockerfile.yml` and that actionlint is watching.

If you instead see errors of the form `property "submodules" is not defined in object type {...}` or `property "gh_pat" is not defined in object type {...}`, you added the pass-through in the `with:`/`secrets:` block but forgot to **declare** the input or secret in that same wrapper's own `on.workflow_call` block. Go back and add the declaration.

---

- [x] **Step 5: Implement — declare the input and secret in `dockerfile.yml`**

Edit `.github/workflows/dockerfile.yml`. Add `submodules` after `checkout-ref`, and `GH_PAT` between `DOCKER_HUB_PASSWORD` and `SLACK_NOTIFICATIONS_BOT_TOKEN` — the same ordering `mise.yml` uses:

```yaml
on:
  workflow_call:
    inputs:
      # ... notify-failure, image, tag, push, context unchanged
      checkout-ref:
        description: Custom ref to check out
        type: string
        default: ""
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

---

- [x] **Step 6: Implement — wire the checkout step in `dockerfile.yml`**

The checkout step is the first step of the `run` job and currently has only `ref:` under `with:`. Add four lines:

```yaml
jobs:
  run:
    runs-on: ${{ vars.CUSTOM_RUNNER || 'ubuntu-latest' }}
    steps:
      - uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
        with:
          ref: ${{ inputs.checkout-ref }}
          submodules: ${{ inputs.submodules }}
          # so no checkout credential (PAT or GITHUB_TOKEN) ends up in
          # .git/config inside the Docker build context
          persist-credentials: false
          # provide a token in case submodules are enabled and private (secret is expected to be present)
          token: ${{ inputs.submodules != 'false' && secrets.GH_PAT || github.token }}
```

Three things worth understanding rather than pattern-matching:

1. `persist-credentials: false` is the security hardening. Without it, `actions/checkout` writes the token into `.git/config` in the workspace — and the workspace is the Docker build context, so a `Dockerfile` with `COPY . .` and no `.dockerignore` entry for `.git` would bake the PAT into an image layer. It is safe to disable here because this workflow never pushes back via git; credentials are still used during the checkout itself.
2. The `token:` expression falls back on purpose. When `GH_PAT` is not supplied, `secrets.GH_PAT` is the empty string, so `&&` short-circuits and `||` yields `github.token`. Submodule checkout then still works for public submodules instead of failing on an empty token.
3. Do not "simplify" the expression to `secrets.GH_PAT || github.token`. The `inputs.submodules != 'false'` guard is what keeps the default path using `github.token`, preserving today's behaviour for every existing caller.

---

- [x] **Step 7: Run actionlint to verify it passes**

Run:

```bash
mise x actionlint@1.7.12 shellcheck@0.11.0 -- actionlint
```

Expected: exit 0, no output. The four errors from Step 4 are gone.

---

- [x] **Step 8: Run Prettier**

Run:

```bash
mise x node'@24.18.1' prettier'@3.9.6' -- prettier --check .github/workflows/dockerfile.yml .github/workflows/dockerfile-build.yml .github/workflows/dockerfile-publish.yml
```

Expected: `All matched files use Prettier code style!`

If it reports style issues, fix them by re-running the same command with `--write` instead of `--check`, then re-run Step 7, since reformatting can shift things.

---

- [x] **Step 9: Review the diff against the constraints**

Run:

```bash
git diff
```

Check each of these against the diff — they are the things most likely to be silently wrong:

- `default: "false"` is quoted in all three files, and `type: string` — no booleans anywhere.
- `GH_PAT` has no `required: true` in any file, including `dockerfile-publish.yml`.
- The `actions/checkout` SHA is unchanged.
- Only the three intended files appear in the diff.
- The diff is purely additive: every hunk adds lines and removes none.

---

- [x] **Step 10: Commit**

```bash
git add .github/workflows/dockerfile.yml .github/workflows/dockerfile-build.yml .github/workflows/dockerfile-publish.yml
git commit -m "feat(dockerfile): support submodules configuration"
```

The `hk` pre-commit hook runs Prettier and actionlint again on the staged files. It should pass, since Steps 7 and 8 already ran both. If the hook reformats anything, re-stage and re-run the commit.

---

## Manual verification (optional, requires a real repo)

`actionlint` proves the wiring is valid but cannot prove a submodule is actually checked out. To confirm end to end, in a repo that consumes `dockerfile-build.yml` and has a private submodule:

1. Set `submodules: "true"` in the caller and pass `GH_PAT: ${{ secrets.GH_PAT }}`.
2. Run the workflow and confirm the checkout step logs the submodule clone.
3. Confirm the Docker build sees the submodule content.

This is not required to land the change — the default path is unchanged, so nothing regresses for existing callers without it.

---

## Self-Review

**Spec coverage:**

| Spec requirement                                                                  | Covered by                   |
| --------------------------------------------------------------------------------- | ---------------------------- |
| `submodules` string input, default `"false"`, on `dockerfile.yml`                 | Step 5                       |
| `GH_PAT` optional secret on `dockerfile.yml`, ordered after `DOCKER_HUB_PASSWORD` | Step 5                       |
| Checkout wiring with the conditional token expression                             | Step 6                       |
| `persist-credentials: false` hardening + rationale                                | Step 6                       |
| Both wrappers declare and forward `submodules` and `GH_PAT`                       | Steps 2 and 3                |
| `GH_PAT` stays optional in `dockerfile-publish.yml`                               | Step 3, re-checked in Step 9 |
| Out-of-scope workflows untouched                                                  | Global Constraints, Step 9   |
| Verification via hk / Prettier / actionlint                                       | Steps 1, 4, 7, 8, 10         |
| Single `feat:` commit                                                             | Step 10                      |

No gaps.

**Placeholder scan:** No TBD/TODO items, no "add error handling", no "similar to Task N". Every code step contains the literal YAML to write, and every command step contains the literal command and its expected output.

**Type consistency:** `submodules` (string) and `GH_PAT` (secret) are spelled identically across all three files and all steps. The token expression `${{ inputs.submodules != 'false' && secrets.GH_PAT || github.token }}` appears once, in Step 6, and matches `gradle-service.yml:109` and `mise.yml:117` character for character.

**Note on the spec's verification section:** the spec left open whether actionlint runs as part of the hook set. It does — it is a core step in `wetransform/hk-config` covering `.github/workflows/*.yml`, excluding `tf-*`. This plan resolves that open question and uses actionlint as the primary test.
