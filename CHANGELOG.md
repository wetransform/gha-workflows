## [2.5.0](https://github.com/wetransform/gha-workflows/compare/v2.4.0...v2.5.0) (2024-08-21)


### Features

* **gradle-library:** attempt artifact upload also on failure ([52e8930](https://github.com/wetransform/gha-workflows/commit/52e893078117417e81560a2801a38dd903e644c8))
* **gradle-library:** pass on artifact input parameters ([7256ee1](https://github.com/wetransform/gha-workflows/commit/7256ee1b185826f5c5a4fc55e79805085003e5b1))


### Bug Fixes

* **gradle-library:** fix artifact upload condition ([82bde79](https://github.com/wetransform/gha-workflows/commit/82bde79f7971feedd31a3e60a631071e10b2ff08))

## [2.4.0](https://github.com/wetransform/gha-workflows/compare/v2.3.0...v2.4.0) (2024-08-20)


### Features

* **gradle-library:** add outputs related to release ([1c0252b](https://github.com/wetransform/gha-workflows/commit/1c0252b47f7ce9ac7751c20bd8057e87bdbb09b1))
* **gradle-library:** support skipping security scan ([4586a5d](https://github.com/wetransform/gha-workflows/commit/4586a5dbf42262eb8847978c9b6d5800cd22946e))
* **gradle-library:** supported nested subprojects ([be1659a](https://github.com/wetransform/gha-workflows/commit/be1659a730590584117b3623df2a7ffa6191aa30))
* **gradle:** support specifying custom Git ref to check out ([4243206](https://github.com/wetransform/gha-workflows/commit/4243206d49cecde1ff3571b25b66e559363bc6cf))


### Bug Fixes

* **deps:** update all non-major dependencies ([6342262](https://github.com/wetransform/gha-workflows/commit/63422626a96d0a07dfcee283b45e9ffa06161751))
* **deps:** update all non-major dependencies ([03162c8](https://github.com/wetransform/gha-workflows/commit/03162c85d1e5b1623f02af9e007a9ba4f34dcb4b))
* **deps:** update all non-major dependencies ([75cadd9](https://github.com/wetransform/gha-workflows/commit/75cadd96383dbac8992cb04b296946ef32fb0a4e))
* **deps:** update all non-major dependencies ([9ca5947](https://github.com/wetransform/gha-workflows/commit/9ca5947893efdfd33a05872881f81bd7d5076fe3))
* **deps:** update gradle/actions action to v4 ([714271b](https://github.com/wetransform/gha-workflows/commit/714271be278baf135f93ed69e19bcdc252911fb0))

## [2.3.0](https://github.com/wetransform/gha-workflows/compare/v2.2.1...v2.3.0) (2024-06-28)


### Features

* **scan-images:** create combined vulnerability report ([7bb6d66](https://github.com/wetransform/gha-workflows/commit/7bb6d669a338901444964a7e8eb69e0afc4135bc))
* **scan-images:** support logging in to secondary registry ([5c14dfc](https://github.com/wetransform/gha-workflows/commit/5c14dfc561fd9f1d737a51bd439d6e1075066ba0)), closes [ING-4250](https://wetransform.atlassian.net/browse/ING-4250)
* **scan-images:** support passing in environment to use ([5de883d](https://github.com/wetransform/gha-workflows/commit/5de883df75ee761ed24ac19e859b643554f4c3b7))
* **scan-images:** support secondary registry from variable ([e2ca84d](https://github.com/wetransform/gha-workflows/commit/e2ca84d5597123e492230a775de372cf4ba4526f)), closes [ING-4250](https://wetransform.atlassian.net/browse/ING-4250)


### Bug Fixes

* **deps:** update all non-major dependencies ([8db3e0b](https://github.com/wetransform/gha-workflows/commit/8db3e0bd8980a5872a80036c30c2ba747296aa77))
* **scan-images:** disable scanning merged SBOM by default ([378360e](https://github.com/wetransform/gha-workflows/commit/378360e72a4a7feec1054a1463850864aeace8b9))

## [2.2.1](https://github.com/wetransform/gha-workflows/compare/v2.2.0...v2.2.1) (2024-06-21)


### Bug Fixes

* **deps:** update all non-major dependencies ([5bf2a0f](https://github.com/wetransform/gha-workflows/commit/5bf2a0f30fba55c2643b6f0a5a6b448e29212d21))
* **gradle-service:** check out all tags ([8b5c150](https://github.com/wetransform/gha-workflows/commit/8b5c15094e1a033b577a216dc3441b523363c51b))

## [2.2.0](https://github.com/wetransform/gha-workflows/compare/v2.1.0...v2.2.0) (2024-06-20)


### Features

* **deps:** update wetransform/gha-trivy action to v2 ([fffeaac](https://github.com/wetransform/gha-workflows/commit/fffeaac5b4426c4d5b805b7a373d856777a37bb7))
* **scan-images:** include SBOM of scanned images and merge them ([e001ea3](https://github.com/wetransform/gha-workflows/commit/e001ea3da2147b9db42b49d66bd9e8d12c3fd0a7))
* **scan-images:** remove individual SBOM and HTML report artifacts ([f804d9f](https://github.com/wetransform/gha-workflows/commit/f804d9fc88501afeb589097060acedc8a6f2d6b7))


### Bug Fixes

* **deps:** pin jaxxstorm/action-install-gh-release action to 25d5e2d ([7de1fbc](https://github.com/wetransform/gha-workflows/commit/7de1fbccf719c1a6d519d2d720717aa407e6f16f))
* **deps:** update actions/checkout action to v4.1.6 ([2e2eaa7](https://github.com/wetransform/gha-workflows/commit/2e2eaa73785b861f4c0f1c946c8c24461027f38d))
* **deps:** update all non-major dependencies ([7b16a5f](https://github.com/wetransform/gha-workflows/commit/7b16a5f20b20a0867b9f6e2b659ded7991ce6dec))
* **deps:** update all non-major dependencies ([4345049](https://github.com/wetransform/gha-workflows/commit/4345049c27f65e948b85b6d17bfa31ba38861c77))
* **deps:** update all non-major dependencies ([7af9039](https://github.com/wetransform/gha-workflows/commit/7af90394a934c92458945ad5f623e75640adf944))
* **deps:** update docker/build-push-action action to v5.4.0 ([15e42be](https://github.com/wetransform/gha-workflows/commit/15e42be328c667d85bfb45edbf72ed17bb6a0a1c))
* **deps:** update docker/build-push-action action to v6 ([2622b71](https://github.com/wetransform/gha-workflows/commit/2622b71fc26927526f357d8ed447f27d4b3860db))
* **deps:** update wetransform/gha-gradle-semantic-release action to v2.1.0 ([8caa0d2](https://github.com/wetransform/gha-workflows/commit/8caa0d29f8f8d38f42e2a21ad185cee7a0ee1c83))
* **scan-images:** fix typo ([a9c893a](https://github.com/wetransform/gha-workflows/commit/a9c893aab9f19965c3d280669e3435a160149919))

## [2.1.0](https://github.com/wetransform/gha-workflows/compare/v2.0.4...v2.1.0) (2024-05-07)


### Features

* allow to override Gradle tasks for gradle-service-publish ([2a6fc2e](https://github.com/wetransform/gha-workflows/commit/2a6fc2ee6aeac10bafcb841f1be6f780f2b5dac6))


### Bug Fixes

* **deps:** update actions/create-github-app-token action to v1.10.0 ([ef7e7ff](https://github.com/wetransform/gha-workflows/commit/ef7e7ff0d5f77914d591c783f3dcf854e3c4e204))

## [2.0.4](https://github.com/wetransform/gha-workflows/compare/v2.0.3...v2.0.4) (2024-04-25)


### Bug Fixes

* **deps:** update all non-major dependencies ([715d364](https://github.com/wetransform/gha-workflows/commit/715d364463371b152365ef6f10964b48989a0167))
* **deps:** update all non-major dependencies ([a4a9d94](https://github.com/wetransform/gha-workflows/commit/a4a9d948506ccbc3964c7203f78b5e9f298780f2))
* **deps:** update all non-major dependencies ([202e930](https://github.com/wetransform/gha-workflows/commit/202e930625aba73103c109a6162a67d7ef5040e9))
* **gradle-library:** attempt to fix condition for fetch depth ([602fcf2](https://github.com/wetransform/gha-workflows/commit/602fcf2105eeb6647c1abaf3ea34a80565fc629a))
* **gradle-library:** fetch all tags for publishing ([9f9eb56](https://github.com/wetransform/gha-workflows/commit/9f9eb56289dae7409d69a513cef5b1a0c21ebb3c))

## [2.0.3](https://github.com/wetransform/gha-workflows/compare/v2.0.2...v2.0.3) (2024-04-18)


### Bug Fixes

* **deps:** update wetransform/gha-gradle-semantic-release action to v2.0.1 ([0875fc5](https://github.com/wetransform/gha-workflows/commit/0875fc56ea2de593ea3bd201fbd9d14a997206e5))

## [2.0.2](https://github.com/wetransform/gha-workflows/compare/v2.0.1...v2.0.2) (2024-04-17)


### Bug Fixes

* **deps:** update wetransform/gha-gradle-semantic-release action to v2 ([82d4182](https://github.com/wetransform/gha-workflows/commit/82d41826cd391ab503383c849c1425ca8e714b1b))

## [2.0.1](https://github.com/wetransform/gha-workflows/compare/v2.0.0...v2.0.1) (2024-04-15)


### Bug Fixes

* only generate release token if required ([52a47bc](https://github.com/wetransform/gha-workflows/commit/52a47bca3b128d737da2acf69d3e9f1bec3c8f95))

## [2.0.0](https://github.com/wetransform/gha-workflows/compare/v1.1.7...v2.0.0) (2024-04-12)


### ⚠ BREAKING CHANGES

* Configuration of Gradle tasks to run now split into two
different inputs.

### Features

* support expecting no tests for Gradle library workflows ([e4266c0](https://github.com/wetransform/gha-workflows/commit/e4266c0dc958efeccebe387e62a2c8ab27a4713e))
* support semantic release for Gradle libraries ([a30e8de](https://github.com/wetransform/gha-workflows/commit/a30e8dee7854867573a1a5b96619cdedb6b55083))


### Bug Fixes

* **deps:** pin dependencies ([4dee22b](https://github.com/wetransform/gha-workflows/commit/4dee22b66d0ab35ff920501f555c04f9b9856c86))
* **deps:** update gradle/actions action to v3.3.0 ([be3b722](https://github.com/wetransform/gha-workflows/commit/be3b722ce961dcc838cc8792f30215202a585015))
* fix incompatibility with semantic-release action ([d2c3be8](https://github.com/wetransform/gha-workflows/commit/d2c3be8ab4e238fda1664c72d566090377203cbc))
* use app token for release ([063ab9e](https://github.com/wetransform/gha-workflows/commit/063ab9e8a502075d5488e8ccdfb9dfdae219d93b))
* use concrete versions for all used actions ([fd56f3b](https://github.com/wetransform/gha-workflows/commit/fd56f3bd438cb805a10f35512d6518daa69bf64b))

## [1.1.7](https://github.com/wetransform/gha-workflows/compare/v1.1.6...v1.1.7) (2024-04-08)


### Bug Fixes

* **deps:** update all non-major dependencies ([6819ce9](https://github.com/wetransform/gha-workflows/commit/6819ce902bfe5697274f687a7eab89723ff1a322))

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
