# Dockerfiles

A Dockerfile is a recipe for building an `image`.

Each command in a Dockerfile creates a read-only layer that is stacked onto the prior layers. The full stack of read-only layers is immutable and is the `image`, When that `image` is run, a writable layer is added on top of a copy of the existing layers, producing a `container`.

## Dockerfile Instructions

### FROM

```Dockerfile
FROM [--platform=<platform>] <image> [AS <name>]
```

The `FROM` instruction defines the image to use as a base for subsequent commands.

### RUN

The `RUN` instruction will execute shell commands a new layer and can take 2 possible forms

* `RUN <command>` (Shell form):
    * This form requires the image to already have a shell (eg `/bin/sh`) installed and executable
* `RUN ["executable", "arg0", "arg2", ..., "last_arg"]` (Exec form):
    * This form doesn't require the image to have a shell, and while it requires a bit more boilerplate, it is much more flexible and resiliant to string quirks.
    * This form is preferred.

### EXPOSE

```Dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

The `EXPOSE` instruction makes `containers` spawned from this image listen on the indicated port number (inside of the `container`).

### COPY

```Dockerfile
COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

The `COPY` instruction copies a file from the host system (at location `src`, which can be an absolute path or a relative path from the directory containing the Dockerfile*) to the container (specifically to the location `dest`). You can also set user and user-group ownership/permissions on the file/directory

The `COPY` instruction has 2 forms (like `RUN`), with the square-bracket exec-style being required for file_paths that contain whitespace.

### ADD

Very similar to `COPY`

### CMD

```Dockerfile
CMD ["executable","param1","param2"] # (exec form, this is the preferred form)
CMD ["param1","param2"]              # (as default parameters to ENTRYPOINT)
CMD command param1 param2            # (shell form)
```

The `CMD` instruction mainly provides an initial executable and parameters to use when a `container` starts running. If there are multiple `CMD` instructions, only the last one will be used.

### USER

```Dockerfile
USER <user>[:<group>]
```

The `USER` instruction sets the user-name (oe UID, with corresponding permissions) and optionally the user-group (GID) for the following Dockerfile instructions (unless/until another `USER` instruction sets a new default user). The default user in `root`, but many official images on Docker Hub set a different user for so that `containers` spawned from that `image` don't give users root powers (even if it is just within the `container`). 

If you need to `root` privileges to extend an `image`, you can include a `USER root` instruction, whatever commands you needs, and then include another `USER <the_prior_user_name>` instruction to reset it (you might have to check github for the base image's Dockerfile to find the correct `USER` name).

## How to build an `image` from a Dockerfile

If your working directory contains a Dockerfile (named `Dockerfil`), you can create the `image` with the following:

```bash
$ docker build . -t <relevant_name>
```

To make it easier to parse `docker ps` and to generally interact with that image, you can use the `-t` option to tag the image with a relevant name.