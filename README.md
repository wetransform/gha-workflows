# gha-workflows

Common GitHub Actions workflows used in wetransform projects.

## Partial releases (Gradle)

The Gradle library and service workflows can create a release from a non-default branch (a "partial release" of only part of the changes on the default branch). The release is created by `wetransform/gha-gradle-semantic-release`; see its [README](https://github.com/wetransform/gha-gradle-semantic-release#partial-release-from-a-release-branch) for the how-to. By default the reverted changes are re-added on the default branch afterwards; this can be disabled with the `partial-release-readd: false` input of the workflows.

Known limitation: for services, a partial release also pushes the `:latest` image tag, because the `dockerPushImage` task of the wetransform conventions plugin pushes both the versioned tag and `:latest` (`dockerPushLatest` is only an alias that depends on it).
