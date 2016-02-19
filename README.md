## Quickstart

Clone this repo:

```bash
git clone git@github.com:OldCrowEW/docker-centos-bind.git
```

Build docker image:

```bash
docker build -t oldcrowew/docker-centos-bind .
```

or

Docker pull command:

```bash
docker pull oldcrowew/docker-centos-bind
```

Start BIND using:

```bash
docker run --name bind -d --restart=always \
  --publish 53:53/udp \
  --volume /srv/docker/bind:/data \
  oldcrowew/docker-centos-bind:latest
```

## Persistence

For the BIND to preserve its state across container shutdown and startup you should mount a volume at `/data`.

> *The [Quickstart](#quickstart) command already mounts a volume for persistence.*

SELinux users should update the security context of the host mountpoint so that it plays nicely with Docker:

```bash
mkdir -p /srv/docker/bind
chcon -Rt svirt_sandbox_file_t /srv/docker/bind
```

# Maintenance

## Upgrading

To upgrade to newer releases:

  1. Download the updated Docker image:

  ```bash
  docker pull oldcrowew/docker-centos-bind:latest
  ```

  2. Stop the currently running image:

  ```bash
  docker stop bind
  ```

  3. Remove the stopped container

  ```bash
  docker rm -v bind
  ```

  4. Start the updated image

  ```bash
  docker run -name bind -d \
    [OPTIONS] \
    oldcrowew/docker-centos-bind:latest
  ```

## Shell Access

For debugging and maintenance purposes you may want access the containers shell. You can access a running containers shell by starting `bash` using `docker exec`:

```bash
docker exec -it bind bash
```