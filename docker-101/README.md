# Docker

Cross posted to [dev.to](https://dev.to/benmatselby/docker-knowledge-to-get-you-through-the-day-47bl).

**Time allocation:** 60 minutes

This was last run on 15th March 2021 and 39 people attended.

This workshop will be a high level overview of the basic docker commands you will use daily. It's an entry level workshop, so if you're comfortable with docker on the CLI, this probably isn't for you.

What we will cover:

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
- Container - A virtualised run time environment of a docker image.

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

This shows us all the images that are pre-built and stored on your machine. Any images listed here are available to be run locally.

Let's pull down the `hello-world` image.

`docker pull hello-world`

If you now run `docker images` you should see

```shell
❯ docker images
REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
hello-world     latest    d1165f221234   7 days ago     13.3kB
```

## Running basic commands

Let's now run that image as a container.

`docker run hello-world`

So `docker` is the main command. `run` is the subcommand which allows us to run the `hello-world` image as a container.

If we now run `docker ps -a` you will see containers that are: running, have ran and stopped.

```shell
❯ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED              STATUS                          PORTS     NAMES
1fd4a9f1b80b   hello-world   "/hello"   About a minute ago   Exited (0) About a minute ago             practical_blackwell
```

Ideally, we want to cleanup after ourselves. So lets run the container again.

`docker run --rm hello-world`

Notice the `--rm` flag, which will remove the container once finished.

If we run `docker ps -a` again you will notice there isn't a new entry in the list.

## Defining

Let's cover two scenarios

1. An environment you want to work in
2. Shipping something.

### Environment

Let's look at a [VS Code Dev Environment](https://github.com/microsoft/vscode-dev-containers/blob/v0.163.1/containers/go/.devcontainer/base.Dockerfile).

Key items to cover off are:

- `FROM` - The base image is `golang:1`. These can be chained if required, each adding another layer to the environment.
- `ENV` - Environment variables for the image
- `ARG` - Arguments that can be overridden when building the image.
- `COPY` - The ability to copy files from the host into the image (These will persist in the image). The host file is on the left, whilst the image location is on the right hand side of the declaration.
- `RUN` - Running commands to build the image. You can see that commands are grouped together as one `RUN` command. This is to keep those defined as a layer.

### Shipping something

Let's look at [Hagen](https://github.com/benmatselby/hagen/blob/main/Dockerfile) which is a CLI tool for getting some data out of GitHub. This is running a Go binary inside it.

Key items to cover off are:

- `WORKDIR` - Define what is the working directory of the image, and the container.
- `alpine` - Base images are generally linux based. [Alpine](https://www.alpinelinux.org) is a very small linux distribution. If you can use it, it will keep your images small (which ultimately saves time and money). Alpine, by default, uses `apk` not `apt` to install packages.
- `FROM scratch` - Start with a bare bones container, nothing in there. This is classed as a [multi-stage build](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#use-multi-stage-builds).
- `ENTRYPOINT` - This defines what should be executed when the image is instantiated as a container. Here you can see we are running the `hagen` command, which has been placed in `/usr/bin/`.
- `CMD` - Here we using `CMD` to pass `--help` to `hagen`. So if you do not pass any other arguments into the container when running it, it will provide you will a help menu.

## Building

Let's look at the Hagen example again.

```shell
docker build -t benmatselby/hagen .
```

Breaking this down we have `docker build` as the command.

`-t` defines the string we will tag the image with. You can specify multiple tags, and they will all show when you run `docker images`.

And finally the `.` provides the context to the build command.

## Running more advanced commands

For a more in-depth review of what you can do whilst running containers, please see the [documentation](https://docs.docker.com/engine/reference/commandline/run/).

An example of sharing environment variables, and files from the host to the container.

```shell
docker run \
  --rm \
  -t \
  -eGITHUB_OWNER \
  -eGITHUB_ORG \
  -eGITHUB_TOKEN \
  -v "${HOME}/.benmatselby":/root/.benmatselby \
  benmatselby/hagen
```

Breaking this down.

- `--rm` will remove the container from the system once finished execution.
- `-t` allocates a `tty`. See [this](https://stackoverflow.com/questions/30137135/confused-about-docker-t-option-to-allocate-a-pseudo-tty).
- `-e` provides environment variables to the container. If the names match you can do it this way. If not you can provide them in the `-e foo=bar` format.
- `v` defines a [volume](https://docs.docker.com/engine/reference/commandline/volume_create/), which is essentially a file share between the host and the container. The format is `host:container`. This allows us to share the configuration used for hagen, with the container.

What if you want to get your terminal back, and run the container unsupervised? Then you can do this

```shell
$ echo "hello world" > index.html
$ docker run \
  --rm \
  -p 8080:80 \
  -v "$(pwd):/usr/share/nginx/html" \
  -d \
  nginx
dcc55061e1118f7b6ccf2c3708d717531bf0c92327ed42f94f08d13020e901ea
$ curl http://localhost:8080/
$ rm index.html
```

The extra bits here are:

- `-p 8080:80` links the host port (`8080`) to the container port (`80`) that has been exposed.
- `-d` will disconnect the container from your terminal, and then output the container id back to you.

## Process management

Want to know what processes are running?

`docker ps` will give the current containers running.

`docker ps -a` will give you all the containers that are running and stopped.

Want to connect to a container that is running, then you can do this:

`docker exec -it dcc55061e1118f7b6ccf2c3708d717531bf0c92327ed42f94f08d13020e901ea bash`

Breaking this down, we have `docker exec` for executing something, but we run interactively with `-it` options and then run `bash`. This could be `zsh` or actually any command that is available in the container.

Want to know more about a running container, then run `docker inspect dcc55061e1118f7b6ccf2c3708d717531bf0c92327ed42f94f08d13020e901ea`.

To stop a container you can run `docker stop dcc55061e1118f7b6ccf2c3708d717531bf0c92327ed42f94f08d13020e901ea`.

## Cleanup

- `docker rm $(docker ps -aq)` - Remove all containers.
- `docker rmi $(docker images -q)` - Remove all images stored locally.
- `docker rmi $(docker images -f "dangling=true" -q)` - Remove all images that are not being used - "dangling".
- `docker stop $(docker ps -aq)` - Stop all containers running.

Or

`docker system prune`

## Tidbits

- [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices)
