<!-- PROJECT SHIELDS -->
<!--
*** Markdown "reference style" links for readability.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
[![MIT License][license-shield]][license-url]
[![Issues][issues-shield]][issues-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT DESCRIPTION -->

# Continuous Integration and Continuous Delivery (CI/CD) pipelines and tools

This repository contains a continuous integration and continuous delivery (CI/CD) workflows used by all other
repositories in [Syskom][syskom-org-url] organization.

Those workflows can be used by other organizations, but without any warranty as stated in
the [MIT License][license-url].

## GitHub Composite Actions

[GitHub Composite Actions][github-composite-actions-url] are located
in [.github/actions](.github/actions) directory.

### [workflows-check](.github/actions/workflows-check)

Action that validates GitHub Workflow YAML files located in `.github/workflows` folder.

#### Permissions

[Permissions][github-job-permissions] required by job calling the action:

| Scope         | Value | Description                              |
|---------------|-------|------------------------------------------|
| checks        | write |                                          |
| contents      | read  |                                          |
| pull-requests | write | Needed only when run on the pull request |

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
* [Setup Java Action][action-action-setup_java-url]
* [Setup Gradle Action][action-gradle-setup_gradle-url]

## Repository GitHub Workflows

### [this-main-check.yaml](.github/workflows/this-main-check.yaml)

Workflow that runs all checks on `main` branch.

#### Trigger condition

New commit merged to the `main` branch.

Can be also run manually on any branch.

#### Execution result

Info about found errors.

#### Used Actions

* [GitHub Workflows Check Action](#workflows-check) – run only for YAML files with GitHub Workflows definition
  in `.github/workflows/` folder.

### [this-pr-check.yaml](.github/workflows/this-pr-check.yaml)

Workflow that runs all checks on the Pull Request created in this repository.

#### Trigger condition

A pull request to `main` branch after pull request was created (`opened`), commits where pushed to the pull
request (`synchronized`) or closed pull request was reopened (`reopened`).

#### Execution result

If no errors will be found, `Branch protection job` will be executed with success.

#### Used Actions

* [Paths Changes Filter Action][action-dorny-paths_filter-url] – checks what part of the repository was changed.
* [GitHub Workflows Check Action](#workflows-check) – is run only for changes in `.github/workflows/` folder.

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[action-action-checkout-url]: https://github.com/actions/checkout

[action-action-setup_java-url]: https://github.com/actions/setup-java

[action-dorny-paths_filter-url]: https://github.com/dorny/paths-filter

[action-gradle-setup_gradle-url]: https://github.com/gradle/actions/blob/main/docs/setup-gradle.md

[action-reviewdog-actionlint-url]: https://github.com/reviewdog/action-actionlint

[forks-shield]: https://img.shields.io/github/forks/syskom/ci-cd.svg

[forks-url]: https://github.com/syskom/ci-cd/network/members

[github-composite-actions-url]: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

[github-job-permissions]: https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs

[github-reusable-workflows-url]: https://docs.github.com/en/actions/using-workflows/reusing-workflows

[issues-shield]: https://img.shields.io/github/issues/syskom/ci-cd.svg

[issues-url]: https://github.com/syskom/ci-cd/issues

[license-shield]: https://img.shields.io/github/license/syskom/ci-cd.svg

[license-url]: https://github.com/syskom/ci-cd/blob/main/LICENSE

[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?logo=linkedin&colorB=555

[linkedin-url]: https://linkedin.com/in/marcin-k-dabrowski

[stars-shield]: https://img.shields.io/github/stars/syskom/ci-cd.svg

[stars-url]: https://github.com/syskom/ci-cd/stargazers

[syskom-org-url]: https://github.com/syskom