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
your files are in harmony with your [.editorconfig](https://editorconfig.org/) file.

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

| Id           | Description                                                                                          | Required | Default         |
|--------------|------------------------------------------------------------------------------------------------------|----------|-----------------|
| filter-mode  | Filter mode used by Reviewdog to filter results – https://github.com/reviewdog/reviewdog#filter-mode | false    | file            |
| github-token | GitHub token, can be read from `secrets.GITHUB_TOKEN`                                                | true     | none            |
| reporter     | Reporter used by Reviewdog to report results – https://github.com/reviewdog/reviewdog#reporters      | false    | github-pr-check |

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

| Id               | Description                                                                                                                   | Required | Default                |
|------------------|-------------------------------------------------------------------------------------------------------------------------------|----------|------------------------|
| config-file-path | Path to the Renovate config file – https://docs.renovatebot.com/getting-started/installing-onboarding/#configuration-location | false    | .github/renovate.json5 |

#### Output parameters

The Action do not have output parameters.

## GitHub Reusable Workflows

[GitHub Reusable Workflows][github-reusable-workflows-url] are located
in [.github/workflows](.github/workflows) directory.

### [gradle-check.yaml](.github/workflows/gradle-check.yaml)

Workflow that runs Gradle `run` task. It uses action.

#### Input parameters

| Id                | Description                                                                                    | Required | Default | Type   |
|-------------------|------------------------------------------------------------------------------------------------|----------|---------|--------|
| java-distribution | Which Java distribution to use – https://github.com/actions/setup-java#supported-distributions | false    | temurin | string |
| java-version      | Which Java version to use – https://github.com/actions/setup-java#supported-version-syntax     | false    | 21      | string |

#### Output parameters

The Workflow do not have output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Setup Gradle Action][action-gradle-setup_gradle-url]
* [Setup Java Action][action-action-setup_java-url]

## Repository GitHub Workflows

### [Continuous Integration Build](.github/workflows/ci-main.yaml)

Workflow that runs all checks on `main` branch.

#### Trigger condition

New commit merged to the `main` branch.

Can be also run manually on any branch.

#### Execution result

Info about found errors.

#### Used Actions

* [EditorConfig Check Action](#editorconfig-check-action--editorconfig-check) – check `.editorconfig` file.
* [GitHub Workflows Check Action](#github-workflows-check-action--github-workflows-check) – check only YAML files with GitHub
  Workflows definition located in `.github/workflows/` folder.
* [Renovate Config Check Action](#renovate-config-check-action--renovate-config-check) – check `.github/renovate.json5`
  file.

### [Continuous Integration Checks](.github/workflows/ci-pr.yaml)

Workflow that runs all checks on the Pull Request created in this repository.

#### Trigger condition

On pull request to `main` branch, after:

* pull request was created (`opened`),
* commits where pushed to the pull request (`synchronized`),
* previously closed pull request was reopened (`reopened`).

#### Execution result

If no errors will be found, `Branch protection job` will be executed with success.

#### Used Actions

* [EditorConfig Check Action](#editorconfig-check-action--editorconfig-check) – runs only
  when `.editorconfig` file was changed.
* [GitHub Workflows Check Action](#github-workflows-check-action--github-workflows-check) – runs only when YAML files
  in `.github/workflows/` folder where changed.
* [Paths Changes Filter Action][action-dorny-paths_filter-url] – checks what part of the repository was changed.
* [Renovate Config Check Action](#renovate-config-check-action--renovate-config-check) – runs only
  when `.github/renovate.json5` file was changed.

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[action-action-checkout-url]: https://github.com/actions/checkout

[action-action-setup_java-url]: https://github.com/actions/setup-java

[action-dorny-paths_filter-url]: https://github.com/dorny/paths-filter

[action-editorconfig_checker-action_editorconfig_checker-url]: https://github.com/editorconfig-checker/action-editorconfig-checker

[action-gradle-setup_gradle-url]: https://github.com/gradle/actions/blob/main/docs/setup-gradle.md

[action-reviewdog-actionlint-url]: https://github.com/reviewdog/action-actionlint

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

