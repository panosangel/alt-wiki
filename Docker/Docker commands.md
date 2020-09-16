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
docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]
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
docker container stop CONTAINER
```

### Remove
```sh
docker container rm [OPTIONS] CONTAINER [CONTAINER...]
```
Options:
- Remove a running container: `-f`

### Logs
```sh
docker container logs [OPTIONS] CONTAINER
```


## FAQ

- Instead of CONTAINER_NAME we can use the first three characters of CONTAINER_ID  
- When we bind mount a volume (option `-v`), we can express the current directory as `$(pwd)`  
- 

## Tricks

### Enter a running container
```sh
docker container exec -it CONTAINER /bin/bash
```

## Sources

- [YT - Traversy Media - Docker Help Gist](https://www.youtube.com/redirect?v=Kyx2PsuwomE)  
- 
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTcwMDM2NzkzLDE5OTA3OTYyNjMsMTI3OT
U3NTA4MSwtMTM5NDU0NzkwMSwtMzI4ODE5MzExLC0xNDU0NDIz
MTMzLC0yMjAyNDc4MjAsLTY0Mjc3NzE1Miw0MjcxNDkwODksLT
I2Nzk2Mzk3XX0=
-->