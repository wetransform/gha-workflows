# Partial Releases from a Release Branch — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Let a person run the existing Release workflow on a branch cut from the default branch (with `git revert`s of unready changes); after the release, a bot merges the branch back into the default branch and re-applies the reverted changes.

**Architecture:** Two bash scripts in `gha-gradle-semantic-release` carry the logic and are unit-tested with bats against throwaway git repos: `release-preflight.sh` (runs before semantic-release: decides `default` / `partial` / `skip`, enforces the guards) and `merge-back.sh` (runs after a partial release: merge commit, re-adds, push with retry). `action.yml` wires them around the existing semantic-release step. `gha-workflows` passes two new inputs through the gradle-service/library workflows; `github-bootstrap` exposes the re-add switch on the managed Release workflow.

**Tech Stack:** Bash, git, bats-core (via mise), GitHub Actions composite action + reusable workflows, semantic-release 25.0.9 (unchanged), actionlint/Prettier via hk.

**Spec:** `docs/superpowers/specs/2026-10-23-partial-release-design.md` (in `gha-workflows`)

## Global Constraints

- Repos and branches: work on branch `feat/partial-release` in `/home/simon/repos/wetf/gha-gradle-semantic-release`, `/home/simon/repos/wetf/gha-workflows` (already exists, holds spec + this plan) and `/home/simon/repos/wetf/github-bootstrap`.
- Regular default-branch releases must be untouched in behaviour. Sole exception: the pending-merge-back guard (refuse when an unmerged newer release tag exists).
- Release tags are semantic-release's default `v<major>.<minor>.<patch>`. Only tags matching `^v[0-9]+\.[0-9]+\.[0-9]+$` are considered by the guards (repos have odd tags like `v1.7.0-data`).
- Pushes use the existing we-release app token (`inputs.github-token` of the action). No new secrets, no ruleset changes.
- Never use `[skip ci]` in any commit the bot creates.
- Bot git identity: `wetransform Bot <113353961+wetransformer@users.noreply.github.com>` (same as the semantic-release step). Set `HK: "0"` on every step that commits.
- Trailer marking bot commits: `Release-Merge-Back: v<version>`. Re-add commits additionally carry `Re-applies: <original full sha>`.
- New tools via `mise.toml`, latest release at implementation time (`mise latest bats`). New action pins by full SHA with `# vX.Y.Z` comment, latest release.
- Conventional Commits; no `Co-authored-by`. No JIRA issue known — omit footer.
- Out of scope: sbt, play, mise, dockerfile, gha workflow types; `.releaserc.yml`-customised repos get no special handling.

## Review Focus

1. **Default branch moved between release and merge-back push** (someone merged a PR meanwhile) → re-fetch and redo merge + re-adds, up to 3 attempts, then fail with recovery instructions. Test in Task 2.
2. **A revert whose message was edited** (no `This reverts commit <sha>.` line) → not re-added; listed as a warning in the output/summary so a person can handle it. Test in Task 2.
3. **Re-add conflicts** (e.g. two reverts touching the same lines) → reverts are re-applied newest-first; on conflict, abort, push nothing, fail with recovery instructions. Merge-back is all-or-nothing. Test in Task 2.
4. **Manual `workflow_dispatch` release on the default branch right after a merge-back** must release the re-added changes (skip applies only to `push` events). Test in Task 1.
5. **Release branch names with slashes** (`release/2.3.x`) work in messages, refs and push. Covered by using such a name in Task 2 fixtures.

---

## File Structure

| Repo | File | Responsibility |
| --- | --- | --- |
| gha-gradle-semantic-release | `mise.toml` (create) | bats tool + `test` task |
| | `scripts/release-preflight.sh` (create) | mode decision + guards before semantic-release |
| | `scripts/merge-back.sh` (create) | merge commit, re-adds, push with retry, recovery text |
| | `test/helpers.bash` (create) | fixture: bare `origin` + work clone, commit/tag helpers |
| | `test/release-preflight.bats`, `test/merge-back.bats` (create) | unit tests |
| | `.github/workflows/test-scripts.yml` (create) | run `mise run test` on PRs |
| | `action.yml` (modify) | new inputs, preflight/merge-back steps, adjusted conditions |
| | `README.md` (modify) | partial release how-to (person-facing) |
| gha-workflows | `.github/workflows/gradle-{library,service}.yml` | pass `partial-release-readd`, `release-tasks` to the action; action pin |
| | `.github/workflows/gradle-{library,service}-publish.yml` | declare/forward inputs; service computes `release-tasks` without `:latest` |
| | `README.md` | link to how-to |
| github-bootstrap | `workflow-templates/release.gradle-{library,service}.tfpl.yml` | `reAddReverted` dispatch input → `partial-release-readd`; pin |
| | `examples/…` | regenerated via `workflow-templates/generate-workflow-examples.sh` |

---

### Task 1: Test harness + `release-preflight.sh`

**Files:**
- Create: `gha-gradle-semantic-release/mise.toml`, `scripts/release-preflight.sh`, `test/helpers.bash`, `test/release-preflight.bats`, `.github/workflows/test-scripts.yml`

**Interfaces:**
- Produces: `scripts/release-preflight.sh --branch <name> --default-branch <name> --event <github.event_name>`; run in the checked-out repo (HEAD = branch tip, full history + tags, `origin/<default>` present). Prints `mode=<default|partial|skip>` to stdout and appends it to `$GITHUB_OUTPUT` when set. Exit 0 on success; exit 1 with a `::error::` line on a guard failure.
- Produces: `test/helpers.bash` functions `setup_repos` (creates `$ORIGIN` bare repo and `$WORK` clone with default branch `master`, cd into `$WORK`, sets git identity), `commit <message>` (adds a change to a unique file, commits with given message, echoes full sha), `tag_release <version>` (lightweight tag `v<version>` on HEAD, pushed). Task 2 reuses these.

Behaviour:
- `branch == default-branch`:
  - Pending-merge-back guard: `R` = newest `vX.Y.Z` tag reachable from HEAD. If any `vX.Y.Z` tag with higher version (sort `-V`) is **not** an ancestor of HEAD → exit 1: `::error::Release <tag> was created from another branch but is not merged into <default>. Finish the merge-back first (see the failed release run's summary).`
  - If `event == push`, there is at least one commit in `git log --first-parent R..HEAD`, and every such commit has a `Release-Merge-Back` trailer → `mode=skip`.
  - Else `mode=default`. No tags at all → `mode=default`.
- `branch != default-branch`: `T` = newest `vX.Y.Z` tag reachable from `origin/<default>`. If `T` exists and is not an ancestor of HEAD → exit 1: `::error::Branch <branch> does not contain <T>, the latest release of <default>. Cut the release branch from <default> after <T>.` Else `mode=partial`.

- [ ] **Step 1: Add `mise.toml`** with `bats = "<latest>"` under `[tools]` and a task `test = "bats test"`. Run `mise install`.
- [ ] **Step 2: Write `test/helpers.bash` and the failing tests** in `test/release-preflight.bats`:
  - `default branch, no tags → mode=default`
  - `default branch, new feat after tag → mode=default`
  - `default branch, push event, only Release-Merge-Back commits since tag → mode=skip` (fixture: branch `release/2.3.x` from master at tag `v2.3.0`, commit + tag `v2.3.1` on it, `git merge --no-ff` into master with trailer, one re-add commit with trailer)
  - `same fixture, workflow_dispatch event → mode=default` (Review Focus 4)
  - `same fixture plus a normal commit on master → mode=default`
  - `default branch, unmerged newer tag v2.3.1 on side branch → exit 1, output contains "v2.3.1 was created from another branch"`
  - `default branch, ignores non-semver tag v9.9.9-data that is unmerged → mode=default`
  - `release branch containing latest master tag → mode=partial`
  - `release branch cut before latest master tag → exit 1, output contains "does not contain v2.4.0"`
  - `writes mode to $GITHUB_OUTPUT when set`
- [ ] **Step 3: Run** `mise run test` — expect failures (script missing).
- [ ] **Step 4: Implement `scripts/release-preflight.sh`** (`set -euo pipefail`, executable, long-option parsing, behaviour above; trailer check via `git log --format='%(trailers:key=Release-Merge-Back,valueonly)'`).
- [ ] **Step 5: Run** `mise run test` — all pass.
- [ ] **Step 6: Add `.github/workflows/test-scripts.yml`**: on `pull_request` and push to `master`; `actions/checkout` (same pin as other repos: `3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1`), `jdx/mise-action` (latest, SHA-pinned), `mise run test`.
- [ ] **Step 7: Commit** `feat: add release preflight checks for partial releases`

### Task 2: `merge-back.sh`

**Files:**
- Create: `gha-gradle-semantic-release/scripts/merge-back.sh`, `test/merge-back.bats`

**Interfaces:**
- Consumes: `test/helpers.bash` from Task 1.
- Produces: `scripts/merge-back.sh --branch <release-branch> --default-branch <name> --version <x.y.z> --readd <true|false> --push-url <url> [--max-attempts <n, default 3>]`. Run after semantic-release in the checked-out repo (HEAD = release commit on the release branch). Exit 0 when pushed. On failure exit 1 and print recovery instructions; both success summary and recovery text are also appended to `$GITHUB_STEP_SUMMARY` when set. Must leave nothing pushed on failure.

Behaviour per attempt:
1. `git fetch <push-url> <default>` and `git checkout -B merge-back FETCH_HEAD`.
2. `git merge --no-ff --no-edit <release HEAD sha>` with message
   `chore(release): merge v<version> from <branch> into <default>` + blank line + `Release-Merge-Back: v<version>`. Conflict → abort, fail (no retry).
3. If `--readd true`: revert commits = `git log --format=%H <merge-base(release HEAD, fetched default)>..<release HEAD>` whose subject starts with `Revert "` (newest first, as `git log` yields). For each: extract `This reverts commit ([0-9a-f]{40})\.` from the body. No match → record warning `Revert <short sha> "<subject>" has no "This reverts commit" line — not re-added; re-apply it by hand if needed.` and continue. Match → `git revert --no-commit <revert sha>`, then commit with the original's full message (`git log -1 --format=%B <orig>`) plus trailers `Re-applies: <orig>` and `Release-Merge-Back: v<version>` (via `git interpret-trailers`), author = original author (`--author`). Conflict → `git revert --abort`, fail (no retry).
4. `git push <push-url> HEAD:refs/heads/<default>`. Rejected → next attempt from step 1; after `max-attempts` → fail.

Recovery text (on any failure) states: the release `v<version>` is published and must not be re-run; the commands to finish by hand: `git fetch origin && git switch <default> && git pull && git merge --no-ff origin/<branch>`, then for each revert to re-add `git revert <revert sha>` with the original message; and that releases from `<default>` are blocked until then.

- [ ] **Step 1: Write failing tests** in `test/merge-back.bats` (fixtures from helpers; release branch named `release/2.3.x`; "release commit" simulated by `chore(release): 2.3.1` commit + `tag_release 2.3.1` on the branch; `--push-url "$ORIGIN"`):
  - `prefix case: merges into master with merge commit, v2.3.1 reachable from origin/master`
  - `merge commit message and Release-Merge-Back trailer`
  - `interleaved: feat reverted on branch → master content contains the feat again; re-add commit message equals original + trailers; author is original author`
  - `readd false → master contains the revert, no re-add commit`
  - `two reverts touching different files → both re-added, newest revert first`
  - `revert with edited message → not re-added, warning printed, exit 0` (Review Focus 2)
  - `re-add conflict → exit 1, origin/master unchanged, recovery text printed` (Review Focus 3; fixture: revert A, then a later commit on branch modifying the same lines, revert of that later commit too)
  - `master moved before push → succeeds on retry` (simulate with a `pre-push` hook in `$WORK` that pushes an extra commit to `$ORIGIN` master once) (Review Focus 1)
  - `push keeps failing → exit 1 after 3 attempts, recovery text printed, GITHUB_STEP_SUMMARY written`
  - `next preflight on master after merge-back (push) → mode=skip; after normal commit → mode=default` (integration with Task 1)
- [ ] **Step 2: Run** `mise run test` — new tests fail.
- [ ] **Step 3: Implement `scripts/merge-back.sh`** per behaviour above.
- [ ] **Step 4: Run** `mise run test` — all pass.
- [ ] **Step 5: Commit** `feat: add merge-back of partial releases into default branch`

### Task 3: Wire scripts into `action.yml` + how-to

**Files:**
- Modify: `gha-gradle-semantic-release/action.yml`, `README.md`

**Interfaces:**
- Consumes: both scripts (call as `${{ github.action_path }}/scripts/...`).
- Produces: new action input `partial-release-readd` (description "Re-add reverted changes on the default branch after a partial release", default `'true'`). Existing `release-tasks` input unchanged in meaning (Task 4 uses it). New output `partial-release` (`'true'` when mode was `partial`).

Changes:
- Before "Release with semantic-release": step `preflight` (`if: inputs.release == 'true'`), runs `release-preflight.sh --branch "${{ github.head_ref || github.ref_name }}" --default-branch "${{ github.event.repository.default_branch }}" --event "${{ github.event_name }}"`.
- semantic-release step: add `&& steps.preflight.outputs.mode != 'skip'` to its `if`.
- New step after it: "Merge back partial release", `if: inputs.release == 'true' && inputs.release-dryrun != 'true' && steps.preflight.outputs.mode == 'partial' && steps.release.outputs.new_release_published == 'true'`; env: bot identity as `GIT_AUTHOR_*`/`GIT_COMMITTER_*`, `HK: "0"`; runs `merge-back.sh` with `--push-url "https://x-access-token:${{ inputs.github-token }}@github.com/${{ github.repository }}.git"` (token via env var, not inline in the command string), `--version ${{ steps.release.outputs.new_release_version }}`, `--readd ${{ inputs.partial-release-readd }}`.
- "Publish with Gradle" step: also run when `steps.preflight.outputs.mode == 'skip'` (same as "no release").

- [ ] **Step 1: Edit `action.yml`** as above.
- [ ] **Step 2: Verify** `hk check --all` (actionlint doesn't cover composite actions; this runs Prettier/YAML checks) — expect clean. Then `mise run test` — still green.
- [ ] **Step 3: README how-to** section "Partial release from a release branch": when to use; steps (branch from default at chosen commit; `git revert <sha>` for every commit of each unready change incl. follow-up fixes; keep generated message; no `--no-commit`; push; run Release on that branch, optionally untick re-add); what the bot does; the guards and what their errors mean; recovery when merge-back fails.
- [ ] **Step 4: Commit** `feat: support partial releases from non-default branches`

### Task 4: Pass inputs through `gha-workflows`

**Files:**
- Modify: `gha-workflows/.github/workflows/gradle-library.yml`, `gradle-service.yml`, `gradle-library-publish.yml`, `gradle-service-publish.yml`, `README.md`

**Interfaces:**
- Consumes: action inputs `partial-release-readd`, `release-tasks` (Task 3).
- Produces: workflow_call input `partial-release-readd` (boolean, default `true`) on all four workflows; input `release-tasks` (string, default `""`) on `gradle-library.yml` and `gradle-service.yml`. Task 5 passes `partial-release-readd` from the managed template.

Changes:
- `gradle-{library,service}.yml`: declare both inputs, pass to the action step (`partial-release-readd: ${{ inputs.partial-release-readd }}`, `release-tasks: ${{ inputs.release-tasks }}`). Temporarily pin the action to the `feat/partial-release` head SHA of `gha-gradle-semantic-release` for end-to-end testing (Task 6 replaces it with the release pin).
- `gradle-{library,service}-publish.yml`: declare `partial-release-readd`, forward it.
- `gradle-service-publish.yml` only (nice-to-have, no `:latest` from partial releases): forward `release-tasks: ${{ inputs.publish-tasks == '' && github.ref_name != github.event.repository.default_branch && 'dockerPushImage' || '' }}`.
- README: one paragraph + link to the action README how-to.

- [ ] **Step 1: Verify the `:latest` assumption first:** in a gradle-service repo using the wetransform docker conventions (e.g. `/home/simon/repos/wetf/bucket-service`), confirm `dockerPushImage` pushes only versioned tags and `dockerPushLatest` only `:latest` (e.g. `./gradlew help --task dockerPushImage` / inspect the conventions plugin source). If `dockerPushImage` also pushes `:latest` or the split isn't clean, skip the `release-tasks` change and note "partial releases also push `:latest`" as a known limitation in the README instead.
- [ ] **Step 2: Edit workflows** as above.
- [ ] **Step 3: Verify** `hk check --all` (runs actionlint, which validates inputs passed to local reusable workflows) — expect clean.
- [ ] **Step 4: Commit** `feat: pass partial release options to gradle release`

### Task 5: Managed Release workflow input in `github-bootstrap`

**Files:**
- Modify: `github-bootstrap/workflow-templates/release.gradle-library.tfpl.yml`, `release.gradle-service.tfpl.yml`; regenerated files under `examples/`

**Interfaces:**
- Consumes: workflow input `partial-release-readd` (Task 4).

Changes: add `workflow_dispatch` input `reAddReverted` (`type: boolean`, description `"Re-add reverted changes (only for releases from non-default branches)"`, `default: true`) after `skipCheck`; pass `partial-release-readd: $${{ github.event_name != 'workflow_dispatch' || inputs.reAddReverted }}`. Pin bump of `gha-workflows` happens in Task 6.

- [ ] **Step 1: Edit both templates.**
- [ ] **Step 2: Regenerate examples** with `workflow-templates/generate-workflow-examples.sh`; check the diff only adds the input and its pass-through.
- [ ] **Step 3: Verify** `hk check --all` — clean.
- [ ] **Step 4: Commit** `feat: add option to re-add reverted changes on partial releases`

### Task 6: End-to-end verification in `exp-gradle-release`, then release roll-out

Free to create releases in `wetransform/exp-gradle-release`. Push the three feature branches first.

- [ ] **Step 1: Point the test repo at the feature code:** on `master` of `exp-gradle-release`, edit `.github/workflows/tf-release.yml` by hand to use `gradle-library-publish.yml@feat/partial-release` and the new `reAddReverted` input (as in Task 5's template). Commit `ci: test partial release workflow`. Note: release branches must be cut after this commit.
- [ ] **Step 2: Run the spec's scenarios** via `gh workflow run tf-release.yml --ref <branch> [-f reAddReverted=false]` and check results (`git log --graph`, tags, `CHANGELOG.md`, GitHub release notes):
  1. Prefix: `fix:` on master, then `feat:` on master; branch `release/prefix` at the fix → patch release listing only the fix → master gets merge commit; next master release is minor listing only the feat.
  2. Interleaved: `feat:` then `fix:` on master; branch at master, `git revert` the feat → patch release notes list only the fix (confirms revert filtering) → master has re-add commit; next master release is minor listing the feat exactly once.
  3. Re-add disabled: as 2 with `reAddReverted=false` → master contains revert, no re-add.
  4. Guard: branch cut before the latest master tag → run fails with the "does not contain" error, no release.
  5. Regular release from master: unchanged behaviour (dry run plus one real release).
  6. Pending merge-back guard: create a partial release, then simulate a failed merge-back (e.g. run with a branch whose merge conflicts on `dummychanges.txt`) → master release refused with the "not merged" error; finish by hand per recovery text → master release works.
- [ ] **Step 3: Report results to the user** (versions created, changelog excerpts, anything unexpected) before releasing anything shared.
- [ ] **Step 4 (with user, PRs need review):** merge + release `gha-gradle-semantic-release` → set the release SHA pin in `gha-workflows` (`fix: pin gha-gradle-semantic-release <version>`), merge + release → set the new `gha-workflows` pin in both `github-bootstrap` templates, regenerate examples, merge + apply → reset `exp-gradle-release`'s `tf-release.yml` to the managed version.
