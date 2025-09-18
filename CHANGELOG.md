## [3.6.0](https://github.com/wetransform/gha-workflows/compare/v3.5.0...v3.6.0) (2025-09-18)

### Features

* support private submodules ([3630390](https://github.com/wetransform/gha-workflows/commit/36303905c9cacdfe9db8392328d1c9b68783ed88)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)

## [3.5.0](https://github.com/wetransform/gha-workflows/compare/v3.4.5...v3.5.0) (2025-09-18)

### Features

* **mise:** do repository vulnerability scan ([8a8c8bf](https://github.com/wetransform/gha-workflows/commit/8a8c8bfeb79d33c6fbead93093b9d2535db6b39d)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)
* **mise:** first version of mise check workflow ([a4a3a52](https://github.com/wetransform/gha-workflows/commit/a4a3a52179b937dbcb20d4e8fd6049e767346637)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)
* **mise:** first version of mise publish workflow ([b896a58](https://github.com/wetransform/gha-workflows/commit/b896a585727e456180b06f14bb0018a07f1233d7)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)
* **mise:** first version of mise release workflow ([af0a1b0](https://github.com/wetransform/gha-workflows/commit/af0a1b05f86dce19c7d8d13d46bc1a507f4365fd)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)
* **mise:** support configuring submodules for checkout ([565bcb2](https://github.com/wetransform/gha-workflows/commit/565bcb202b69816b1e62aa455bf35a6a9debb5d9)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)
* **mise:** support GH_PAT env variable if secret is present ([a789064](https://github.com/wetransform/gha-workflows/commit/a78906408875fd83c925f4ceb1b07e13515e3664)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)
* **mise:** support security scan for Docker images ([35dd8a5](https://github.com/wetransform/gha-workflows/commit/35dd8a54b13855de4af85bcaecc10c3ba07e57a6)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)

### Bug Fixes

* **deps:** update actions/cache action to v4 ([c5f75ca](https://github.com/wetransform/gha-workflows/commit/c5f75cadd7a9da468aa6840f4f10726891a4dc99))
* **deps:** update actions/checkout action to v5 ([b3506f3](https://github.com/wetransform/gha-workflows/commit/b3506f39604ec249b0ba4a7f342eba3cac7e03b8))
* **deps:** update actions/create-github-app-token action to v2.1.4 ([e24be71](https://github.com/wetransform/gha-workflows/commit/e24be71dca7eac30346e4133f256d2e3c878cb32))
* **deps:** update actions/create-github-app-token action to v2.1.4 ([1002fcd](https://github.com/wetransform/gha-workflows/commit/1002fcd4405d1123bd055c2b3a4b1478891fbddf))
* **deps:** update cycjimmy/semantic-release-action action to v5 ([93a9961](https://github.com/wetransform/gha-workflows/commit/93a9961c6e9bf967400224089152ae6b1da89a19))
* **deps:** update jdx/mise-action action to v3 ([53c5dfe](https://github.com/wetransform/gha-workflows/commit/53c5dfe9212dd8c446aac384fd0c70cd4d14618c))
* **deps:** update wetransform/gha-gradle-semantic-release action to v2.1.4 ([e7efd53](https://github.com/wetransform/gha-workflows/commit/e7efd5332727c0c0c016acbd806065b07e4d34f1))
* **mise:** fix typo in reference ([adeb57d](https://github.com/wetransform/gha-workflows/commit/adeb57db24f36e5c558ae67514a445893ca15a26)), closes [ING-4956](https://wetransform.atlassian.net/browse/ING-4956)

## [3.4.5](https://github.com/wetransform/gha-workflows/compare/v3.4.4...v3.4.5) (2025-09-12)

### Bug Fixes

* **deps:** update actions/checkout action to v5 ([6f6f569](https://github.com/wetransform/gha-workflows/commit/6f6f569321860b0584358c15060012c5d11bce05))
* **deps:** update actions/download-artifact action to v5 ([0a7d28d](https://github.com/wetransform/gha-workflows/commit/0a7d28dace3e3c8de08407277150da5bdb9db475))
* **deps:** update actions/setup-java action to v5 ([c484f59](https://github.com/wetransform/gha-workflows/commit/c484f59f41b3321d2755107124fa3e6f147db3d7))
* **deps:** update all non-major dependencies ([16cffe6](https://github.com/wetransform/gha-workflows/commit/16cffe69b9ba891cd07aa5ea3fb70cf1a507b988))
* **deps:** update all non-major dependencies ([955543d](https://github.com/wetransform/gha-workflows/commit/955543d62012ad4408ce89d522060f47615e4d32))
* **deps:** update all non-major dependencies ([9294652](https://github.com/wetransform/gha-workflows/commit/929465275707b933a34e833d0818293abb545331))

## [3.4.4](https://github.com/wetransform/gha-workflows/compare/v3.4.3...v3.4.4) (2025-07-25)

### Bug Fixes

* **deps:** update actions/create-github-app-token action to v2 ([b6c8970](https://github.com/wetransform/gha-workflows/commit/b6c8970b9f123e7fde363979cbf2498c9ff19e21))
* **deps:** update actions/create-github-app-token action to v2.0.6 ([9a67cc5](https://github.com/wetransform/gha-workflows/commit/9a67cc5b45c19dea80b4f13c7106fe566bfbf9d2))
* **deps:** update actions/setup-java action to v4.7.1 ([3ebc69b](https://github.com/wetransform/gha-workflows/commit/3ebc69b2a11a84721486d8c64af862aae6a77f40))
* **deps:** update all non-major dependencies ([7bb7b32](https://github.com/wetransform/gha-workflows/commit/7bb7b3283e678709818a34dc46e21323ab62372b))
* **deps:** update all non-major dependencies ([4c85e59](https://github.com/wetransform/gha-workflows/commit/4c85e59d0c2aaf533834ab95264b3b8aa841ff95))
* **deps:** update all non-major dependencies ([a0b2cde](https://github.com/wetransform/gha-workflows/commit/a0b2cdedfe03fd28ae0dfd44449399cacb2a217e))
* **deps:** update all non-major dependencies ([8f80081](https://github.com/wetransform/gha-workflows/commit/8f8008105da2bfbb48f5420adb6d2a6c5441dffe))
* **deps:** update all non-major dependencies ([c527958](https://github.com/wetransform/gha-workflows/commit/c5279586d697b4c6cb3f815259d4b23dff747188))
* **deps:** update all non-major dependencies ([febcc53](https://github.com/wetransform/gha-workflows/commit/febcc53517ef8ed5fc2e37a43ca691e3dbb2d5f8))
* **deps:** update wetransform/gha-gradle-semantic-release action to v2.1.2 ([022e53e](https://github.com/wetransform/gha-workflows/commit/022e53ead2b94ba306b1e19dd5ae18a6fa6ddbf3))
* **deps:** update wetransform/gha-trivy action to v2.3.5 ([d514385](https://github.com/wetransform/gha-workflows/commit/d514385201278927b4e030ad0313cb247f39d7d6))

## [3.4.3](https://github.com/wetransform/gha-workflows/compare/v3.4.2...v3.4.3) (2025-03-25)

### Bug Fixes

* specify and pass on secrets explicitly ([edea881](https://github.com/wetransform/gha-workflows/commit/edea88147a62a5c22da3c4d447c7d2ae1a04bd85))

## [3.4.2](https://github.com/wetransform/gha-workflows/compare/v3.4.1...v3.4.2) (2025-03-24)

### Bug Fixes

* **deps:** update actions/create-github-app-token action to v1.11.1 ([852e666](https://github.com/wetransform/gha-workflows/commit/852e666926a563124f34e7768d64cea57d84503a))
* **deps:** update actions/create-github-app-token action to v1.11.5 ([368af87](https://github.com/wetransform/gha-workflows/commit/368af875bc3bfef12bf60a5fca62a0bb9f2286e1))
* **deps:** update actions/create-github-app-token action to v1.11.6 ([cbe2cea](https://github.com/wetransform/gha-workflows/commit/cbe2cea2499eec3b559e1116ab4f2453e1b74e91))
* **deps:** update actions/upload-artifact action to v4.6.0 ([dd904f9](https://github.com/wetransform/gha-workflows/commit/dd904f9d25dfdfc473b41ada5d8a8583737df7b3))
* **deps:** update all non-major dependencies ([433f5e5](https://github.com/wetransform/gha-workflows/commit/433f5e54299a210f54c16bd40c6b5ec313ba7e81))
* **deps:** update all non-major dependencies ([8e8f1e6](https://github.com/wetransform/gha-workflows/commit/8e8f1e695dcd449d015983af680e02e9838075dd))
* **deps:** update all non-major dependencies ([9e79329](https://github.com/wetransform/gha-workflows/commit/9e79329d1c096874e031b64346220ba483880ab2))
* **deps:** update all non-major dependencies ([61856ff](https://github.com/wetransform/gha-workflows/commit/61856ff3b7cbb0454c1be26caeb8a11e290d743a))
* **deps:** update all non-major dependencies ([c1196ae](https://github.com/wetransform/gha-workflows/commit/c1196aeb8fba682bff7ed0768c30d3c9a08020fb))
* **deps:** update docker/build-push-action action to v6.11.0 ([ed5b02f](https://github.com/wetransform/gha-workflows/commit/ed5b02f3cdc0ccae8ce55735e38fe56725bfab86))
* **deps:** update docker/build-push-action action to v6.13.0 ([0773125](https://github.com/wetransform/gha-workflows/commit/0773125f8fa0c805617387741ff67bf4230df31a))
* **deps:** update docker/login-action action to v3.4.0 ([134722e](https://github.com/wetransform/gha-workflows/commit/134722ed264200017131aeaf4810ce6dbd460c71))
* **deps:** update mikepenz/action-junit-report action to v5.5.0 ([640e4d6](https://github.com/wetransform/gha-workflows/commit/640e4d66422a53e23d0a43337c39c32cf522acb0))

## [3.4.1](https://github.com/wetransform/gha-workflows/compare/v3.4.0...v3.4.1) (2024-12-18)

### Bug Fixes

* **deps:** update all non-major dependencies ([cc875b3](https://github.com/wetransform/gha-workflows/commit/cc875b38e23a055c5e10f0c32ad62937207bc125))
* **deps:** update all non-major dependencies ([4e3713e](https://github.com/wetransform/gha-workflows/commit/4e3713e4fd85f45bcf62e0e4e90cbcb1f87fe617))

## [3.4.0](https://github.com/wetransform/gha-workflows/compare/v3.3.2...v3.4.0) (2024-12-03)

### Features

* support using custom runner based on variable ([2597a86](https://github.com/wetransform/gha-workflows/commit/2597a86d9e7b1d138fa3b00202d311a9e3e686d8))

### Bug Fixes

* **deps:** update all non-major dependencies ([602e4a0](https://github.com/wetransform/gha-workflows/commit/602e4a0db4681f0f50f0d987fe7d549945174902))

## [3.3.2](https://github.com/wetransform/gha-workflows/compare/v3.3.1...v3.3.2) (2024-11-21)

### Bug Fixes

* **gradle-service:** add missing secret declaration ([64f333a](https://github.com/wetransform/gha-workflows/commit/64f333a4911bea175d625911e311238fb9acfe46))

## [3.3.1](https://github.com/wetransform/gha-workflows/compare/v3.3.0...v3.3.1) (2024-11-21)

### Bug Fixes

* **deps:** update all non-major dependencies ([e5bf273](https://github.com/wetransform/gha-workflows/commit/e5bf2738c50e03839500ffa3eead976699741f0d))

## [3.3.0](https://github.com/wetransform/gha-workflows/compare/v3.2.1...v3.3.0) (2024-11-19)

### Features

* **dockerfile:** support passing in custom checkout ref ([6971128](https://github.com/wetransform/gha-workflows/commit/6971128da8796211eb8458263e9d8891267266a1))

## [3.2.1](https://github.com/wetransform/gha-workflows/compare/v3.2.0...v3.2.1) (2024-11-18)

### Bug Fixes

* **deps:** update all non-major dependencies ([85c4f89](https://github.com/wetransform/gha-workflows/commit/85c4f897e25769d9abead1c0bf6435b012e1ab6f))
* **deps:** update mikepenz/action-junit-report action to v5 ([18440a9](https://github.com/wetransform/gha-workflows/commit/18440a997db3e982aa9ced1f2862bffe40d84322))

## [3.2.0](https://github.com/wetransform/gha-workflows/compare/v3.1.1...v3.2.0) (2024-11-16)

### Features

* **gradle:** support private submodules ([b25037a](https://github.com/wetransform/gha-workflows/commit/b25037a5497625456c666b92d0dd80fe6e37df39))

### Bug Fixes

* **deps:** update actions/setup-java action to v4.5.0 ([7d4f56b](https://github.com/wetransform/gha-workflows/commit/7d4f56b37bfc1a5bbad7a48ea7d2c570f66457dd))
* **deps:** update all non-major dependencies ([4b90a85](https://github.com/wetransform/gha-workflows/commit/4b90a85493279c480aa466d7962c383a752fe449))

## [3.1.1](https://github.com/wetransform/gha-workflows/compare/v3.1.0...v3.1.1) (2024-10-24)

### Bug Fixes

* **deps:** update all non-major dependencies ([56654b2](https://github.com/wetransform/gha-workflows/commit/56654b25d1b407f90f7b35072b4cf05c3d9586cc))
* **deps:** update all non-major dependencies ([84f4033](https://github.com/wetransform/gha-workflows/commit/84f40334a23404c6bf8b83daad78d30c1ac62cf2))

## [3.1.0](https://github.com/wetransform/gha-workflows/compare/v3.0.0...v3.1.0) (2024-10-11)

### Features

* **gradle:** login to Docker Hub if credentials are provided ([646fe8e](https://github.com/wetransform/gha-workflows/commit/646fe8eb174bf6b02ad84776113b36c4ff58c8db))

## [3.0.0](https://github.com/wetransform/gha-workflows/compare/v2.5.1...v3.0.0) (2024-10-02)

### ⚠ BREAKING CHANGES

* **gradle-service:** Gradle task configuration for Gradle services now split
across two input variables to separate checks from publishing.

### Features

* **gradle-service:** support semantic release ([721629b](https://github.com/wetransform/gha-workflows/commit/721629bd9239f4ec25c407b9960d0855a268a293))

### Bug Fixes

* **deps:** update docker/build-push-action action to v6.8.0 ([d69c9d7](https://github.com/wetransform/gha-workflows/commit/d69c9d7e0c9c3cd53da94033635959a7c4b0e283))
* **deps:** update docker/build-push-action action to v6.9.0 ([ac3db1a](https://github.com/wetransform/gha-workflows/commit/ac3db1aacec428a72efe71bbe074594d04555546))

## [2.5.1](https://github.com/wetransform/gha-workflows/compare/v2.5.0...v2.5.1) (2024-09-26)

### Bug Fixes

* **deps:** update actions/create-github-app-token action to v1.10.4 ([73e877b](https://github.com/wetransform/gha-workflows/commit/73e877b606232f60e3350c5101d54e5f89dc5a1f))
* **deps:** update all non-major dependencies ([07bbc00](https://github.com/wetransform/gha-workflows/commit/07bbc00d01184b087273f62b41686d9d68a0d3fe))
* **deps:** update all non-major dependencies ([2ef3de6](https://github.com/wetransform/gha-workflows/commit/2ef3de66173746db44678c0aaf5be0ce01a4e145))
* **deps:** update all non-major dependencies ([9894f6b](https://github.com/wetransform/gha-workflows/commit/9894f6bd6be411d26e59619fece34d7ee554410c))
* **deps:** update all non-major dependencies ([9106618](https://github.com/wetransform/gha-workflows/commit/9106618de40cb6393b8ebca40c439de3de3c516d))

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
