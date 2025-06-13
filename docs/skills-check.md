---
layout: default
title: 4 - Skills Check
nav_order: 5
last_modified_date: "2025-06-11 02:13AM"
---

# Skills Check
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .no_toc .text-delta }
- TOC
{:toc}
</details>

Test your grasp of containers with the following checks. Which of these can you do?

## Novice

1. Install Docker Desktop on your local computer and sign into Docker.
2. Know the basic docker commands to pull a container image, list images, and delete an image.
3. Understand the two modes for running a container.
4. Be familiar with how to list running containers.

## Advanced Beginner

1. Know how to pass an environment variable into a container when issuing the `run` command.
2. Understand how to stop a running container and start it back again.
3. Be familiar with container registries, image tagging, and how to push/pull images to a registry.
4. Use `docker exec -it` to gain the terminal of a running container, and understand how to troubleshoot
errors or issues from within the container.

## Competent

1. Be familiar with port mapping between a running container and its local host machine.
2. Be familiar with volume mapping between a running container and its local host machine.
3. Write a `Dockerfile` and build your own code into a container image.

## Proficient

1. Understand ways to build small, or "slim," container images.
2. Know how to follow the output log of a running container.

## Expert

1. Be familiar with `docker compose` and how to launch multiple containers as part of a solution.
2. Know how to communicate across containers. For instance, if an API container needs to connect
to a containerized database service, what address does it use?
3. Set up automated container builds as part of a GitHub Action.
