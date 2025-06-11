---
layout: default
title: 1 - Basic Commands
nav_order: 3
toc: true
last_modified_date: "2025-06-11 02:13AM"
---

# Basic Commands
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .no_toc .text-delta }
- TOC
{:toc}
</details>

Below are some of the most common, foundational commands for working with containers. These assume you have Docker installed on your local workstation. However, Podman and other tools provide identical functionality.

1. `docker pull`

    This command pulls a pre-made image from a container registry, such as Docker Hub or GHCR, etc. However, you are not required to pull an image before using it; `docker run` will pull any necessary images.

    Example:
    ```bash
    docker pull pytorch/pytorch
    ```

    ```bash
    docker pull ghcr.io/uvarc/id-generator:latest
    ```

2. `docker images`

    List all container images pulled to the local environment with this command. The output lists containers, in order by most-recent pull. Each container has an a name, a tag, an image ID, a created date, and its size.

    ```bash
    $ docker images

    REPOSITORY                   TAG       IMAGE ID       CREATED        SIZE
    site                         latest    b46e2764b4c9   5 hours ago    1.59GB
    ghcr.io/uvarc/id-generator   1.28      59baec9d466f   8 months ago   1.55GB
    pytorch/pytorch              latest    11691e035a36   5 months ago   11.70GB
    ```

    To remove an image, use the `docker rmi` command. This can also be forced with the `--force/-f` flag:

    ```bash
    docker rmi --force 11691e035a36
    ```

    {: .success :}
    **About Image Names:Tags**
    Container images can be assigned names and tags when built or at any point after they exist. Container **names** can be arbitrary if you are building and running the images locally. Or, if the aim is to build a container and push it to a registry for use elsewhere, you must follow that registry's naming conventions. 
    <br /><br />
    In the case of Docker Hub, the name should be `<account>/<image-name>:<tag>`. For other registries, prepend with the domain to the registry; for GHCR, an example might be `ghcr.io/<account-or-org>/<image-name>:<tag>`
    <br /><br />
    **Tags** If no tag is assigned to an image you build, the assumed tag is called `latest`. However, the developer can easily manage tagging manually or using automation. Some useful tags might be a version number `1.3`, or a `dev` tag. 

3. `docker tag`

    Find the `IMAGE ID` of a container image and you can assign it other tags. Below we pull the `nginx` container, find its ID, and assign it an additional name and tag:

    ```bash
    docker pull nginx
    ```

    Find the ID:

    ```bash
    docker images

    REPOSITORY                   TAG       IMAGE ID       CREATED        SIZE
    nginx                        latest    6784fb0834aa   8 weeks ago    281MB
    ```

    Now we `name:tag` as needed:
    ```bash
    docker tag 6784f ghcr.io/uvads/nginx:special

    docker images

    REPOSITORY                   TAG       IMAGE ID       CREATED        SIZE
    ghcr.io/uvads/nginx          special   6784fb0834aa   8 weeks ago    281MB
    nginx                        latest    6784fb0834aa   8 weeks ago    281MB
    ```
    Notice that a new image appears in the listing, but has the same ID, same created date, and same size. Think of naming and tagging as aliases to a container ID.

    {: .success :}
    **Image and Container IDs** Notice that in all examples in this site, most all commands do not require the full ID in order to identify an image or running container. Generally the first 4-5 characters will suffice to uniquely identify your target.

1. `docker run`
2. `docker ps`
3. `docker exec`
4. `docker logs`
5. `docker stop`
6.  `docker rm`


{: .warning :}
It is important to remember that when **cloning** or **forking** an existing repository.


