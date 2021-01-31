# Introduction

I recently learned about Docker, so let's store the notes I took and some 
configuration files in here.

# Docker Commands

To obtain the list of commands available in docker, simply type `docker` in the
command line. To obtain more information from a certain command, use the
`--help` flag.

```bash
docker <command> --help```

To download something, type the command below. Where image stands for the 
image's name you need to download.

```bash
docker pull image
```

To see the images available use:

```bash
docker images
```

To run an image you need to specify the image to execute and the tag assigned
to the image. If no message pops-up it is because the process has just started
and it is waiting.

```bash
docker run <image>:<tag>
```

The terminal where this command is executed now executes the container, and 
therefore it cannot be used. Unless the flag `-d` is added to the command.

```bash
docker run -d <image>:<tag>
```

Furthermore, to specify the port redirection between the host and the container,
use the `-p host-port:container-port` flag. By default it uses ports `8080:80`. 
Several ports can be mapped. To do so, simply add another `-p` flag.

```bash
docker run -p <hp>:<cp> <image>:<tag>
docker run -p <hp>:<cp> -p <hp>:<cp> ... <image>:<tag>
```

Not to just trust the names given by default (harder to remember), you can give
the container your own name by simply using the `--name` flag.

```bash
docker run --name <name>
```

To host some simple static content execute the command:

```bash
docker run -v <path-to-content>/<path-suggested-container-developer>:<permissions>
```

You are also able to copy the contents of a container to another. By simply 
including the `--volumes-from` flag.

```bash
docker run --volumes-from <container>
```

To see the running containers, run any of the following commands:

```bash
docker container ls
docker ps
```

To stop a container, you need to use the `CONTAINER ID` displayed after 
executing the previous command; or simply use the `NAME` of the container.. The 
execution should return the `CONTAINER ID/NAME` from the stopped contianer.

```bash
docker stop <container-id/name>
```

This means that the container has been stopped, by niot deleted. The same 
container can be started again by typinng the `CONTAINER ID` or the `NAME`.

```bash
docker start <container-id/name>
```

To actually delete a container, instead of stopping it, it must be removed. 
Regard that a running container *cannot* be removed. To force it use the `-f` 
flag.

```
docker rm <container-id/name>
```

To delete ALL container, command redirection is suggested: 
`docker rm $(docker ps -aq)`.

To execute in interactive mode run:

```bash
docker exec -it <container> <command>
```

## Create own image

In order to create an image, it is neede d a file that specifies the different 
options of that image. That file is called `Dockerfile`. The `Dockerfile` must 
be created under the root directory.

Go to the 
[official documentation](https://docs.docker.com/engine/reference/builder/) to 
know what to include in the `Dockerfile` file.

To build that image, execute the command `build`

```bash
docker build <path-to-dockerfile>
```

The image should be displayed after `docker image ls`.

### .dockerignore

Pretty much like a `.gitignore` file.

### Caching

When building an image, docker recovers the cached steps (unchanged steps that
have already been computed). But, after a certain step is detected as modified
(i.e.: include new source code, should change `ADD` step) all the following 
steps need to be re-executed.

To take advantage of this feature, some steps can be altered or included to 
avoid that re-execution of consuming resources (i.e.: re-install packages).

### Size optimization

Each image usually has multiple tags available. By default, the `latest` tag is 
the one selected for a newly downloaded image.

Other important tag is the `alpine` tag. It is a light-weight, simple and secure 
linux distribution with a small image size. In Docker's images repository it is 
given the syntax for every specify image. 

## Tags, Versioning and Tagging

Tags allow to control image version. It also avoidsbreaking changes. It is safe.

### Tagging Override

When creating an image with the same name and tag, the previous tag is 
overrided, and it is displayed as `<nonw> <none>` when showing the list of 
images downloaded (`docker image ls`).

### Tagging Own Images

Just execute thw following command:

```bash
docker tag <old-repository>:<old-tag> <new-repository>:<new-tag>
```

With latest, a new image will be created. With other images, the command acts as
a rename of the image. It allows to have version control.

## Docker Registries

Highly scalable server side application that stores and lets you distribute 
Docker images.
Used ub CD/CI Pipeline (Continous Delivery/Continous Integration).
Run your applications.

There are public and private registries. 

Public registries:
- Docker Hub  (allows public and private).
- quay.io
- Amazon ECR

To create a new repository, go to [hub.docker.com ](hub.docker.com ), sign in 
and create a new repository.

```bash
docker push <username>/<repository>:<tag>
```

**DISCLAIMER**: _The username or repository should be part of the image name. 
Meaning that the format of the repository column when `docker image ls` is done
should have that format._

## Docker Debug

Sometimes, you need to inspect/debug the content of a container. To do so, use:

```bash
docker inspect <id/name>
```

Some useful piece of information, are logs. They provide information on traffic,
usage, etc.

```bash
docker logs <id/name>
```

Using the `-f` flag leaves a process that blocks the terminal and that is 
updated on every log interaction.

```bash
docker logs -f <id/name>
```

To directly jump into a container is by simply typing the `exec` command.

```bash
docker exec -it <id/name> /bin/<cmd>
```

