---
layout: default
title: Home
nav_order: 1
description: "Working with Containers in Data Science."
permalink: /
last_modified_date: "2025-06-11 02:55PM"
---

# Containers in Data Science
{: .fs-9 }

Container Basics: How to set up, configure, and work with containerized applications.
{: .fs-6 .fw-300 }

[Setting Up](docs/setup/){: .btn .btn-outline .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Basic](docs/basic-commands/){: .btn .btn-outline .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Advanced](docs/advanced-commands/){: .btn .btn-outline .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Workloads](docs/workloads/){: .btn .btn-outline .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Use Cases](docs/use-cases/){: .btn .btn-outline .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Check](docs/skills-check/){: .btn .btn-outline .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

<img src="https://uvads.github.io/container-basics/assets/images/docker-container.png" style="float:right;max-width:40%;" alt="Docker Container image" />

## What is a Container?

A container is a standardized, self-contained environment that packages an application and all its dependencies, allowing it to run reliably on different systems. It's a lightweight, portable unit of software that's easy to deploy and manage. 

## What is Docker?

> **Docker** is a company that made the creation and use of containers easy and manageable for developers. While they did not invent the notion of the container, they popularized it with the launch of the Docker engine in 2013. Docker is a for-profit PaaS company that significantly helped standardize the container runtime.

Most companies today use containers to some degree. **Google**, for example, runs all of their web platform within
containers (which run on a global container orchestration engine they wrote named Kubernetes), as well as Instagram,
Uber, Netflix, Spotify, PayPal, ING, and Coca-Cola. Containers are an important part of a flexible, scalable platform, but their usefulness easily transfers to finance, backend systems, and data science.

## Containers in Data Science

Containers are incredibly useful in data science because they package an application and all its dependencies (libraries, code, runtime, system tools, etc.) into a single, isolated unit. This solves the perennial "it works on my machine" problem by ensuring that a data scientist's code, models, and analytical environments run consistently across different computing environments – from a local laptop to a cloud server. This reproducibility and portability streamline collaboration, simplify deployment of models into production, and make it easier to share research, as anyone can run the exact same environment with guaranteed results, regardless of their underlying system configuration.
