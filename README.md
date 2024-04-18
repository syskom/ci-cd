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

## GitHub Composite Actions

[GitHub Composite Actions][github-composite-actions-url] are located
in [.github/actions](.github/actions) directory.

### [workflows-check](.github/actions/workflows-check)

Workflow that validates if GitHub workflow YAML file is valid.

#### Permissions

[Permissions][github-job-permissions] required by job calling the actions:

| Scope         | Value |
|---------------|-------|
| checks        | write |
| contents      | read  |
| pull-requests | write |

#### Input parameters

| Id           | Description                                           | Required | Default |
|--------------|-------------------------------------------------------|----------|---------|
| github-token | GitHub token, can be read from `secrets.GITHUB_TOKEN` | true     | none    |

#### Output parameters

No output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Reviewdog Actionlint Action][action-reviewdog-actionlint-url]

## GitHub Reusable Workflows

[GitHub Reusable Workflows][github-reusable-workflows-url] are located
in [.github/workflows](.github/workflows) directory.

### [actions-check.yaml](.github/workflows/actions-check.yaml)

Workflow that validates if workflow file is valid.

It is also used as a required workflow for PRs with workflows for this repository.

#### Input parameters

No input parameters.

#### Output parameters

No output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Reviewdog Actionlint Action][action-reviewdog-actionlint-url]

### [gradle-check.yaml](.github/workflows/gradle-check.yaml)

Workflow that runs Gradle `run` task. It uses action.

#### Input parameters

| Id                | Description                                                                                    | Required | Default | Type   |
|-------------------|------------------------------------------------------------------------------------------------|----------|---------|--------|
| java-distribution | Which Java distribution to use - https://github.com/actions/setup-java#supported-distributions | false    | temurin | string |
| java-version      | Which Java version to use - https://github.com/actions/setup-java#supported-version-syntax     | false    | 21      | string |

#### Output parameters

No output parameters.

#### Used Actions

* [Checkout Action][action-action-checkout-url]
* [Setup Java Action][action-action-setup_java-url]
* [Setup Gradle Action][action-gradle-setup_gradle-url]

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[action-action-checkout-url]: https://github.com/actions/checkout

[action-action-setup_java-url]: https://github.com/actions/setup-java

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
