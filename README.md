# docker-python-db2-cluster-demo

## Overview

This `senzing/python-db2-cluster-demo` docker image demonstrates how to write an app based on the
[senzing/python-db2-cluster-base](https://github.com/Senzing/docker-python-db2-cluster-base) docker image.

To see a demonstration of this python demo in action, see
[github.com/senzing/docker-compose-db2-cluster-demo](https://github.com/senzing/docker-compose-db2-cluster-demo).

### Contents

1. [Demonstrate](#demonstrate)
    1. [Build docker image](#build-docker-image)
    1. [Create SENZING_DIR](#create-senzing_dir)
    1. [Set environment variables for demonstration](#set-environment-variables-for-demonstration)
    1. [Run docker container](#run-docker-container)
1. [Develop](#develop)
    1. [Prerequisite software](#prerequisite-software)
    1. [Set environment variables for development](#set-environment-variables-for-development)
    1. [Clone repository](#clone-repository)
    1. [Build docker image](#build-docker-image)

## Demonstrate

### Build docker image

This Dockerfile uses `FROM senzing/python-db2-cluster-base`.
If the `senzing/python-db2-cluster-base` docker image not available, create it by following instructions at
[github.com/Senzing/docker-python-db2-cluster-base](https://github.com/Senzing/docker-python-db2-cluster-base#build-docker-image)

```console
docker build --tag senzing/python-db2-cluster-demo https://github.com/senzing/docker-python-db2-cluster-demo.git
```

### Create SENZING_DIR

If you do not already have an `/opt/senzing` directory on your local system, visit
[HOWTO - Create SENZING_DIR](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/create-senzing-dir.md).

### Set environment variables for demonstration

1. Identify the host and port running DB2 server.
   Example:

    ```console
    docker ps

    # Choose value from NAMES column of docker ps
    export DB2_HOST=docker-container-name
    ```

    ```console
    export DB2_PORT=50000
    ```

1. Identify the database username and password.
   Example:

    ```console
    export DB2_USERNAME=db2inst1
    export DB2_PASSWORD=db2inst1
    ```

1. Identify the database that is the target of the SQL statements.
   Example:

    ```console
    export DB2_DATABASE=G2
    ```

### Run docker container

1. Option #1 - Run the docker container without database or volumes.

    ```console
    docker run -it \
      senzing/python-db2-demo
    ```

1. Option #2 - Run the docker container with database and volumes.

    ```console
    docker run -it  \
      --volume ${SENZING_DIR}:/opt/senzing \
      --env SENZING_DATABASE_URL="db2://${DB2_USERNAME}:${DB2_PASSWORD}@${DB2_HOST}:${DB2_PORT}/${DB2_DATABASE}" \
      senzing/python-db2-demo
    ```

1. Option #3 - Run the docker container accessing a database in a docker network.

    Identify the Docker network of the DB2 database.
    Example:

    ```console
    docker network ls

    # Choose value from NAME column of docker network ls
    export DB2_NETWORK=nameofthe_network
    ```

    Run docker container.

    ```console
    docker run -it  \
      --volume ${SENZING_DIR}:/opt/senzing \
      --net ${DB2_NETWORK} \
      --publish 5000:5000 \
      --env SENZING_DATABASE_URL="db2://${DB2_USERNAME}:${DB2_PASSWORD}@${DB2_HOST}:${DB2_PORT}/${DB2_DATABASE}" \
      senzing/python-db2-demo
    ```

## Develop

### Prerequisite software

The following software programs need to be installed.

#### git

```console
git --version
```

#### make

```console
make --version
```

#### docker

```console
docker --version
docker run hello-world
```

### Set environment variables for development

1. These variables may be modified, but do not need to be modified.
   The variables are used throughout the installation procedure.

    ```console
    export GIT_ACCOUNT=senzing
    export GIT_REPOSITORY=docker-python-demo
    export DOCKER_IMAGE_TAG=senzing/python-db2-demo
    ```

1. Synthesize environment variables.

    ```console
    export GIT_ACCOUNT_DIR=~/${GIT_ACCOUNT}.git
    export GIT_REPOSITORY_DIR="${GIT_ACCOUNT_DIR}/${GIT_REPOSITORY}"
    export GIT_REPOSITORY_URL="git@github.com:${GIT_ACCOUNT}/${GIT_REPOSITORY}.git"
    ```

### Clone repository

1. Get repository.

    ```console
    mkdir --parents ${GIT_ACCOUNT_DIR}
    cd  ${GIT_ACCOUNT_DIR}
    git clone ${GIT_REPOSITORY_URL}
    ```

### Build docker image

1. Option #1 - Using make command

    ```console
    cd ${GIT_REPOSITORY_DIR}
    make docker-build
    ```

1. Option #2 - Using docker command

    ```console
    cd ${GIT_REPOSITORY_DIR}
    docker build --tag ${DOCKER_IMAGE_TAG} .
    ```
