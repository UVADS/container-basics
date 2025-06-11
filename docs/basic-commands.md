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
    **About Image Tags**
    asdflkajs flwekj fsldkfj welkjfsd lfkj welkrjsd lfksdj flkwejr lksdjf lwkerj ldkfj sdlkfjw elrkjsd flksj ejrlwkej fsldkfj lwekj sdlkfjsd fl.


3. `docker run`
4. `docker ps`
5. `docker exec`
6. `docker logs`
7. `docker stop`
8. `docker rm`


{: .warning :}
It is important to remember that when **cloning** or **forking** an existing repository.


