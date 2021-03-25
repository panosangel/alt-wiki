# Docker Commands

## General

```sh
docker version
```

```sh
docker info
```

## Container commands

### List
```sh
docker container ls
```
Options:
- Show all: `-a`  

### Run
```sh
docker container run [OPTIONS] <image-name> [COMMAND] [ARG...]
```
Options:
- Name: `--name CONTAINER`  
- Interactive Terminal: `-it`  
- Detach: `-d`  
- Publish ports: `-p HOST_PORT:CONTAINER_PORT`  
- Bind mount volume: `-v HOST_FULL_PATH:CONTAINER_FULL_PATH`  
- Environment variable: `--env VAR=VALUE`  
- Remove when stopped: `--rm`  

### Stop
```sh
docker container stop <container-name>
```

### Remove
```sh
docker container rm [OPTIONS] <container-name> [CONTAINER...]
```
Options:
- Remove a running container: `-f`

### Logs
```sh
docker container logs [OPTIONS] <container-name>
```

## FAQ

- Instead of CONTAINER_NAME we can use the first three characters of CONTAINER_ID  
- When we bind mount a volume (option `-v`), we can express the current directory as `$(pwd)`  
- 

## Tricks

### Enter a running container
```sh
docker container exec -it <container-name> /bin/bash
```

## Sources
- [Docker Commands, Help & Tips](https://www.youtube.com/redirect?v=Kyx2PsuwomE)  
