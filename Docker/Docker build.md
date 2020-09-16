# Docker `build`

# Build

Build a new image based on another image and an instruction file (Dockerfile).
The path `.` is the context filesystem of the build.
Traditionally the `Dockerfile` position in in the root of this context.
```sh
docker build .
```
Options:
- Set a name: `-t IMAGE_NAME`
- Overwrite Dockerfile position: `-f NEW/PATH/OF/DOCKERFILE`
- Invalidate cache: `--no-cache`

## Instructions

### FROM

`FROM <image>[:<tag>] [AS <name>]`

It's always the first line in the Dockerfile

### LABEL

`LABEL <key>=<value>`

The `LABEL` instruction adds metadata to an image. For example:

- `LABEL description="This is an awesome docker guide"`
- `LABEL maintainer="somebody@somewhere.sth"`

### WORKDIR

Acts like CLI `cd` and set the working directory for the following instructions: `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD`.

`WORKDIR /NEW/PATH/`

**Note:** If a relative path is provided, it will be relative to the path of the previous `WORKDIR` instruction.

### RUN

-   `RUN <command>` (_shell_ form, the command is run in a shell, which by default is `/bin/sh -c` on Linux or `cmd /S /C` on Windows)
-   `RUN ["executable", "param1", "param2"]` (_exec_ form)

Example:
- Shell form: `RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'`
- Shell form: `RUN mkdir -p /var/www/html/newdir`
- Exec form: `["mkdir","-p","/var/www/html/newdir"]`

### COPY

-   `COPY [--chown=<user>:<group>] <src>... <dest>`
-   `COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]` (this form is required for paths containing whitespace)

For example:
- Copy everything from the context path to the image 
`COPY . .`
- `COPY example.json /var/www/html`

### EXPOSE

`EXPOSE <port> [<port>/<protocol>...]`

Documents which ports an image is exposing to the host.

The default protocol is TCP so `EXPOSE 80` is equal to `EXPOSE 80/tcp`.

During runtime `-p` option mush be used to bind a host port to the exposed one.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2OTU2OTYzNzcsLTE3OTYzNTA5NTEsNz
M1NzI4NjQzLDEwMTA5NDAxNjEsMjAxMjAzNTY3OCwtMTk5OTk2
NjkzMyw0OTkxMDM0MDUsLTIwODU5ODQ2MTgsODI4ODczODA0XX
0=
-->