# Docker - Tips & Tricks

## Containers


### Disposable containers
You want to create a new container for some testing and you have no reason to keep it after closing it.

Run the container create/run command with the `--rm` option.

### Change hostname
By default, the docker container's hostname is the same as its id. It's pretty clear that when you have more than 2-3 containers it can get messy and lose control.

The solution is very easy. When creating a container choose the `-h` or `--hostname` option.

### Copy files between containers and the host

TODO

### Get access in a docker compose service
Sometimes we want to run a couple of commands on-the-fly inside a service that has been initiated by docker compose.
```bash
docker-compose exec <service-name> /bin/bash
```

