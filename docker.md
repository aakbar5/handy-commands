# Table of Contents
- [Background](#background)
- [Docker](#docker)
- [Docker images](#images)
- [Docker containers](#containers)
- [Create a docker image/container](#create_a_docker_img)
- [Other useful docker commands](#misc_cmds)
- [Copy file to/from from container](#copy_file)
- [Copy folder to/from from container](#copy_folder)
- [Container backup](#container_backup)
- [Registry -- push](#registry_push)
- [Workflow # 1](#workflow_1)
- [Workflow # 2](#workflow_2)
- [Workflow # 3](#workflow_3)
- [Workflow # 4](#workflow_4)
- [Untested commands](#untested_commands)
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
- `docker info` - print detailed info docker installation
- `docker run hello-world` - test docker installation

<a name="images"></a>
## Docker images
- `docker image ls` - List Docker images
- `docker image ls --digests` - List docker images with sha256 sum
- `docker rmi <image-name_OR_image-id-number>` - Remove a specific docker image
- `docker images -f dangling=true` - Show dangling docker image
- `docker images purge` - Remove dangling docker images

1 Docker images consist of multiple layers. Dangling images are layers that have no relationship to any tagged images. They no longer serve a purpose and consume disk space.

<a name="containers"></a>
## Docker containers
- `docker container ls` - Show running containers
- `docker container ls --all` - List all container
- `docker container ls --all --quiet` - List only IDs of all containers
- `docker rm <docker-name_OR_docker-id-number>` - Remove a specific container

<a name="create_a_docker_img"></a>
## Create a docker image & container
- `mkdir docker-project`
- `cd docker-project`
- `touch Dockerfile` - Create docker specification file
- Put commands in Dockerfile
```
# Set the base image
FROM ubuntu:16.04
```

- `docker build --tag image-name:tag .` - Build a docker image
- `docker create --interactive --tty --name container-name image-name bash` - Create a docker container from image; It's entry point point is set to bash shell
- `docker start --interactive --attach container-name` - Re-start container which previsouly exited and open interactive shell
- `docker run --security-opt seccomp:unconfined --interactive image-name bash` - Create/attach to a container with security relaxation
- `docker run --interactive --tty --volume /path/to/folder/on/host:/path/to/folder/in/container image-name bash` - Create a container from image and attach a volume to it
- `exit`-- Exit from the docker container
- `docker commit container-name` -  Command to save modified contents back to image otherwise on stopping container these will changes will be lost

<a name="misc_cmds"></a>
## Other useful docker commands
- `docker stop container-name` - Stop a docker
- `docker rm $(docker container ls --all --quiet)` - Delete all containers
- `docker rmi $(docker images --quiet)` - Delete all images
- `docker rmi $(docker images --quiet --filter "dangling=true")` - Delete dangling images
- `docker run --interactive --detach --name container-name image-name` - Run docker container using above image
- `docker tag <old-repo-name:old-tag-name> <new-repo-name:new-tag-name>` - Rename docker image
- `docker rename <old-name> <new-name>` - Rename docker container
- `docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs docker rm` - Remove docker which are in exit state
- `dockviz images -t` - Docker image tree visualization
- `dockviz images -s` - Docker image flat visualization

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

<a name="container_backup"></a>
## Container backup
- `docker container ls` - Check container is running
- `docker commit --pause container-name image-name` - Issue commit command to save state of running container to an image
- `docker save image-name --output image-name-backup.tar` - Create backup of the docker image
- `docker load --input image-name-backup.tar` - Restore docker image backup

<a name="registry_push"></a>
## Registry -- push
- To push image to docker registry, you need to crate a tag against that docker registry:
```
docker tag <image-name> <docker-registry-host>/<image-name>
docker push <docker-registry-host>/<image-name>
```

<a name="workflow_1"></a>
## Workflow # 1
Create a container from existing backup image.

- docker load   -- `docker load --input image-name-backup.tar`
- docker create -- `docker create --interactive --tty --name container-name image-name bash`
- docker start  -- `docker start --interactive --attach container-name`

<a name="workflow_2"></a>
## Workflow # 2
Rename a docker image.

- docker tag -- `docker tag old_repo:tag new_repo:new_tag`
- docker remove image -- `docker rmi old_repo`

<a name="workflow_3"></a>
## Workflow # 3
Backup a docker container.

- docker commit -- `docker commit --pause container-name image-name`
- docker save   -- `docker save --output image-name-backup.tar image-name`

<a name="workflow_4"></a>
## Workflow # 4
Attach multiple shell to a running container.

- `docker exec --interactive container-name bash`

OR

- `docker exec --interactive --tty container-name /bin/bash -c "export TERM=xterm; exec bash"`
 - Shell without tty width restriction.

:information_source:
Put following in Dockerfile to have shell without width restriction.
```
echo "export TERM=xterm" >> /etc/bash.bashrc
```

<a name="untested_commands"></a>
## Untested commands

- Map host to container
```
ls "/proc/$(docker inspect --format {{.State.Pid}} container-name)/root"
bindfs --map=root/<MY_USER> "/proc/$(docker inspect --format {{.State.Pid}} <container-id>)/root" "/home/<MY_USER>/<SOME_FOLDER>"
```

- Give DISPLAY to container to run GUI
```
docker run -ti -e DISPLAY=$DISPLAY blah-image blah-command
```

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
Logout and login again to use docker without sudo.