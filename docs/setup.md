---
layout: default
title: 0 - Install & Setup
nav_order: 2
last_modified_date: "2025-06-12 06:40PM"
---

# Install and Set Up Docker
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .no_toc .text-delta }
- TOC
{:toc}
</details>

## Installation

[Docker Desktop]([https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

Docker Desktop is the easiest and most popular way to run Docker on your local workstation. However, there are a number of other solutions that work in nearly-identical ways:

- Rancher
- Podman
- Colima
- `containerd`

## Configuration & Signing In

Most default settings are fine for Docker Desktop. In some cases you might want to allocate more/less CPU or memory to running containers. You can also specify how much disk space can be used for running containers and their storage.

You will also be asked to authenticate Docker Desktop with a free Docker account (enroll [here](https://app.docker.com/signup/)).

If you intend on pushing images to a container registry other than Docker Hub, you will need to sign into that registry as well from the command-line.

For example, if you want to work with GHCR, the GitHub Container Registry. Be aware that with GHCR you cannot authenticate with a password, but with a PAT (Personal Access Token) instead:

```bash
docker login ghcr.io
Username: ******
Password: ******
```

## How to create a Personal Access Token (PAT) in GitHub

1. Go to https://github.com/settings/tokens
2. Generate a new PAT and grant it necessary permissions.
3. Expiration can be specified for a length of time. You will be notified when a PAT is about to expire.
4. Be sure to copy down the PAT once it is displayed on the screen; it will never be shown to you again.
