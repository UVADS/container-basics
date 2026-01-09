---
layout: default
title: 2 - Advanced Usage
nav_order: 4
toc: true
last_modified_date: "2026-01-09 02:13AM"
---

# Advanced Usage
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .no_toc .text-delta }
- TOC
{:toc}
</details>

## Advanced `run` Options

Three important options can extend a container's functionality at runtime:

1. Injecting Environment Variables
2. Exposing Ports
3. Mounting Persistent Storage

### 1. Environment Variables

To inject an environment variable into a container when you are running it, use the `-e` flag with a key-value pair:

```bash
docker run -it -e MY_KEY="a1b2c3d4e5f6" ubuntu /bin/bash
```

Applications within this container could then call `$MY_KEY` from the environment.

You can assign multiple `env` variables in this way. This is particularly useful for injecting sensitive data into the environment, such as API keys, tokens, passwords, etc.

It can also be used to provide a database connection to a container, or indicate a particular mode for your application to run under, such as `dev` vs. `prod`, or to limit output during testing, etc.

### 2. Exposing Ports

There are many cases in which an application running on one container needs to connect to another container to make a request. This might be a database connection, an HTTP request to an API, etc.

To expose the port of a service running in a container, you should provide a port mapping with the `-p` flag at runtime. The value of this flag should be a `PORT:PORT` mapping with the host machine on the left and the container port on the right of the `:`.

Here is an example, exposing port `80` of the container to port `8181` of the host machine:

```bash
docker run -d -p 8181:80 nginx
```

You can also expose multiple ports at the same time, or even ranges of ports:

```bash
docker run -d -p 8050-8090:8050-8090 my_application
```

### 3. Persistent Storage Volumes

One troubling fact developers encounter when working with containers is that they do not persist. That is, when you run a container and it logs its work or handles user requests, none of that data is stored beyond the lifetime of the container. Once the container dies, that data is generally lost.

The solution to this problem, by design, is to allow for persistent storage from the host machine to be mapped into the container when it is run. In this way, both the host machine AND the running container share a storage path at the same time. If the container is stopped a new container can be restarted with the same mapping and all data is present.

To map a persistent storage volume to a container at runtime, use the `-v` flag with a `PATH:PATH` mapping with the host machine path on the left and the container path on the right of the `:`. An example:

```bash
docker run -d -v /home/user/proj1/data:/app/data my_app
```

## Finding Images

Container images are widely available, published alongside many code repositories in GitHub and elsewhere. The most common marketplace for common images is [**Docker Hub**](https://hub.docker.com/) where many common images are hosted, as well as a wide selection of [base images](#base-image).

## `docker build`

To create your own container image you have two options:

1. Run a container interactively, customize it, commit the container as an image, then push it to a registry. This is considered the "manual" approach, and is not very efficient since software versions will slowly drift and your container will soon contain outdated code.
2. Build the container using a code-driven method. This is the preferred method, and involves writing a special file called a `Dockerfile`. 

Below we will write a very simple application and containerize it using `docker build`.

`script.sh`

{% gist 619ebe4865e61accedef6de03fc6f072 %}

This is an exceedingly simple script that asks the user for two input variables and then adds them for an output. It will do for our purposes here.

So we have the software that will run within this container. The only prerequisite software it needs is `bash`. In some cases your code might need Python, R, or other language support, as well as any libraries or plugins needed by the software.

To provide `bash` or another basic shell for this script, we could use a base Ubuntu container image to start with, or a much smaller Alpine Linux distribution. Either would work fine, though the interpreter and its path (on line 1 of the script above) would need to match.

In the case of Ubuntu, you will need to write a file named `Dockerfile` in the same directory as the `script.sh` file:

`Dockerfile`

```bash
FROM ubuntu:latest
COPY script.sh script.sh
```

This is actually enough to build a container. Line `1` is the required `FROM` command, which tells Docker what base image to start with, and Line `2` copies the script into the image.

Next, build it with a simple tag, such as `foo`:

```bash
docker build -t foo .
```

Two elements in the command above are: an (optional) tag giving the image a convenient name, and the `.` (dot) indicating where to find the `Dockerfile`.

After building, run `docker images` and you will see an image named `foo`.

Run the image and trigger the script manually:

```bash
docker run -it foo bash script.sh
```

You should be prompted to input two numbers, and then the script gives the expected output. **At that point the container dies, i.e. stops running** since its "process" has completed in the script.

To improve upon this example, what if we wanted the container to run the script automatically without specifying a script path? And what if we did not want the script to be interactive, but input variables at runtime? Let's rewrite the script a bit:

`script.sh` - takes positional arguments when running the script.

{% gist aaf9281e7238b557ce5fc27a86ca60cd %}

Then build it:

```bash
docker build -t foo:2 .
```

And run it:

```bash
docker run foo:2 8 4
```

Since it is possible to pass positional arguments or environment variables to a container for a specific job, the smart developer will write their code flexibly enough to accept these values at runtime, and to read inputs accordingly.

For instance, imagine a genomic statistical script that needs to count or analyze a BED or BAM file passed to it. An input variable could be the path to the file itself, or even a URL. An output variable could specify where to write or send the output findings after completion. This might be a text file, a log file, a new row or document within a database, or a message in a Kafka topic.

<a id="base-image">&nbsp;</a>

{: .success :}
**What is a base image?**
In many cases, a containerized application might have a number of basic requirements to be installed alongside the application code. A language, some plugins, some configuration and settings files, an updated Operating System, etc. 
<br /><br />
Instead of building all of that into a container image every single time it is built, many developers use pre-built base images, and then write their newest code into in a final/second build. This speeds up the build process greatly.

## Platform Architecture & Multi-Arch Builds

Docker supports building images for multiple processor architectures. The two most common are `amd64` and `arm64`.

**amd64** (also called x86-64 or AMD64) is the 64-bit x86 architecture used by most desktop and laptop computers 
with Intel or AMD processors, as well as traditional cloud servers like standard AWS EC2, Google Cloud, and Azure instances.

**arm64** (also called AArch64) is the 64-bit ARM architecture known for power efficiency. You'll find it in Apple 
Silicon Macs (M1/M2/M3/M4), AWS Graviton instances, Raspberry Pi devices, and newer cloud instances optimized for cost and efficiency.

Why Multi-Arch Matters

If you build a Docker image only for amd64, it won't run natively on ARM-based systems and vice versa. While Docker 
Desktop can emulate different architectures, emulation is significantly slower than native execution. Building 
multi-arch images ensures your containers run efficiently regardless of the underlying hardware, which matters 
when developing on Apple Silicon Macs but deploying to traditional amd64-based AWS instances or experimenting 
with ARM-based Graviton instances.

## Automated Container Builds using GitHub Actions

Since container builds are code-driven, automated build processes such as GitHub Actions or Jenkins are incredibly useful to automate container image builds after a change has been committed to a repository.

## Cross-container Communication

Another feature built into Docker is a separate networking layer, a bridge or overlay network, that allows containers to communicate with one another by default.

The easiest way to establish communications is to use the `--name` flag when running containers and to use those names as virtual endpoints in their communication.

For example, a MySQL container can be run:

```bash
docker run --name mysql1 -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
```

And a client container can then point to that database service:

```bash
docker run -it --rm mysql mysql -hmysql1 -uroot -p
```

## `docker compose`

Docker Compose is an advanced method for running a "stack" of related containers together in a single application. The stack can be brought "up" or "down" and can communicate freely with one another by container name. 

This makes deployment and management easier:

1. All resources in the stack are brought up or down as a single unit.
2. The stack is managed in code, which can be versioned or shared with others.
3. This makes both testing and deployment easier across a team.

Review the code for this stack, and notice the resources it manages:

- Two services, the `db` itself and a `phpmyadmin` interface.
- A new network for these services.
- A persistent storage volume for data.

 {% gist 32b37537d505c9cef245d3cec62fe6a7 %}

 To run this stack locally, first create the persistent storage directory `db_data` and open its permissions:

 ```bash
 mkdir db_data && chmod 777 db_data
 ```

 Then run the stack using `docker compose`:

 ```bash
 docker compose up -d
 ```

 Notice the `-d` flag to run in "detached" mode. This launches the stack into a separate thread. To watch the logs of either container, find its ID and use `docker logs --follow <ID>`.

 Alternatively, omit the `-d` flag and you will see the log output in realtime.
