# Table of Contents
- [Background](#background)
- [Docker](#docker)
- [Docker images](#images)
- [Docker containers](#container)
- [Container inspection](#inspect)
- [Copy file to/from from container](#copy_file)
- [Copy folder to/from from container](#copy_folder)
- [Image backup](#image_backup)
- [Registry -- push](#registry_push)
- [Extract container info](#extract_container_info)
- [Errors](#errors)

<a name="background"></a>
## Background

- Container: A self contained blackbox which contains all of the software configurations required to run an application. It can be a lightweight or heavyweight box depending upon the end app's requirement. A underlying OS can host multiple containers and each of these container can have totally different software configurations inside them.
- Docker: A company providing solution for containers.
- Docker image: It contains an OS + extra packages + applications.
- Docker container: A running instance of the docker image. Multiple contains can use the same docker image.
- Docker is not end of world, there are alot of others players (LXD, OpenVZ, OpenVZ, Linux VServer) are too in the market.
- Union file systems implement a union mount and operate by creating layers. Docker uses union file systems in conjunction with copy-on-write techniques to provide the building blocks for containers, making them very lightweight and fast.

<a name="docker"></a>
## Docker

- `docker --version` - print docker version
- `docker version` - print docker version
- `docker info` OR `docker system info` - print detailed info related to docker installation
- `docker info --format "{{json .}}" | jq -C .` - print detailed info related to docker installation in JSON format
- Following commands can be used to extract a specific JSON node from docker info
```
docker info --format "{{json .Plugins}}" | jq -C .
docker info --format "{{json .RegistryConfig}}" | jq -C .
docker info --format "{{json .MemTotal}}" | jq -C .
```
- `docker run hello-world` - test docker installation
- Manipulating Docker daemon on Ubuntu
   `sudo service docker status`
   `sudo service docker restart`

<a name="images"></a>
## Docker images

- `docker build --tag image-name:tag .` - Build a docker image
- `docker image ls` - List Docker images
- `docker image ls --digests` - List docker images with sha256 sum
- `docker images ls -f dangling=true` - Show dangling docker image
- `docker rmi <image-name_OR_image-id-number>` - Remove a specific docker image
- `docker images purge` - Remove unused docker images
- `docker rmi $(docker images --quiet)` - Remove all images
- `docker rmi $(docker images --quiet --filter "dangling=true")` - Remove dangling images
- `docker images | grep '^<none>' | awk '{print $3}'` - List image having <none> as their name

<a name="container"></a>
## Docker containers

- `docker container ls` - Show running containers
- `docker container ls --all` - List all container
- `docker container ls --all --quiet` - List only IDs of all containers
- `docker rm <docker-name_OR_docker-id>` - Remove a specific container
- `docker rm $(docker container ls --all --quiet)` - Remove all containers
- `docker rm $(docker container ls --quiet --filter "status=exited")` - Remove containers which are in exited state
- `docker commit container-name_OR_container-id` - Command to save modified contents back to image otherwise modifications will be lost on stopping container
- `docker create --interactive --tty --name container-name image-name /bin/bash` - Create a docker container from image; It's entry point point is set to bash shell
- `docker start --interactive --attach container-name` - Re-start container which previously exited and an open interactive shell
- `docker stop container-name` - Stop a docker
- `docker run <OPTIONS> --interactive --tty image-name /bin/bash` - Create a container from the specified image and open an interactive shell. `<OPTIONS>` are:
  - `--security-opt seccomp:unconfined` - container with security relaxation
  - `--volume /path/to/folder/on/host:/path/to/folder/in/container` - map host folder into container env
  - `--rm` - Remove container once interactive shell has been closed

- `docker run --rm -it -v ``pwd``:/mapped image-name:tag /bin/bash` - Create/run a container from the specified image and mapped current dir to `/mapped` inside the docker
- `docker run --interactive --detach --name container-name image-name` - Run docker container and de-attach mode
- `docker run -it --entrypoint /bin/bash <image>` - Simple docker run command does not work if specified image has a defined ENTRYPOINT.
- `docker tag <old-repo-name:old-tag-name> <new-repo-name:new-tag-name>` - Rename docker image
- `docker rename <old-name> <new-name>` - Rename docker container

- Attach multiple shell to a running container.
  - `docker exec --interactive --tty container-id bash`
  OR
  - `docker exec --interactive --tty container-id /bin/bash -c "export TERM=xterm; exec bash"`
  - Shell without tty width restriction.

:information_source:
Put following in Dockerfile to have shell without width restriction.
```
echo "export TERM=xterm" >> /etc/bash.bashrc
```

<a name="inspect"></a>
## Container inspection

- `docker inspect --format '{{ .Volumes }}' container-id`
- `docker inspect --format '{{ .NetworkSettings.IPAddress }}' container-id`
- `docker inspect --format '{{ .Mounts }}' container-id`
- `docker inspect --format '{{ range .NetworkSettings.Networks }} {{ .IPAddress }} {{ end }}' container-id`
- `docker inspect --format '{{ range .NetworkSettings.Networks }} {{ .IPAddress }} {{ end }}' container-id`
- `docker inspect --format '{{ json .Mounts }}' container-id | jq -C .`

<a name="copy_file"></a>
## Copy file to/from from container

```
docker cp foo.txt container-name:/foo.txt
docker cp container-name:/foo.txt foo.txt
```

<a name="copy_folder"></a>
## Copy folder to/from from container

```
docker cp folder container-name:/folder
docker cp container-name:/folder folder
```

<a name="image_backup"></a>
## Image backup

- `docker save image-name --output image-name-backup.tar` - Create backup of the docker image
- `docker load --input image-name-backup.tar` - Restore docker image backup
- `docker save image-name | gzip > image-name-backup.tar.gz` - Create backup of the docker image and compress it
- `gzcat image-name-backup.tar.gz | docker load` - Restore compressed docker image

<a name="registry_push"></a>
## Registry -- push

- To push image to docker registry, you need to crate a tag against that docker registry:
```
docker tag <image-name> <docker-registry-host>/<image-name>
docker push <docker-registry-host>/<image-name>
```

<a name="extract_container_info"></a>
## Extract container info

- Commands to extract Docker container hostname, ip and full Docker Id
https://gist.github.com/aakbar5/8ac6f4447dbb25c7e443dfa88aa9a69e


<a name="errors"></a>
## Errors

- Error # 1
```
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.26/containers/create: dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```
-- Solution
```
sudo groupadd docker
sudo usermod -a -G docker $USER
```
Logout and login again to use docker without `sudo`.
