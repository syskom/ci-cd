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

# Continuous Integration and Continuous Delivery (CI/CD) pipelines and tools

This repository contains a continuous integration and continuous delivery (CI/CD) workflows used by all other
repositories in [Syskom][syskom-org-url] organization.

## GitHub reusable workflows

[GitHub reusable workflows][reusable-workflows-url] are located
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

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[action-action-checkout-url]: https://github.com/actions/checkout

[action-reviewdog-actionlint-url]: https://github.com/reviewdog/action-actionlint

[forks-shield]: https://img.shields.io/github/forks/syskom/ci-cd.svg

[forks-url]: https://github.com/syskom/ci-cd/network/members

[issues-shield]: https://img.shields.io/github/issues/syskom/ci-cd.svg

[issues-url]: https://github.com/syskom/ci-cd/issues

[license-shield]: https://img.shields.io/github/license/syskom/ci-cd.svg

[license-url]: https://github.com/syskom/ci-cd/blob/main/LICENSE

[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?logo=linkedin&colorB=555

[linkedin-url]: https://linkedin.com/in/marcin-k-dabrowski

[reusable-workflows-url]: https://docs.github.com/en/actions/using-workflows/reusing-workflows

[stars-shield]: https://img.shields.io/github/stars/syskom/ci-cd.svg

[stars-url]: https://github.com/syskom/ci-cd/stargazers

[syskom-org-url]: https://github.com/syskom
