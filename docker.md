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

<a name="background"></a>
## Background
- Container: A self contained blackbox which contains all of the software configurations required to run an application. It can be a lightweight or heavyweight box depending upon the end app's requirement. A underlying OS can host multiple containers and each of these container can have totally different software configurations inside them.
- Docker: A company providing solution for containers.
- Docker image: It contains an OS + extra packages + applications.
- Docker container: A running instance of the docker image. Multiple contains can use the same docker image.
- Docker is not end of world, there are alot of others players (LXD, OpenVZ, OpenVZ, Linux VServer) are too in the market.

<a name="docker"></a>
## Docker
`docker --version` - print docker version
`docker version` - print docker version
`docker info` - print detailed info docker installation
`docker run hello-world` - test docker installation

<a name="images"></a>
## Docker images
`docker image ls` - List Docker images
`docker rmi <image-name_OR_image-id-number>` - Remove a specific docker image
`docker images -f dangling=true` - Show dangling* docker image
`docker images purge` - Remove dangling docker images

* Docker images consist of multiple layers. Dangling images are layers that have no relationship to any tagged images. They no longer serve a purpose and consume disk space.

<a name="containers"></a>
## Docker containers
`docker container ls` - Show running containers
`docker container ls --all` - List all container
`docker container ls --all --quiet` - List only IDs of all containers
`docker rm <docker-image_OR_docker-id-number>` - Remove a specific container

<a name="create_a_docker_img"></a>
## Create a docker image & container
- `mkdir docker-project`
- `cd docker-project`
- `touch Dockerfile` - Create docker specification file
- Put commands in Dockerfile
```
# Set the base image
FROM ubuntu
```

- `docker build -t project .` - Build a docker image
- `docker create --interactive --tty --name project-container project bash` - Create a docker container from image; It's entrypoint point is set to bash shell.
- `docker start --interactive --attach project-container` -- Start container and log in to container.
- `exit`-- Exit from the docker container
- NOTE: If you have made any change in your application make sure that you have issued `docker commit` command to save those otherwise on stopping container these will changes will be lost.

<a name="misc_cmds"></a>
## Other useful docker commands
- `docker stop project-container` - Stop a docker
- `docker rm $(docker container ls --all --quiet)` - Delete all containers
- `docker rmi $(docker images --quiet)` - Delete all images
- `docker exec --tty --interactive project-container /bin/bash` - This command will let you land in docker container.
- `docker exec --interactive project-container /bin/bash -c "export COLUMNS=tput cols; export LINES=tput lines; exec bash"`  - This command will let you land in docker container without tty width restriction.
- `docker run --detach --interactive --name project-container project` - Run docker container using above image
- `docker tag <old-repo-name:old-tag-name> <new-repo-name:new-tag-name>` - Rename docker image

<a name="copy_file"></a>
## Copy file to/from from container
```
docker cp foo.txt project-container:/foo.txt
docker cp project-container:/foo.txt foo.txt
```

<a name="copy_folder"></a>
## Copy folder to/from from container
```
docker cp folder project-container:/folder
docker cp project-container:/folder folder
```

<a name="container_backup"></a>
##  Container backup
- `docker container ls` - Check container is running
- `docker commit --pause project-container docker-image-name` - Issue commit command to save state of running container to an image
- `docker save -o docker-image-backup.tar image-name` - Create backup of the docker image
- `docker load --input docker-image-backup.tar` - Restore docker image backup
