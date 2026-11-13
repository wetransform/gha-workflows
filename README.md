# gha-workflows

Common GitHub Actions workflows used in wetransform projects.

## Traceable branch image tags

For branch builds, the Docker image is additionally tagged as `<branch>-<YYYYMMDD>T<HHmmss>-<sha>`, e.g. `master-20261113T153012-c9dfaf0`. The date/time is the committer date of the built commit in UTC, the sha is the short commit sha; a `/` in the branch name becomes `-`. Branch names longer than 104 characters get no such tag (Docker tags are limited to 128 characters). Unlike the plain branch tag, this tag identifies the exact commit an image was built from.

- **mise workflows:** only for builds using `docker-bake.hcl`. Not for publishes dispatched with an explicit version.
- **Dockerfile workflows:** enabled by default, in addition to the `tag` input. Disable it with `branch-tag: false`. Not added when a custom `checkout-ref` is used.
- No such tag is created for tag or pull request builds.

These tags are intended for development and test deployments. Production deployments should use release versions.

### Updating branch image tags with Renovate

Renovate's default Docker versioning does not understand these tags. Use a `regex` versioning in a package rule of the consuming repository:

```json
{
  "packageRules": [
    {
      "matchDatasources": ["docker"],
      "matchPackageNames": ["wetransform/<image>"],
      "versioning": "regex:^(?<compatibility>.+)-(?<major>\\d{8})(?:T(?<minor>\\d{6}))?-[0-9a-f]{7}$",
      "separateMajorMinor": false
    }
  ]
}
```

- The branch is treated as compatibility, so updates stay on the branch of the current tag.
- The date is treated as major and the time as minor version. `separateMajorMinor: false` makes Renovate propose only the newest tag instead of separate updates for the same and a newer day.
- Older tags without time (`<branch>-<YYYYMMDD>-<sha>`) are still matched and sort before timestamped tags of the same day.
- Alternative without these tags: pin the plain branch tag to a digest (`wetransform/<image>:master@sha256:…`) and let Renovate update the digest. This works, but the deployed version is not readable from the tag.

## Partial releases (Gradle)

The Gradle library and service workflows can create a release from a non-default branch (a "partial release" of only part of the changes on the default branch). The release is created by `wetransform/gha-gradle-semantic-release`; see its [README](https://github.com/wetransform/gha-gradle-semantic-release#partial-release-from-a-release-branch) for the how-to. By default the reverted changes are re-added on the default branch afterwards; this can be disabled with the `partial-release-readd: false` input of the workflows.

Known limitation: for services, a partial release also pushes the `:latest` image tag, because the `dockerPushImage` task of the wetransform conventions plugin pushes both the versioned tag and `:latest` (`dockerPushLatest` is only an alias that depends on it).
