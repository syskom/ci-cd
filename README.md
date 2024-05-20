<!-- PROJECT SHIELDS -->
<!--
*** Markdown "reference style" links for readability.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

[![MIT License][badge-license-shield]][license-url]
[![GitHub Workflow Status][badge-build-shield]][badge-build-url]
[![Maintained][badge-maintained-shield]][badge-maintained-url]
[![GitHub latest commit][badge-last-commit-shield]][badge-last-commit-url]
[![Issues][badge-issues-shield]][badge-issues-url]
[![Pull requests][badge-pr-shield]][badge-pr-url]
[![Hits-of-Code][badge-hits_of_code-shield]][badge-hits_of_code-url]
[![Forks][badge-forks-shield]][badge-forks-url]
[![Stargazers][badge-stars-shield]][badge-stars-url]
[![LinkedIn][badge-linkedin-shield]][linkedin-url]

<!-- PROJECT DESCRIPTION -->

# Continuous Integration and Continuous Delivery (CI/CD) pipelines and tools

This repository contains a continuous integration and continuous delivery (CI/CD) workflows used by all other
repositories in the [Syskom][syskom-org-url] organization.

Those workflows can be used by other organizations, but without any warranty as stated in
the [MIT License][license-url].

## GitHub Composite Actions

[GitHub Composite Actions][github-composite-actions-url] are located
in [.github/actions](.github/actions) directory.

### [EditorConfig Lint Composite Action – editorconfig-lint](.github/actions/editorconfig-lint)

Action that Runs [editorconfig-checker](https://github.com/editorconfig-checker/editorconfig-checker) to verify that
all repository files are in harmony with its [.editorconfig](https://editorconfig.org/) file.

#### Permissions

[Permissions][github-job-permissions] required by job calling this action:

| Scope    | Value | Description |
|----------|-------|-------------|
| contents | read  |             |

#### Input parameters

The Action do not have input parameters.

#### Output parameters

The Action do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [EditorConfig Action][action-editorconfig_checker-action_editorconfig_checker-url]

### [GitHub Actions Lint Composite Action – github-actions-lint](.github/actions/github-actions-lint)

Action that validates [GitHub Actions](https://docs.github.com/en/actions/quickstart) YAML files located in folder
denoted by `file` parameter with the JSON schema of GitHub Action YAML file pointed by `schema` parameter.

#### Permissions

[Permissions][github-job-permissions] required by job calling this action:

| Scope    | Value | Description |
|----------|-------|-------------|
| contents | read  |             |

#### Input parameters

| Id     | Description                                                                                                                                                                 | Required | Default                                                                                              |
|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------------|
| file   | Path to the GitHub Action YAML file(s). Can accept a glob pattern (will validate all matched files). Can accept multiple files (or glob patterns) separated by `\|` symbol. | false    | .github/actions/\*\*/\*.yaml \| .github/actions/**/*.yml                                             |
| schema | JSON schema of GitHub Action YAML file.                                                                                                                                     | false    | https://raw.githubusercontent.com/SchemaStore/schemastore/master/src/schemas/json/github-action.json |

#### Output parameters

The Action do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [JSON Schema validator Action][action-cardinalby-schema_validator-url]

### [GitHub Workflows Lint Composite Action – github-workflows-lint](.github/actions/github-workflows-lint)

Action that validates [GitHub Workflow](https://docs.github.com/en/actions/using-workflows) YAML files located
in `.github/workflows` folder.

#### Permissions

[Permissions][github-job-permissions] required by job calling this action:

| Scope         | Value | Description                               |
|---------------|-------|-------------------------------------------|
| checks        | write |                                           |
| contents      | read  |                                           |
| pull-requests | write | Needed only when runs on the pull request |

#### Input parameters

| Id           | Description                                                                                           | Required | Default         |
|--------------|-------------------------------------------------------------------------------------------------------|----------|-----------------|
| filter-mode  | Filter mode used by Reviewdog to filter results – https://github.com/reviewdog/reviewdog#filter-mode. | false    | file            |
| github-token | GitHub token, can be read from `secrets.GITHUB_TOKEN`.                                                | true     | none            |
| reporter     | Reporter used by Reviewdog to report results – https://github.com/reviewdog/reviewdog#reporters.      | false    | github-pr-check |

#### Output parameters

The Action do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Reviewdog Actionlint Action][action-reviewdog-actionlint-url]

### [Renovate Config Lint Composite Action – renovate-config-lint](.github/actions/renovate-config-lint)

Runs `renovate-config-validator` tool to lint [Renovate](https://docs.renovatebot.com/) config file.

#### Permissions

[Permissions][github-job-permissions] required by job calling this action:

| Scope    | Value | Description |
|----------|-------|-------------|
| contents | read  |             |

#### Input parameters

| Id               | Description                                                                                                                    | Required | Default                |
|------------------|--------------------------------------------------------------------------------------------------------------------------------|----------|------------------------|
| config-file-path | Path to the Renovate config file – https://docs.renovatebot.com/getting-started/installing-onboarding/#configuration-location. | false    | .github/renovate.json5 |

#### Output parameters

The Action do not have output parameters.

### [YAML Lint Composite Action – yaml-lint](.github/actions/yaml-lint)

Action that lints [YAML](https://yaml.org) files.

#### Permissions

[Permissions][github-job-permissions] required by job calling this action:

| Scope         | Value | Description                               |
|---------------|-------|-------------------------------------------|
| checks        | write |                                           |
| contents      | read  |                                           |
| pull-requests | write | Needed only when runs on the pull request |

#### Input parameters

| Id           | Description                                                                                           | Required | Default         |
|--------------|-------------------------------------------------------------------------------------------------------|----------|-----------------|
| filter-mode  | Filter mode used by Reviewdog to filter results – https://github.com/reviewdog/reviewdog#filter-mode. | false    | file            |
| github-token | GitHub token, can be read from `secrets.GITHUB_TOKEN`.                                                | true     | none            |
| reporter     | Reporter used by Reviewdog to report results – https://github.com/reviewdog/reviewdog#reporters.      | false    | github-pr-check |

#### Output parameters

The Action do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Reviewdog Yamllint Action][action-reviewdog-yamllint-url]

## GitHub Reusable Workflows

[GitHub Reusable Workflows][github-reusable-workflows-url] are located
in [.github/workflows](.github/workflows) directory.

### [reusable-gradle-check.yaml](.github/workflows/reusable-gradle-check.yaml)

Workflow that runs Gradle `check` task.

#### Permissions

[Permissions][github-job-permissions] required by job calling this action:

| Scope         | Value | Description                                                                                     |
|---------------|-------|-------------------------------------------------------------------------------------------------|
| contents      | read  |                                                                                                 |
| pull-requests | write | Needed only when `add-gradle-job-summary-as-pr-comment` input parameter is not equal to `never` |

#### Input parameters

| Id                                   | Description                                                                                                                                                            | Required | Default | Type    |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|---------|---------|
| add-gradle-job-summary-as-pr-comment | Add Gradle Job Summary data as a Pull Request comment – https://github.com/gradle/actions/blob/main/docs/setup-gradle.md#adding-job-summary-as-a-pull-request-comment. | false    | never   | string  |
| java-distribution                    | Which Java distribution to use – https://github.com/actions/setup-java#supported-distributions.                                                                        | false    | temurin | string  |
| java-version                         | Which Java version to use – https://github.com/actions/setup-java#supported-version-syntax.                                                                            | false    | 21      | string  |
| run-sonar-scan                       | Should run [SonarCloud](https://sonarcloud.io) scan.                                                                                                                   | false    | false   | boolean |
| sonar-timeout-seconds                | The number of seconds that the build should wait for a report to be processed. The default is 300 seconds.                                                             | false    | 300     | number  |

#### Output parameters

The Workflow do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Setup Gradle Action][action-gradle-setup_gradle-url]
* [Setup Java Action][action-action-setup_java-url]

### [reusable-gradle-codeql.yaml](.github/workflows/reusable-gradle-codeql.yaml)

Workflow that performs [CodeQL][codeql-url] analysis on code compiled by Gradle.

#### Permissions

[Permissions][github-job-permissions] required by job calling this action:

| Scope           | Value | Description                                         |
|-----------------|-------|-----------------------------------------------------|
| contents        | read  |                                                     |
| security-events | write | https://github.com/github/codeql-action#permissions |

#### Input parameters

| Id                | Description                                                                                     | Required | Default | Type   |
|-------------------|-------------------------------------------------------------------------------------------------|----------|---------|--------|
| java-distribution | Which Java distribution to use – https://github.com/actions/setup-java#supported-distributions. | false    | temurin | string |
| java-version      | Which Java version to use – https://github.com/actions/setup-java#supported-version-syntax.     | false    | 21      | string |

#### Output parameters

The Workflow do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [CodeQL Action][action-github-codeql-url]
* [Setup Gradle Action][action-gradle-setup_gradle-url]
* [Setup Java Action][action-action-setup_java-url]

### [reusable-lint-files.yaml](.github/workflows/reusable-lint-files.yaml)

Workflow that runs linting jobs:

* `lint-editorconfig-job` that
  runs [EditorConfig Lint Composite Action](#editorconfig-lint-composite-action--editorconfig-lint).
* `lint-github-actions-job` that
  runs [GitHub Actions Lint Composite Action](#github-actions-lint-composite-action--github-actions-lint).
* `lint-github-workflows-job` that runs
  [GitHub Workflows Lint Composite Action](#github-workflows-lint-composite-action--github-workflows-lint).
* `lint-renovate-config-job` that runs
  [Renovate Config Lint Composite Action](#renovate-config-lint-composite-action--renovate-config-lint).
* `lint-yaml-files-job` that runs [YAML Lint Composite Action](#yaml-lint-composite-action--yaml-lint).

By default, none job will be run. It should be enabled by input parameter.

#### Input parameters

| Id                                | Description                                                                                                         | Required | Default                                                                                              | Type    |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------------|---------|
| lint-editorconfig                 | Should run `lint-editorconfig-job` to check that all repository files are in harmony with its `.editorconfig` file. | false    | false                                                                                                | boolean |
| lint-github-actions               | Should run `lint-github-actions-job` to lint GitHub Actions YAML files.                                             | false    | false                                                                                                | boolean |
| lint-github-actions-file          | Path to the GitHub Action YAML file(s) used by `lint-github-actions-job`.                                           | false    | .github/actions/\*\*/\*.yaml \| .github/actions/**/*.yml                                             | string  |
| lint-github-actions-schema        | Path to the GitHub Action YAML file schema used by `lint-github-actions-job`.                                       | false    | https://raw.githubusercontent.com/SchemaStore/schemastore/master/src/schemas/json/github-action.json | string  |
| lint-github-workflows             | Should run `lint-github-workflows-job` to lint GitHub Workflows YAML files.                                         | false    | false                                                                                                | boolean |
| lint-github-workflows-filter-mode | Filter mode used by `lint-github-workflows-job` to filter results.                                                  | false    | nofilter                                                                                             | string  |
| lint-github-workflows-reporter    | Reporter used by `lint-github-workflows-job` to report results.                                                     | false    | github-check                                                                                         | string  |
| lint-renovate-config              | Should run `lint-renovate-config-job` to lint Renovate config file.                                                 | false    | false                                                                                                | boolean |
| lint-renovate-config-file-path    | Path to the Renovate config file used by `lint-renovate-config-job`.                                                | false    | .github/renovate.json5                                                                               | string  |
| lint-yaml-files                   | Should run `lint-yaml-files-job` to lint YAML files.                                                                | false    | false                                                                                                | boolean |
| lint-yaml-files-filter-mode       | Filter mode used by `lint-yaml-files-job` to filter results.                                                        | false    | nofilter                                                                                             | string  |
| lint-yaml-files-reporter          | Reporter used by `lint-yaml-files-job` to report results.                                                           | false    | github-check                                                                                         | string  |

#### Secrets

| Id           | Description                                                                                         |
|--------------|-----------------------------------------------------------------------------------------------------|
| GITHUB_TOKEN | Access to this secret is automatically granted to `secrets.GITHUB_TOKEN` for all reusable workflows |

#### Permissions

[Permissions][github-job-permissions] required by job calling this reusable workflow:

| Scope         | Value | Description                                                                                                  |
|---------------|-------|--------------------------------------------------------------------------------------------------------------|
| checks        | write |                                                                                                              |
| contents      | read  |                                                                                                              |
| pull-requests | write | Needed only when `lint-github-workflows-reporter` or `lint-yaml-files-reporter` has `github-pr-check` value. |

#### Output parameters

| Id                           | Description                                |
|------------------------------|--------------------------------------------|
| editorconfig-lint-result     | The result of `lint-editorconfig-job`.     |
| github-actions-lint-result   | The result of `lint-github-actions-job`.   |
| github-workflows-lint-result | The result of `lint-github-workflows-job`. |
| renovate-config-lint-result  | The result of `lint-renovate-config-job`.  |
| workflow-result              | The result of the workflow.                |
| yaml-lint-result             | The result of `lint-yaml-files-job`.       |

#### Used Actions

* [Alls-green Action][action-re_actors-alls_green-url] – checks if all jobs succeeded.
* [EditorConfig Lint Composite Action](#editorconfig-lint-composite-action--editorconfig-lint) – runs only when
  `lint-editorconfig` parameter was set to `true`.
* [GitHub Actions Lint Composite Action](#github-actions-lint-composite-action--github-actions-lint) – runs only
  when `lint-github-actions` parameter was set to `true`.
* [GitHub Workflows Lint Composite Action](#github-workflows-lint-composite-action--github-workflows-lint) – runs
  only when `lint-github-workflows` parameter was set to `true`.
* [Renovate Config Lint Composite Action](#renovate-config-lint-composite-action--renovate-config-lint) – runs only
  when `lint-renovate-config` parameter was set to `true`.
* [YAML Lint Composite Action](#yaml-lint-composite-action--yaml-lint) – runs only when `lint-yaml-files` parameter
  was set to `true`.

## Repository GitHub Workflows

### [Continuous Integration Build](.github/workflows/ci-main.yaml)

Workflow that runs all checks on `main` branch.

#### Trigger condition

New commits pushed to the `main` branch.

Can be also run manually on any branch.

#### Execution result

Info about found errors.

#### Used Actions and Reusable Workflows

* [reusable-lint-files.yaml](#reusable-lint-filesyaml)

### [Continuous Integration Checks](.github/workflows/ci-pr.yaml)

Workflow that runs all checks on the Pull Request created in this repository.

#### Trigger condition

On pull request to `main` branch, after:

* pull request was created (`opened`),
* commits where pushed to the pull request (`synchronized`),
* previously closed pull request was reopened (`reopened`).

#### Execution result

If no errors will be found, `Check if all required jobs succeeded` job will be executed with success.

#### Used Actions and Reusable Workflows

* [Alls-green Action][action-re_actors-alls_green-url] – checks if all required jobs succeeded.
* [Paths Changes Filter Action][action-dorny-paths_filter-url] – checks what files in the repository were changed.
* [reusable-lint-files.yaml](#reusable-lint-filesyaml) – runs linting of changed files.

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[action-action-checkout-url]: https://github.com/actions/checkout

[action-action-setup_java-url]: https://github.com/actions/setup-java

[action-cardinalby-schema_validator-url]: https://github.com/cardinalby/schema-validator-action

[action-dorny-paths_filter-url]: https://github.com/dorny/paths-filter

[action-editorconfig_checker-action_editorconfig_checker-url]: https://github.com/editorconfig-checker/action-editorconfig-checker

[action-github-codeql-url]: https://github.com/github/codeql-action

[action-gradle-setup_gradle-url]: https://github.com/gradle/actions/blob/main/docs/setup-gradle.md

[action-reviewdog-actionlint-url]: https://github.com/reviewdog/action-actionlint

[action-reviewdog-yamllint-url]: https://github.com/reviewdog/action-yamllint

[action-re_actors-alls_green-url]: https://github.com/re-actors/alls-green

[badge-build-shield]: https://img.shields.io/github/actions/workflow/status/syskom/ci-cd/ci-main.yaml?branch=main

[badge-build-url]: https://github.com/syskom/ci-cd/actions/workflows/ci-main.yaml?query=branch%3Amain+event%3Apush

[badge-forks-shield]: https://img.shields.io/github/forks/syskom/ci-cd.svg

[badge-forks-url]: https://github.com/syskom/ci-cd/network/members

[badge-hits_of_code-shield]: https://hitsofcode.com/github/syskom/ci-cd?branch=main&label=Hits-of-Code

[badge-hits_of_code-url]: https://hitsofcode.com/github/syskom/ci-cd/view?branch=main&label=Hits-of-Code

[badge-issues-shield]: https://img.shields.io/github/issues/syskom/ci-cd.svg

[badge-issues-url]: https://github.com/syskom/ci-cd/issues

[badge-last-commit-shield]: https://badgen.net/github/last-commit/syskom/ci-cd

[badge-last-commit-url]: https://GitHub.com/syskom/ci-cd/commit/

[badge-license-shield]: https://img.shields.io/github/license/syskom/ci-cd.svg

[badge-linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?logo=linkedin&colorB=555

[badge-maintained-shield]: https://img.shields.io/badge/maintained%3F-yes-green.svg

[badge-maintained-url]: https://github.com/syskom/ci-cd/graphs/commit-activity

[badge-pr-shield]: https://img.shields.io/github/issues-pr/syskom/ci-cd.svg

[badge-pr-url]: https://GitHub.com/syskom/ci-cd/pull

[badge-stars-shield]: https://img.shields.io/github/stars/syskom/ci-cd.svg

[badge-stars-url]: https://github.com/syskom/ci-cd/stargazers

[codeql-url]: https://codeql.github.com/docs/codeql-overview/about-codeql/

[github-composite-actions-url]: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

[github-job-permissions]: https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs

[github-reusable-workflows-url]: https://docs.github.com/en/actions/using-workflows/reusing-workflows

[license-url]: https://github.com/syskom/ci-cd/blob/main/LICENSE

[linkedin-url]: https://linkedin.com/in/marcin-k-dabrowski

[syskom-org-url]: https://github.com/syskom

