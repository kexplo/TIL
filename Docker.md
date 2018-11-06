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