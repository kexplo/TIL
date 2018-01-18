## Create sudoer user in docker

```Dockerfile
RUN apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*
RUN useradd -m user && \
    echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && \
    chmod 0400 /etc/sudoers.d/gitlab
USER user
```