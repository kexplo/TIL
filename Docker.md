## Create sudoer user in docker

```Dockerfile
RUN apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*
RUN useradd -m user && \
    echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && \
    chmod 0400 /etc/sudoers.d/gitlab
USER user
```

## Docker save, load

Documents:

- https://docs.docker.com/engine/reference/commandline/save/
- https://docs.docker.com/engine/reference/commandline/load/

> Save one or more images to a tar archive (streamed to STDOUT by default)

```bash
$ docker save -o etcd_v3.2.9.tar gcr.io/etcd-development/etcd:v3.2.9
```

```bash
$ docker load -i etcd_v3.2.9.tar
```

It is very useful when `docker pull` command hangs downloading image

## Multi-stage build

```Dockerfile
# base image
FROM microsoft/dotnet:2.1-runtime AS base
WORKDIR /app
EXPOSE 80

# build image
FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY . .
# `dotnet publish` command will run `dotnet restore` and `dotnet build` implicitly
RUN dotnet publish -c Release -o /app

# Add the binary files that generated in the `build image` container above
FROM base as final
WORKDIR /app
COPY --from=build /app .
RUN ls -al
ENTRYPOINT ["dotnet", "OrleansSilo.dll"]
```

## Set build-time variables (--build-arg)

Dockerfile:

```Dockerfile
FROM ubuntu
ARG MESSAGE
RUN echo $MESSAGE
```

Command:

```bash
$ docker build --build-arg MESSAGE=hello .
```

### Use ARG with ENV

```text
ARG <name>[=<default value>]
```

> You can use an `ARG` or an `ENV` instruction to specify variables that are available to the `RUN` instruction. Environment variables defined using the `ENV` instruction always override an `ARG` instruction of the same name. Consider this Dockerfile with an `ENV` and `ARG` instruction.

Dockerfile:

```Dockerfile
FROM ubuntu
ARG CONT_IMG_VER
ENV CONT_IMG_VER v1.0.0
RUN echo $CONT_IMG_VER
```

Command:

```bash
$ docker build --build-arg CONT_IMG_VER=v2.0.1 .
```

> In this case, the `RUN` instruction uses `v1.0.0` instead of the `ARG` setting passed by the user: `v2.0.1`

references:

- https://docs.docker.com/engine/reference/commandline/build/#set-build-time-variables---build-arg
- https://docs.docker.com/engine/reference/builder/#arg