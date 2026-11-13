# Branch timestamp image tags (`<branch>-<date>T<time>-<sha>`)

## Intent

Dev/test deployments should be able to follow branch builds (e.g. `master`) via Renovate,
with image tags that stay human-readable and traceable to a commit. Releases remain the
preferred (and in production required) way to deploy; this is not a replacement for them.

The existing `<branch>-<YYYYMMDD>-<sha>` tag (mise workflows with `docker-bake.hcl`) cannot be
ordered by Renovate when a branch is built more than once per day, because the sha is not
sortable. Dockerfile builds do not produce such a tag at all.

## Goals

- New tag format `<branch>-<YYYYMMDD>T<HHmmss>-<sha>`, e.g. `master-20261113T153012-c9dfaf0`
  (commit date/time in UTC, short sha).
- Produced by `mise.yml` (bake builds) — replacing the current date-only format.
- Produced by `dockerfile.yml` **by default**, disableable via an input, in addition to the
  caller-provided `tag`.
- Documented Renovate configuration that updates such tags, ordered correctly, staying on the
  same branch.

## Non-goals

- Gradle/Play service workflows (image tagged by the build tool; would need a re-tag step) — later.
- mise builds without `docker-bake.hcl` (the task tags the image itself).
- A shared Renovate preset; documentation in the README is sufficient for now.

## Anti-goals

- A tag whose timestamp/sha does not match the code that was actually built.
- Breaking existing consumers of the caller-provided tag or already-pushed old-format tags.

## Approaches considered

- **A. Keep date-only format, add Renovate regex config** — no workflow change, but builds of
  the same day cannot be ordered, so dev environments lag. Rejected.
- **B. Add time to the tag + documented Renovate rule** — readable, traceable, sortable. **Chosen.**
- **C. Renovate digest pinning of the moving branch tag (`master@sha256:…`)** — no change needed,
  but the deployed version is not readable. Remains a fallback for repos that can't adopt B.

Separator between date and time: `T` (ISO 8601 basic format) — chosen over `-` (adds a fourth
dash-separated field, ambiguous with numeric branch suffixes) and `.` (reads like a version).

## Design

### Tag expression (both workflows)

```
type=raw,value={{branch}}-{{commit_date 'YYYYMMDD[T]HHmmss'}}-{{sha}}
```

Revised after review: timestamp and short sha are taken from the checked out commit in a
shell step (`git show -s --format=%cd`, committer date, UTC) and passed into a `type=raw` value,
because metadata-action's `commit_date` uses the push payload's commit timestamp, which may be
the author date (rebased commits would get older tags). The same step skips the tag for branch
names longer than 104 characters (Docker tag limit 128). The Renovate rule additionally sets
`"separateMajorMinor": false`.

### `mise.yml`

Replace the existing `{{branch}}-{{commit_date 'YYYYMMDD'}}-{{sha}}` line with the expression above.
Keep the existing `enable` condition (branch refs only, not for publishes dispatched with an
explicit version).

### `dockerfile.yml`

- New boolean input `branch-tag` (default `true`), passed through by `dockerfile-build.yml`
  and `dockerfile-publish.yml`.
- A separate `docker/metadata-action` step renders the expression above (the existing step only
  provides labels; giving it explicit tags would change its version label). It runs only if `branch-tag` is true, the ref is a branch (`refs/heads/`), and `checkout-ref`
  is empty (a custom checkout ref means the workflow context's sha/date would not match the
  built code — same reasoning as mise's explicit-version case).
- `build-push-action` `tags:` = the caller's `<image>:<tag>` plus the metadata-action tags.
- Vulnerability scan stays on `<image>:<tag>` (same image).

### Renovate (README)

Document a `packageRules` entry for consumers, e.g.:

```json
{
  "matchDatasources": ["docker"],
  "matchPackageNames": ["wetransform/<image>"],
  "versioning": "regex:^(?<compatibility>.+)-(?<major>\\d{8})(?:T(?<minor>\\d{6}))?-[0-9a-f]{7}$"
}
```

- `compatibility` = branch → updates stay on the same branch.
- Time is optional, so old-format tags (`master-20261113-c9dfaf0`) count as `000000` and are
  ordered correctly against new ones — no migration needed.
- Mention digest pinning (approach C) as the alternative, and that this is for dev/test only.

## Constraints

- Docker tag charset `[A-Za-z0-9_.-]`, max 128 chars; `{{branch}}` turns `/` into `-`
  (verified in the metadata-action README; note it in the README).
- Commit date is the committer date of the built commit — monotonic enough for dev use, not
  strictly push order.

## Left to the builder

- Exact input description/wording and README placement.
- Changelog is generated from conventional commits; use `feat:` for the dockerfile default.
