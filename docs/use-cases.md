---
layout: default
title: 4 - Use Cases
nav_exclude: false
nav_order: 5
last_modified_date: "2025-06-10 02:13AM"
---

# Use Cases
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .no_toc .text-delta }
- TOC
{:toc}
</details>

## Devcontainers

xxxxx

## Run a Script

One of the simplest use cases for a container is to run some process, or chain of updates or transformations against a data file.

In this example 

## Run a Service

Since containers are designed to perform a single operation well, it may be useful to run a service for lookups, references,
or local testing.

Here we run a unique ID generator API on `localhost` port 8080 in detached mode. Once running the API is available at [http://127.0.0.1:8080/](http://127.0.0.1:8080/)

To fetch the API documentation, append `/docs` to the URL.

```
docker run -d -p 8080:80 ghcr.io/uvarc/id-generator:1.28
```

Access the API using a browser or the command-line, and pass the JSON through a filter like `jq`:

```
$ curl -s http://127.0.0.1:8080/keys | jq -r

{
  "access_key": "UVJVZIMJAWPI",
  "secret_key": "Hsibi1dR6As6RLNKqILIAaWPKG0T72Ra8ZNq"
}
```

```
$ curl -s http://127.0.0.1:8080/id/14 | jq -r

{
  "id": "futcxiq2fllef6"
}
```

## Pipeline Tasks

A variety of pipeline/workflow engines allow for the use of containers to peform individual tasks within a pipeline. For example,
Apache Airflow, Dagster, and Prefect are all container-enabled. In most scenarios, the output of a previous task serve as input
parameters for a containerized task (i.e. file location, task parameters, task options, output type).


## Run a Database

Containers provide an ideal local database when testing code or a larger application. The local database can be seeded with test or
example data to simulate the "real" production environment with live data.

Most databases can be launched locally with containers, including MySQL, PostgreSQL, MongoDB, Redis, and others.

Here we launch a single-node Redis in-memory datastore, available from the host machine over port `6379`.

```
docker run -d --name redis -p 6379:6379 redis:latest
```

To connect using the Redis CLI:

```
redis-cli -h 127.0.0.1 -p 6379
```

The datastore is available as if it were a native service on the host machine. Keep in mind this deployment does not have persistent
storage or any security associated with it. Restart the container and all data will be lost.


## Launch a Service and Interface

Docker is also built to orchestrate the creation and management of multiple resources simultaneously, in a "stack". This way, the
entire stack is defined in a single piece of code, and can be started or stopped together as a whole.

In this use case, `docker compose` is used to launch a group of related resources to support a three-node clustser of Apache Kafka
brokers, a Redpanda GUI to interact with the cluster, a private network across all containers in the solution, and persistent storage
for each of the brokers.

`docker-compose.yaml`

```
networks:
  redpanda_network:
    driver: bridge
volumes:
  redpanda-0: null
  redpanda-1: null
  redpanda-2: null
services:
  redpanda-0:
    command:
      - redpanda
      - start
      - --kafka-addr internal://0.0.0.0:9092,external://0.0.0.0:19092
      # Address the broker advertises to clients that connect to the Kafka API.
      # Use the internal addresses to connect to the Redpanda brokers'
      # from inside the same Docker network.
      # Use the external addresses to connect to the Redpanda brokers'
      # from outside the Docker network.
      - --advertise-kafka-addr internal://redpanda-0:9092,external://localhost:19092
      - --pandaproxy-addr internal://0.0.0.0:8082,external://0.0.0.0:18082
      # Address the broker advertises to clients that connect to the HTTP Proxy.
      - --advertise-pandaproxy-addr internal://redpanda-0:8082,external://localhost:18082
      - --schema-registry-addr internal://0.0.0.0:8081,external://0.0.0.0:18081
      # Redpanda brokers use the RPC API to communicate with each other internally.
      - --rpc-addr redpanda-0:33145
      - --advertise-rpc-addr redpanda-0:33145
      # Mode dev-container uses well-known configuration properties for development in containers.
      - --mode dev-container
      # Tells Seastar (the framework Redpanda uses under the hood) to use 1 core on the system.
      - --smp 1
      - --default-log-level=info
    image: docker.redpanda.com/redpandadata/redpanda:v25.1.3
    container_name: redpanda-0
    volumes:
      - redpanda-0:/var/lib/redpanda/data
    networks:
      - redpanda_network
    ports:
      - 18081:18081
      - 18082:18082
      - 19092:19092
      - 19644:9644
  redpanda-1:
    command:
      - redpanda
      - start
      - --kafka-addr internal://0.0.0.0:9092,external://0.0.0.0:29092
      - --advertise-kafka-addr internal://redpanda-1:9092,external://localhost:29092
      - --pandaproxy-addr internal://0.0.0.0:8082,external://0.0.0.0:28082
      - --advertise-pandaproxy-addr internal://redpanda-1:8082,external://localhost:28082
      - --schema-registry-addr internal://0.0.0.0:8081,external://0.0.0.0:28081
      - --rpc-addr redpanda-1:33145
      - --advertise-rpc-addr redpanda-1:33145
      - --mode dev-container
      - --smp 1
      - --default-log-level=info
      - --seeds redpanda-0:33145
    image: docker.redpanda.com/redpandadata/redpanda:v25.1.3
    container_name: redpanda-1
    volumes:
      - redpanda-1:/var/lib/redpanda/data
    networks:
      - redpanda_network
    ports:
      - 28081:28081
      - 28082:28082
      - 29092:29092
      - 29644:9644
    depends_on:
      - redpanda-0
  redpanda-2:
    command:
      - redpanda
      - start
      - --kafka-addr internal://0.0.0.0:9092,external://0.0.0.0:39092
      - --advertise-kafka-addr internal://redpanda-2:9092,external://localhost:39092
      - --pandaproxy-addr internal://0.0.0.0:8082,external://0.0.0.0:38082
      - --advertise-pandaproxy-addr internal://redpanda-2:8082,external://localhost:38082
      - --schema-registry-addr internal://0.0.0.0:8081,external://0.0.0.0:38081
      - --rpc-addr redpanda-2:33145
      - --advertise-rpc-addr redpanda-2:33145
      - --mode dev-container
      - --smp 1
      - --default-log-level=info
      - --seeds redpanda-0:33145
    image: docker.redpanda.com/redpandadata/redpanda:v25.1.3
    container_name: redpanda-2
    volumes:
      - redpanda-2:/var/lib/redpanda/data
    networks:
      - redpanda_network
    ports:
      - 38081:38081
      - 38082:38082
      - 39092:39092
      - 39644:9644
    depends_on:
      - redpanda-0
  console:
    container_name: redpanda-console
    image: docker.redpanda.com/redpandadata/console:v3.1.0
    networks:
      - redpanda_network
    entrypoint: /bin/sh
    command: -c 'echo "$$CONSOLE_CONFIG_FILE" > /tmp/config.yml; /app/console'
    environment:
      CONFIG_FILEPATH: /tmp/config.yml
      CONSOLE_CONFIG_FILE: |
        kafka:
          brokers: ["redpanda-0:9092"]
        schemaRegistry:
          enabled: true
          urls: ["http://redpanda-0:8081"]
        redpanda:
          adminApi:
            enabled: true
            urls: ["http://redpanda-0:9644"]
    ports:
      - 8080:8080
    depends_on:
      - redpanda-0
```

To run the stack, `cd` into the directory containing the `docker-compose.yaml` file and issue this command:

```
docker compose up -d
```

You will see that a network and four (4) containers are spawned, and you can inspect each one using regular `docker` commands.

- Open a browser to [http://127.0.0.1:8080/](http://127.0.0.1:8080/) to see the GUI.
- Point any Kafka producers or consumers to `internal://0.0.0.0:9092`

To bring down the stack:

```
docker compose down
```

{: .note }
**Learn More about Docker Compose** at [https://docs.docker.com/compose/](https://docs.docker.com/compose/)

## Watch how to set up SSH Keys

[![Using SSH keys with GitHub](https://i.ytimg.com/vi/rajlGZ3w4OU/maxresdefault.jpg)](https://www.youtube.com/watch?v=rajlGZ3w4OU)
