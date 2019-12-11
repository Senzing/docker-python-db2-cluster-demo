# docker-python-db2-cluster-demo

## :warning: Obsolete

This repository has not been updated to use the RPM/DEB installation of Senzing.

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
    1. [Build docker image for development](#build-docker-image-for-development)

### Legend

1. :thinking: - A "thinker" icon means that a little extra thinking may be required.
   Perhaps you'll need to make some choices.
   Perhaps it's an optional step.
1. :pencil2: - A "pencil" icon means that the instructions may need modification before performing.
1. :warning: - A "warning" icon means that something tricky is happening, so pay attention.

## Expectations

### Space

This repository and demonstration require 6 GB free disk space.

### Time

Budget 40 minutes to get the demonstration up-and-running, depending on CPU and network speeds.

### Background knowledge

This repository assumes a working knowledge of:

1. [Docker](https://github.com/Senzing/knowledge-base/blob/master/WHATIS/docker.md)

## Demonstrate

### Build docker image

1. If `senzing/python-db2-cluster-base` image is not in local docker repository, it must be built manually.
   Follow the build instructions at
   [github.com/Senzing/docker-python-db2-cluster-base](https://github.com/Senzing/docker-python-db2-cluster-base#build)

1. Build image:

    ```console
    sudo docker build --tag senzing/python-db2-cluster-demo https://github.com/senzing/docker-python-db2-cluster-demo.git
    ```

### Create SENZING_DIR

Note: this is an obsolete method.

1. If you do not already have an `/opt/senzing` directory on your local system, visit
   [HOWTO - Create SENZING_DIR](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/create-senzing-dir.md).

### Set environment variables for demonstration

1. Identify the Senzing directory.
   Example:

    ```console
    export SENZING_DIR=/opt/senzing
    ```

1. Identify the database username and password for each database instance.
   Example:

    ```console
    export DB2_USERNAME_CORE=db2inst1
    export DB2_USERNAME_RES=db2inst1
    export DB2_USERNAME_LIBFE=db2inst1

    export DB2_PASSWORD_CORE=db2inst1
    export DB2_PASSWORD_RES=db2inst1
    export DB2_PASSWORD_LIBFE=db2inst1
    ```

1. Identify the database alias that is the target of the SQL statements.
   Example:

    ```console
    export DB2_DATABASE_ALIAS_CORE=G2_CORE
    export DB2_DATABASE_ALIAS_RES=G2_RES
    export DB2_DATABASE_ALIAS_LIBFE=G2_LIBFE
    ```

1. Identify the host and port running DB2 server.
   Example:

    ```console
    sudo docker ps

    # Choose value from NAMES column of docker ps
    export DB2_HOST_CORE=docker-container-name-1
    export DB2_HOST_RES=docker-container-name-2
    export DB2_HOST_LIBFE=docker-container-name-3
    ```

    ```console
    export DB2_PORT_CORE=50000
    export DB2_PORT_RES=50000
    export DB2_PORT_LIBFE=50000
    ```

### Docker network

:thinking: **Optional:**  Use if docker container is part of a docker network.

1. List docker networks.
   Example:

    ```console
    sudo docker network ls
    ```

1. :pencil2: Specify docker network.
   Choose value from NAME column of `docker network ls`.
   Example:

    ```console
    export SENZING_NETWORK=*nameofthe_network*
    ```

1. Construct parameter for `docker run`.
   Example:

    ```console
    export SENZING_NETWORK_PARAMETER="--net ${SENZING_NETWORK}"
    ```

### Run docker container

1. Run the docker container.

    ```console
    sudo docker run -it  \
      --env SENZING_CORE_DATABASE_URL="db2://${DB2_USERNAME_CORE}:${DB2_PASSWORD_CORE}@${DB2_HOST_CORE}:${DB2_PORT_CORE}/${DB2_DATABASE_ALIAS_CORE}" \
      --env SENZING_RES_DATABASE_URL="db2://${DB2_USERNAME_RES}:${DB2_PASSWORD_RES}@${DB2_HOST_RES}:${DB2_PORT_RES}/${DB2_DATABASE_ALIAS_RES}" \
      --env SENZING_LIBFE_DATABASE_URL="db2://${DB2_USERNAME_LIBFE}:${DB2_PASSWORD_LIBFE}@${DB2_HOST_LIBFE}:${DB2_PORT_LIBFE}/${DB2_DATABASE_ALIAS_LIBFE}" \
      --publish 5000:5000 \
      --volume ${SENZING_DIR}:/opt/senzing \
      ${SENZING_NETWORK_PARAMETER} \
      senzing/python-db2-cluster-demo
    ```

## Develop

### Prerequisite software

The following software programs need to be installed:

1. [git](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/install-git.md)
1. [make](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/install-make.md)
1. [docker](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/install-docker.md)

### Clone repository

For more information on environment variables,
see [Environment Variables](https://github.com/Senzing/knowledge-base/blob/master/lists/environment-variables.md).

1. Set these environment variable values:

    ```console
    export GIT_ACCOUNT=senzing
    export GIT_REPOSITORY=docker-python-db2-cluster-demo
    export GIT_ACCOUNT_DIR=~/${GIT_ACCOUNT}.git
    export GIT_REPOSITORY_DIR="${GIT_ACCOUNT_DIR}/${GIT_REPOSITORY}"
    ```

1. Follow steps in [clone-repository](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/clone-repository.md) to install the Git repository.

### Build docker image for development

1. **Option #1:** Using `docker` command and local repository.

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo docker build --tag senzing/python-db2-cluster-demo .
    ```

1. **Option #2:** Using `make` command.

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo make docker-build
    ```

    Note: `sudo make docker-build-development-cache` can be used to create cached docker layers.
