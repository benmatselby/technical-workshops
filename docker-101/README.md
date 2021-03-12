# Docker

**Time allocation:** 30 minutes

This workshop will be a high level overview of the basic commands you will use for docker daily. It's an entry level workshop, so if you're comfortable with docker on the CLI, this probably isn't for you.

- [Commands](#commands)
- [Pulling](#pulling)
- [Running basic commands](#running-basic-commands)
- [Defining](#defining)
- [Building](#building)
- [Running more advanced commands](#running-more-advanced-commands)
- [Process management](#process-management)
- [Cleanup](#cleanup)
- [Tidbits](#tidbits)

## Notes

This workshop will use the `hello-world` example from the Docker Hub registry.

Some up front terminology

- Image - A definition of an environment.
- Container - A running instantiation of an Image.

## Commands

We need to make sure we have `docker` installed, so let's run that first.

`docker`

This will output a raft of commands that we can make use of. Some of the commands are the same, e.g.

- `docker images`
- `docker image ls`

Take some time to review the output from the main `docker` command.

## Pulling

Let's start with understanding what images we have on our machine.

`docker images`

This shows us all the images that are pre-built and stored on our machine. Any images listed here are available to be run locally.

You can either run an image, and if not present, will pull it down. However, we can pull down ahead of time. Let's do that.

`docker pull hello-world`

## Running basic commands

Let's now run that image as a container.

`docker run hello-world`

So `docker` is the main command. `run` is the subcommand which allows us to run the image `hello-world`.

If we now run `docker ps -a` you will see containers that have ran, stopped, and left there.

Ideally, we want to cleanup after ourselves. So lets run the container again.

`docker run --rm hello-world`

Notice the `--rm` flag, which will remove the container once finished.

If we run `docker ps -a` again you will notice there isn't a new entry in the list.

## Defining

Let's cover two scenarios

1. An environment you want to work in
2. Shipping something.

### Environment

Let's look at a [Jenkins agent](https://github.com/emisgroup/jenkins-infrastructure/blob/develop/dockerfiles/agent/go-1.16/Dockerfile)

Key items to cover off in the demo

- `FROM` - The base image
- `USER` - Ability to specify users
- `ENV` - Environment variables for the image
- `COPY` - The ability to copy files from the host into the image (These will persist in the image)
- `RUN` - Running commands to build the image. This is a definition.

### Shipping something

Let's look at the [Hello Production](https://github.com/emisgroup/hello-production/blob/develop/src/server/Dockerfile) example.

Key items to cover off in the demo

- `ARG` - The ability to pass arguments into the image creation process
- `WORKDIR` - Define what is the working directory of the image, and the container.
- `alpine` - Base images generally linux based, alpine is a very small linux distribution. If you can use it, it will keep your images small (saves time and money). But uses `apk` not `apt`.
- `FROM scratch` - Start with a bare bones container, nothing in there.
- `EXPOSE` - What ports to expose if you are running something port based. Here, Hello runs on port 8080, so we expose that.
- `CMD` - The entry point for the container when it starts.

## Building

## Running more advanced commands

- Interactive
- Mounting
- Sharing environment variables

## Process management

## Cleanup

## Tidbits

- `docker rm $(docker ps -aq)` - Remove all containers.
- `docker rmi $(docker images -q)` - Remove all images stored locally.
- `docker rmi $(docker images -f "dangling=true" -q)` - Remove all images that are not being used - "dangling".
- `docker stop $(docker ps -aq)` - Stop all containers running.
