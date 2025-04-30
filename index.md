---
layout: default
title: Home
nav_order: 1
description: "Working with Containers in Data Science."
permalink: /
last_modified_date: "2025-04-24 02:13AM"
---

# Containers in Data Science
{: .fs-9 }

Container Basics: How to set up, configure, and work with containerized applications.
{: .fs-6 .fw-300 }

[Setting Up](docs/setup/){: .btn .btn-green .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Basic Commands](docs/basics/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Advanced](docs/advanced/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Use Cases](docs/advanced/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Check](docs/skills-check/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

<img src="https://uvads.github.io/container-basics/assets/images/docker-container.png" style="float:right;max-width:40%;" alt="Docker Container image" />

## What is a Container?

Source control, also known as "version control," is the process of tracking changes and versions of electronic files over time. This might include code, data, images, and other files. Good source control management tools allow users to see the entire history of changes to any specific file, or the evolution of an entire project. `git` is currently the most popular and feature-rich source control tool.

## What is Docker?

> Git is a distributed version control system that tracks changes in any set of computer files, usually used for coordinating work among programmers who are collaboratively developing source code during software development. Its goals include speed, data integrity, and support for distributed, non-linear workflows. <sup>[Wikipedia](https://en.wikipedia.org/wiki/Git)</sup>

## Containers in Data Science

Data aggregation, cleaning, pipelines and ML models all rely on software in order to operate. Responsible software management depends on well-managed code, versioning, prioritizing bugs, features, and user issues. Working at scale, modern platforms and infrastructure tend to require code-driven tests, builds, deployments, and management. Code can be used to define all the layers of effort across teams of engineers and data scientists.

Which is to say: Code is fundamental to our work, and it would be risky, inefficient, and impractical not to use source control.

<iframe width="720" height="405" src="https://www.youtube.com/embed/3N3n9FzebAA?si=UH6ibNNWgXfn25Qe" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


## Contents

- [**Setup**](docs/setup/)
  - Install and set up Docker
  - Authenticate `git` to GitHub
  - Basic configuration
  - Troubleshooting authentication
- [**Creating and managing a repository**](docs/creating-repositories/)
  - Create a repository locally
  - Create a repository in GitHub
  - Add or remove collaborators
- [**Source control basics**](docs/basics/)
  - Diff
  - Status
  - Add
- [**Branches, Forks, and Merges**](docs/forks-branches/)
  - Branches
  - Forks
  - Fetch from Upstream
- [**Advanced Git/GitHub Features**](docs/advanced/)
  - Stash
  - Signing commits
  - Reset and Revert
- [**Skills Check**](docs/skills-check/)
