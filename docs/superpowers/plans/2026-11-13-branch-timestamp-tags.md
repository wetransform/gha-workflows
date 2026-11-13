# Branch Timestamp Image Tags — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Produce `<branch>-<YYYYMMDD>T<HHmmss>-<sha>` image tags in the mise (bake) and Dockerfile workflows, and document how Renovate can follow them for dev/test deployments.

**Architecture:** Both workflows render the tag with `docker/metadata-action` from the workflow context (`{{branch}}`, `{{commit_date}}`, `{{sha}}`). `mise.yml` only changes its existing tag expression. `dockerfile.yml` gets a second, dedicated metadata step whose output is appended to the `build-push-action` tags; it is on by default and can be switched off with a new `branch-tag` input, which the two wrappers forward. The README documents a Renovate `regex:` versioning rule.

**Tech Stack:** GitHub Actions reusable workflows (YAML), `docker/metadata-action` v6.2.0 (pinned `dc802804100637a589fabce1cb79ff13a1411302`), `docker/build-push-action` v7.4.0, validated by `actionlint` 1.7.12 + Prettier via the repo's `hk` pre-commit hook; Renovate regex versioning (checked with Node).

**Spec:** `docs/superpowers/specs/2026-11-13-branch-timestamp-tags-design.md`

## Global Constraints

- Tag expression, verbatim, in both workflows: `{{branch}}-{{commit_date 'YYYYMMDD[T]HHmmss'}}-{{sha}}`
  (single moment format with an escaped literal `T` — verified in metadata-action's README, which uses `[Z]` the same way; default `tz` is UTC, so no `tz=` attribute).
  This supersedes the spec's "two `commit_date` expressions" note; the result is identical.
- Example rendering: `master-20261113T153012-c9dfaf0`. `{{branch}}` sanitizes `/` → `-` (`feature/foo` → `feature-foo`).
- New input name `branch-tag`, `type: boolean`, `default: true`, in `dockerfile.yml`, `dockerfile-build.yml`, `dockerfile-publish.yml`; placed directly after `submodules`.
- Renovate regex, verbatim (JSON-escaped form in the README):
  `^(?<compatibility>.+)-(?<major>\d{8})(?:T(?<minor>\d{6}))?-[0-9a-f]{7}$`
- Do not change any pinned action SHA. Do not touch gradle/play/sbt workflows or managed `tf-*` workflows.
- Conventional commits; no JIRA issue for this branch; no `Co-authored-by` footer.

## Review Focus

- **Tag/PR builds** (`refs/tags/*`, `refs/pull/*`): no branch tag must be produced (`{{branch}}` is empty → would yield an invalid tag). Covered by the `startsWith(github.ref, 'refs/heads/')` condition in both workflows — reviewer checks it is present in Task 2.
- **Dockerfile caller with `checkout-ref`**: built code may differ from `github.sha` → no branch tag. Covered by the `inputs.checkout-ref == ''` condition in Task 2.
- **`branch-tag: false`**: `build-push-action` must receive only the caller's `<image>:<tag>` — the skipped step's empty output must not produce an empty/invalid tag line. Reviewer checks the `tags:` block; confirmed on first real run (see Task 2 Step 6).
- **Old-format tags** (`master-20261113-c9dfaf0`) in a consumer: must parse and sort below any new tag of the same day. Covered by the regex check in Task 3.
- **Branch names with dashes/digits** (`release-2026`, `feature/foo-1`): regex must still isolate branch as compatibility. Covered by the regex check in Task 3.

Known and accepted (not fixed here): a scheduled rebuild of an unchanged commit re-pushes the same branch tag with a new digest — same as today's mise behavior.

---

## File Structure

| File | Change |
| --- | --- |
| `.github/workflows/mise.yml` (~L214-217) | Update tag expression + comment |
| `.github/workflows/dockerfile.yml` | `branch-tag` input; new `branch_tag` metadata step; append to build tags |
| `.github/workflows/dockerfile-build.yml` | Declare + forward `branch-tag` |
| `.github/workflows/dockerfile-publish.yml` | Declare + forward `branch-tag` |
| `README.md` | New section on branch image tags + Renovate rule |

## How this is tested

No workflow test harness exists. `actionlint` validates reusable-workflow calls (an input forwarded by a wrapper but not declared in `dockerfile.yml` fails), giving a red/green cycle for Task 2. Tag rendering itself can only be observed in a real run (Task 2 Step 6). The Renovate regex is checked with a Node one-liner (Task 3).

Lint command used throughout:

```bash
mise x actionlint@1.7.12 shellcheck@0.11.0 -- actionlint
```

---

### Task 1: Add commit time to the mise branch tag

**Files:**
- Modify: `.github/workflows/mise.yml` — the last entry of the `Docker metadata` step's `tags:` (currently `type=raw,value={{branch}}-{{commit_date 'YYYYMMDD'}}-{{sha}},enable=...`) and its comment

**Interfaces:**
- Consumes: nothing.
- Produces: the tag format that Tasks 2 and 3 reuse verbatim.

- [ ] **Step 1: Confirm lint baseline** — run the lint command. Expected: exit 0 (note any pre-existing unrelated errors).
- [ ] **Step 2: Change the value** to `{{branch}}-{{commit_date 'YYYYMMDD[T]HHmmss'}}-{{sha}}`; keep the `enable=` expression unchanged. Update the comment's first line to `# Immutable, traceable tag for branch builds: <branch>-<commit date>T<commit time, UTC>-<short sha>`; keep the rest of the comment.
- [ ] **Step 3: Lint** — run the lint command. Expected: exit 0.
- [ ] **Step 4: Commit**

```bash
git add .github/workflows/mise.yml
git commit -m "feat(mise): add commit time to traceable branch image tag" \
  -m "Tags are now <branch>-<YYYYMMDD>T<HHmmss>-<sha> so builds of the same day can be ordered (e.g. by Renovate)."
```

---

### Task 2: Branch tag for Dockerfile builds (on by default)

**Files:**
- Modify: `.github/workflows/dockerfile-build.yml`, `.github/workflows/dockerfile-publish.yml` — inputs + `with:`
- Modify: `.github/workflows/dockerfile.yml` — inputs, new step, `Build and push` tags

**Interfaces:**
- Consumes: tag expression from Global Constraints.
- Produces: input `branch-tag` (boolean, default `true`) on all three workflows; step id `branch_tag` in `dockerfile.yml`.

- [ ] **Step 1: Failing test — forward the input in both wrappers.** Declare after `submodules`:

```yaml
      branch-tag:
        description: Also tag branch builds as <branch>-<YYYYMMDD>T<HHmmss>-<sha>
        type: boolean
        default: true
```

and add `branch-tag: ${{ inputs.branch-tag }}` to the `with:` block after `submodules:`.

- [ ] **Step 2: Lint, expect failure** — Expected: actionlint reports `input "branch-tag" is not defined in "./.github/workflows/dockerfile.yml" reusable workflow` for both wrappers.
- [ ] **Step 3: Declare the same `branch-tag` input in `dockerfile.yml`** (after `submodules`, identical block).
- [ ] **Step 4: Add the tag step and wire it into the build.** Keep the existing `Docker meta` (`id: meta`) step unchanged — it provides labels, and giving it explicit tags would change its `org.opencontainers.image.version` label. Insert after it:

```yaml
      # Immutable, traceable tag for branch builds: <branch>-<commit date>T<commit time, UTC>-<short sha>
      # Skipped for tag/PR refs ({{branch}} is empty there) and for a custom checkout-ref
      # (the workflow context's sha/commit date would not match the built code)
      - name: Docker branch tag
        id: branch_tag
        if: ${{ inputs.branch-tag && startsWith(github.ref, 'refs/heads/') && inputs.checkout-ref == '' }}
        uses: docker/metadata-action@dc802804100637a589fabce1cb79ff13a1411302 # v6.2.0
        with:
          images: ${{ inputs.image }}
          tags: |
            type=raw,value={{branch}}-{{commit_date 'YYYYMMDD[T]HHmmss'}}-{{sha}}
```

In `Build and push`, replace the single-string `tags:` with:

```yaml
          tags: |
            ${{ inputs.image }}:${{ inputs.tag }}
            ${{ steps.branch_tag.outputs.tags }}
```

Leave the vulnerability scan on `${{ inputs.image }}:${{ inputs.tag }}`.

- [ ] **Step 5: Lint** — Expected: exit 0.
- [ ] **Step 6: Commit**

```bash
git add .github/workflows/dockerfile.yml .github/workflows/dockerfile-build.yml .github/workflows/dockerfile-publish.yml
git commit -m "feat(dockerfile): add traceable <branch>-<date>T<time>-<sha> image tag" \
  -m "Enabled by default for branch builds, can be disabled with the branch-tag input. Skipped for tag/PR refs and when a custom checkout-ref is used."
```

Post-merge check (outside this plan's execution, note it in the hand-off): in the first consumer run on a branch, the `Build and push` log lists both tags; with `branch-tag: false` it lists only `<image>:<tag>`.

---

### Task 3: Document branch tags and the Renovate rule

**Files:**
- Modify: `README.md` — new `## Traceable branch image tags` section after the intro, before `## Partial releases (Gradle)`

**Interfaces:**
- Consumes: tag format (Task 1/2), `branch-tag` input (Task 2), regex from Global Constraints.
- Produces: nothing.

- [ ] **Step 1: Verify the regex** (this is the test for the documented rule):

```bash
mise x node@24 -- node -e '
const re = /^(?<compatibility>.+)-(?<major>\d{8})(?:T(?<minor>\d{6}))?-[0-9a-f]{7}$/;
const cases = {
  "master-20261113T153012-c9dfaf0": ["master","20261113","153012"],
  "master-20261113-c9dfaf0":        ["master","20261113",undefined],
  "feature-foo-1-20261113T080000-1234567": ["feature-foo-1","20261113","080000"],
  "release-2026-20261113T080000-abcdef0":  ["release-2026","20261113","080000"],
};
for (const [t, exp] of Object.entries(cases)) {
  const g = t.match(re)?.groups; const got = g && [g.compatibility, g.major, g.minor];
  if (JSON.stringify(got) !== JSON.stringify(exp)) { console.error("FAIL", t, got); process.exit(1); }
}
for (const t of ["master", "latest", "v1.2.3", "master-2026111-c9dfaf0"]) if (re.test(t)) { console.error("FAIL matched", t); process.exit(1); }
console.log("OK");'
```

Expected: `OK`.

- [ ] **Step 2: Write the section.** Content (wording is the builder's):
  - What: workflows `mise*` (with `docker-bake.hcl`) and `dockerfile*` push `<branch>-<YYYYMMDD>T<HHmmss>-<sha>` for branch builds (commit time, UTC; `/` in branch names becomes `-`); not for tag/PR builds or Dockerfile builds with `checkout-ref`; Dockerfile: disable with `branch-tag: false`.
  - Intended for dev/test deployments; production should use release versions.
  - Renovate `packageRules` example:

```json
{
  "matchDatasources": ["docker"],
  "matchPackageNames": ["wetransform/<image>"],
  "versioning": "regex:^(?<compatibility>.+)-(?<major>\\d{8})(?:T(?<minor>\\d{6}))?-[0-9a-f]{7}$"
}
```

  - Notes: branch = compatibility, so updates stay on the same branch; older date-only tags remain valid and sort before same-day timestamped ones; alternative without these tags is Renovate digest pinning of the branch tag (`image:master@sha256:…`), which is less readable.

- [ ] **Step 3: Lint/format** — `mise x -- hk check --all` (or `hk fix` for Prettier). Expected: no errors.
- [ ] **Step 4: Commit**

```bash
git add README.md
git commit -m "docs: document traceable branch image tags and Renovate config"
```
