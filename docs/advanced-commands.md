---
layout: default
title: 2 - Advanced Usage ❌
nav_order: 4
toc: true
last_modified_date: "2025-06-12 02:13AM"
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

## Finding Images

Container images are widely available, published alongside many code repositories in GitHub and elsewhere. The most common marketplace for common images is [**Docker Hub**](https://hub.docker.com/) where many common images are hosted, as well as a wide selection of [base images]().

## `docker build`

To create your own container image you have two options:

1. Run a container interactively, customize it, commit the container as an image, then push it to a registry. This is considered the "manual" approach, and is not very efficient since software versions will slowly drift and your container will soon contain outdated code.
2. Build the container using a code-driven method. This is the preferred method, and involves writing a special file called a `Dockerfile`. 

Below we will write a very simple application and containerize it using `docker build`.

`script.sh`

```bash
#!/bin/bash

clear
echo "I am the Great Adder! I will add any two numbers you give me.";
echo "";
read -p "What is your first number " val1;
read -p "What is your second number " val2;
sum=$((val1+val2));
echo "I am thinking . . . ";
sleep 5;
echo "";
echo "Your sum is $sum!!!"
```

This is an exceedingly simple script that asks the user for two input variables and then adds them for an output. It will do for our purposes here.

So we have the software that will run within this container. The only prerequisite software it needs is `bash`. In some cases your code might need Python, R, or other language support, as well as any libraries or plugins needed by the software.

To provide `bash` or another basic shell for this script, we could use a base Ubuntu container image to start with, or a much smaller Alpine Linux distribution. Either would work fine, though the interpreter and its path (on line 1 of the script above) would need to match.

In the case of Ubuntu, you will need to write a file named `Dockerfile` in the same directory as the `script.sh` file:

```Dockerfile```

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

`script.sh` - takes positional arguments when running the script

```bash
#!/bin/bash

clear
echo "I am the Great Adder! I will add the two numbers you gave me.";
echo "";
sum=$(($1+$2));
echo "I am thinking . . . ";
sleep 2;
echo "";
echo "Your sum is $sum!!!"
```

`Dockerfile`

```bash
FROM ubuntu:latest
COPY script.sh script.sh
CMD chmod 755 script.sh
ENTRYPOINT ["bash", "script.sh"]
```

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

{: .success :}
**What is a base image?**<a name="base-image">&nbsp;</a>
In many cases, a containerized application might have a number of basic requirements to be installed alongside the application code. A language, some plugins, some configuration and settings files, an updated Operating System, etc. 
<br /><br />
Instead of building all of that into a container image every single time it is built, many developers use pre-built base images, and then write their newest code into in a final/second build. This speeds up the build process greatly.

## Automated Container Builds using GitHub Actions

Since container builds are code-driven, automated build processes such as GitHub Actions or Jenkins are incredibly useful to automate container image builds after a change has been committed to a repository.

## `docker compose`

Docker Compose is an advanced method for running a "stack" of related containers together in a single application. The stack can be brought "up" or "down" and can communicate freely with one another by container name.

 {% gist 542ee380dd0cf5a55a1cb19820c878e6 %}