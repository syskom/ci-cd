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

### [EditorConfig Check Action – editorconfig-check](.github/actions/editorconfig-check)

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

### [GitHub Actions Check Action – github-actions-check](.github/actions/github-actions-check)

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

### [GitHub Workflows Check Action – github-workflows-check](.github/actions/github-workflows-check)

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

### [Renovate Config Check Action – renovate-config-check](.github/actions/renovate-config-check)

Runs `renovate-config-validator` tool to check [Renovate](https://docs.renovatebot.com/) config file.

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

## GitHub Reusable Workflows

[GitHub Reusable Workflows][github-reusable-workflows-url] are located
in [.github/workflows](.github/workflows) directory.

### [gradle-check.yaml](.github/workflows/gradle-check.yaml)

Workflow that runs Gradle `check` task.

#### Input parameters

| Id                | Description                                                                                     | Required | Default | Type   |
|-------------------|-------------------------------------------------------------------------------------------------|----------|---------|--------|
| java-distribution | Which Java distribution to use – https://github.com/actions/setup-java#supported-distributions. | false    | temurin | string |
| java-version      | Which Java version to use – https://github.com/actions/setup-java#supported-version-syntax.     | false    | 21      | string |

#### Output parameters

The Workflow do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Setup Gradle Action][action-gradle-setup_gradle-url]
* [Setup Java Action][action-action-setup_java-url]

### [reusable-lint-files.yaml](.github/workflows/reusable-lint-files.yaml)

Workflow that runs linting jobs:

* `lint-editorconfig-job` that runs [EditorConfig Check Action](#editorconfig-check-action--editorconfig-check).
* `lint-github-actions-job` that runs [GitHub Actions Check Action](#github-actions-check-action--github-actions-check).
* `lint-github-workflows-job` that runs
  [GitHub Workflows Check Action](#github-workflows-check-action--github-workflows-check).
* `lint-renovate-config-job` that runs
  [Renovate Config Check Action](#renovate-config-check-action--renovate-config-check).

By default, none job will be run. It should be enabled by input parameter.

#### Input parameters

| Id                           | Description                                                                                                         | Required | Default                                                                                              | Type    |
|------------------------------|---------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------------|---------|
| github-actions-file          | Path to the GitHub Action YAML file(s) used by `lint-github-actions-job`.                                           | false    | .github/actions/\*\*/\*.yaml \| .github/actions/**/*.yml                                             | string  |
| github-actions-schema        | Path to the GitHub Action YAML file schema used by `lint-github-actions-job`.                                       | false    | https://raw.githubusercontent.com/SchemaStore/schemastore/master/src/schemas/json/github-action.json | string  |
| github-workflows-filter-mode | Filter mode used by `lint-github-workflows-job` to filter results.                                                  | false    | nofilter                                                                                             | string  |
| github-workflows-reporter    | Reporter used by `lint-renovate-config-job` to report results.                                                      | false    | github-check                                                                                         | string  |
| lint-editorconfig            | Should run `lint-editorconfig-job` to check that all repository files are in harmony with its `.editorconfig` file. | false    | false                                                                                                | boolean |
| lint-github-actions          | Should run `lint-github-actions-job` to check GitHub Actions YAML files.                                            | false    | false                                                                                                | boolean |
| lint-github-workflows        | Should run `lint-github-workflows-job` to check GitHub Workflows YAML files.                                        | false    | false                                                                                                | boolean |
| lint-renovate-config         | Should run `lint-renovate-config-job` to check Renovate config file.                                                | false    | false                                                                                                | boolean |
| renovate-config-file-path    | Path to the Renovate config file used by `lint-renovate-config-job`.                                                | false    | .github/renovate.json5                                                                               | string  |

#### Secrets

| Id           | Description                                                                                         |
|--------------|-----------------------------------------------------------------------------------------------------|
| GITHUB_TOKEN | Access to this secret is automatically granted to `secrets.GITHUB_TOKEN` for all reusable workflows |

#### Permissions

[Permissions][github-job-permissions] required by job calling this reusable workflow:

| Scope         | Value | Description                                                               |
|---------------|-------|---------------------------------------------------------------------------|
| checks        | write |                                                                           |
| contents      | read  |                                                                           |
| pull-requests | write | Needed only when `github-workflows-reporter` has `github-pr-check` value. |

#### Output parameters

| Id                           | Description                                |
|------------------------------|--------------------------------------------|
| editorconfig-lint-result     | The result of `lint-editorconfig-job`.     |
| github-actions-lint-result   | The result of `lint-github-actions-job`.   |
| github-workflows-lint-result | The result of `lint-github-workflows-job`. |
| renovate-config-lint-result  | The result of `lint-renovate-config-job`.  |
| workflow-result              | The result of the workflow.                |

#### Used Actions

* [Alls-green Action][action-re_actor-alls_green-url] – checks if all jobs succeeded.
* [EditorConfig Check Action](#editorconfig-check-action--editorconfig-check) – runs only when `lint-editorconfig`
  parameter was set to `true`.
* [GitHub Actions Check Action](#github-actions-check-action--github-actions-check) – runs only when
  `lint-github-actions` parameter was set to `true`.
* [GitHub Workflows Check Action](#github-workflows-check-action--github-workflows-check) – runs only when
  `lint-github-workflows` parameter was set to `true`.
* [Renovate Config Check Action](#renovate-config-check-action--renovate-config-check) – runs only when
  `lint-renovate-config` parameter was set to `true`.

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

If no errors will be found, `Branch protection job` will be executed with success.

#### Used Actions and Reusable Workflows

* [Alls-green Action][action-re_actor-alls_green-url] – checks if all required jobs succeeded.
* [Paths Changes Filter Action][action-dorny-paths_filter-url] – checks what files in the repository were changed.
* [reusable-lint-files.yaml](#reusable-lint-filesyaml) – runs linting of changed files.

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[action-action-checkout-url]: https://github.com/actions/checkout

[action-action-setup_java-url]: https://github.com/actions/setup-java

[action-re_actor-alls_green-url]: https://github.com/re-actors/alls-green

[action-dorny-paths_filter-url]: https://github.com/dorny/paths-filter

[action-editorconfig_checker-action_editorconfig_checker-url]: https://github.com/editorconfig-checker/action-editorconfig-checker

[action-gradle-setup_gradle-url]: https://github.com/gradle/actions/blob/main/docs/setup-gradle.md

[action-reviewdog-actionlint-url]: https://github.com/reviewdog/action-actionlint

[action-cardinalby-schema_validator-url]: https://github.com/cardinalby/schema-validator-action

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

[github-composite-actions-url]: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

[github-job-permissions]: https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs

[github-reusable-workflows-url]: https://docs.github.com/en/actions/using-workflows/reusing-workflows

[license-url]: https://github.com/syskom/ci-cd/blob/main/LICENSE

[linkedin-url]: https://linkedin.com/in/marcin-k-dabrowski

[syskom-org-url]: https://github.com/syskom

