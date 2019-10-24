# Run GitLab Runner in a Docker container

Register GitLab Runner to create the `/etc/gitlab-runner/config.toml`

```bash
$ docker run --rm -t -i -v /etc/gitlab-runner:/etc/gitlab-runner gitlab/gitlab-runner:latest register

Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
https://gitlab.com

Please enter the gitlab-ci token for this runner:
token

Please enter the gitlab-ci description for this runner:
[xxxxxxxxxxxx]: runner desc

Please enter the gitlab-ci tags for this runner (comma separated):

Registering runner... succeeded                     runner=XXXXXXXX

Please enter the executor: parallels, ssh, virtualbox, docker+machine, docker-ssh+machine, docker, docker-ssh, shell, kubernetes, custom:
docker

Please enter the default Docker image (e.g. ruby:2.6):
alpine:latest

Runner registered successfully.
```

Run a GitLab Runner container

```bash
$ docker run -d --name gitlab-runner --restart always \
  -v /etc/gitlab-runner:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

references:

- https://docs.gitlab.com/runner/install/docker.html
- https://docs.gitlab.com/runner/register/
