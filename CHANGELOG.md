## [1.1.6](https://github.com/wetransform/gha-workflows/compare/v1.1.5...v1.1.6) (2024-03-24)


### Bug Fixes

* **deps:** update actions/setup-node action to v4.0.2 ([212f385](https://github.com/wetransform/gha-workflows/commit/212f385244784ccf67b68f98bff12e029955c660))

## [1.1.5](https://github.com/wetransform/gha-workflows/compare/v1.1.4...v1.1.5) (2024-03-24)


### Bug Fixes

* **deps:** update actions/upload-artifact action to v4 ([73b6113](https://github.com/wetransform/gha-workflows/commit/73b611379b2bbe5029784fd19a92c1edc7df7956))

## [1.1.4](https://github.com/wetransform/gha-workflows/compare/v1.1.3...v1.1.4) (2024-03-24)


### Bug Fixes

* **deps:** update actions/setup-java action to v4 ([2541900](https://github.com/wetransform/gha-workflows/commit/2541900f7add1e70847bae85f6143af2a720a817))

## [1.1.3](https://github.com/wetransform/gha-workflows/compare/v1.1.2...v1.1.3) (2024-03-24)


### Bug Fixes

* **deps:** update gradle/actions action to v3.1.0 ([8ea6da0](https://github.com/wetransform/gha-workflows/commit/8ea6da0c4bb6daa4bd0080c3d58ba3e3e3d74dfe))

## [1.1.2](https://github.com/wetransform/gha-workflows/compare/v1.1.1...v1.1.2) (2024-03-24)


### Bug Fixes

* **deps:** update mikepenz/action-junit-report action to v4 ([4d67cc2](https://github.com/wetransform/gha-workflows/commit/4d67cc25528aa1c50370725e7bc218f70ee70165))

## [1.1.1](https://github.com/wetransform/gha-workflows/compare/v1.1.0...v1.1.1) (2024-03-24)


### Bug Fixes

* **deps:** update actions/checkout action to v4 ([fac4bf6](https://github.com/wetransform/gha-workflows/commit/fac4bf608378f7e3d294f63fe1ab5371f0d2e46a))

## [1.1.0](https://github.com/wetransform/gha-workflows/compare/v1.0.0...v1.1.0) (2024-03-23)


### Features

* add first version of Play framework workflows ([ae3be41](https://github.com/wetransform/gha-workflows/commit/ae3be414b473969c1727ea48b619bfc449c0a333))
* add shared workflows based on Dockerfiles ([836aae8](https://github.com/wetransform/gha-workflows/commit/836aae8ee064bcf44ffbce2de51f7130010ce236))
* allow specifying custom Gradle tasks for service check ([161be51](https://github.com/wetransform/gha-workflows/commit/161be51606b0bc1fea5efaf366dde3ff86106e82))
* configure failure on test failure for Gradle library ([0aeec1a](https://github.com/wetransform/gha-workflows/commit/0aeec1a62f9edc5b193e521a77df60cef67d57f7))
* first versions of workflows for sbt libraries ([c01a8d2](https://github.com/wetransform/gha-workflows/commit/c01a8d2d36a2ecee3641a35879545564550050f4))
* generate lockfiles for Gradle subprojects ([b841f71](https://github.com/wetransform/gha-workflows/commit/b841f7180f42cb9fcee30a45d550c2dcb55ac706))
* make context for Docker build configurable ([85ac57e](https://github.com/wetransform/gha-workflows/commit/85ac57e239e40b0ea7399f689aa59a49a9d5c44f))
* make DockerHub login optional ([2d2e709](https://github.com/wetransform/gha-workflows/commit/2d2e7091e407a675e81f8e83c69c77b40a941d3a))
* support artifact upload for Gradle library build ([5547d18](https://github.com/wetransform/gha-workflows/commit/5547d18e4c4a20ccf7b93a48d044fdf33a55c287))
* support custom pre-build command in play workflows ([f265b2c](https://github.com/wetransform/gha-workflows/commit/f265b2c33a00307416be2e70191580306b745b63))
* support multi module and custom tasks ([37c8f83](https://github.com/wetransform/gha-workflows/commit/37c8f8320ff3b8a4858b86f841636a7d677d245b))
* support multi module builds for services ([a867ddb](https://github.com/wetransform/gha-workflows/commit/a867ddbb5c5922133bffc7068842ecfbf19f6cfe))
* support scanning multiple images ([46af370](https://github.com/wetransform/gha-workflows/commit/46af37020d5e9b0ce024ce30a27edfde905b60b7))
* support setting for expecting tests in gradle service check ([17c68ad](https://github.com/wetransform/gha-workflows/commit/17c68ad5d19381cf35deff1855424baeb152529f))
* support setting for expecting tests in gradle service publishing ([769b69a](https://github.com/wetransform/gha-workflows/commit/769b69aa83a0e6003625cdab7588d86cc6c8967d))
* suppport passing custom environment variables ([ee341f2](https://github.com/wetransform/gha-workflows/commit/ee341f2b8f03e8d7d70c94d96a7e4e6372ffe138))


### Bug Fixes

* add missing input types ([d98ffce](https://github.com/wetransform/gha-workflows/commit/d98ffce2e5f501c07162a0c36cfafe6aaa842759))
* fix configuration for pushing image ([9b93e03](https://github.com/wetransform/gha-workflows/commit/9b93e030fc1b4a339cacd8be2ff0d8dd5b076677))
* fix reference to wrong base workflow ([d110e2f](https://github.com/wetransform/gha-workflows/commit/d110e2ff0307f9718d2e122075de9320018bb831))
* registry credentials should only be required for pushing ([2b27cd2](https://github.com/wetransform/gha-workflows/commit/2b27cd27af5d08ebf4ec5dd7733229711f948566))
