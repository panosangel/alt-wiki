# Docker composer

## CLI

**Command:** `docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]` 

### Run

Builds, (re)creates, starts, and attaches to containers for a service.
```sh
docker-compose up
```
Options:
- Run in background: `-d`

### Clean-up

Stops containers and removes containers, networks, volumes, and images created by `up`.
```sh
docker-compose down
```

## Compose file

### Example

**Filename:** `docker-compose.yml` or `docker-compose.yaml`
```yaml
version: "3.7"

services:
  app:
    container_name: docker-node-mongo
    restart: always
    build: ./node
    volumes:
      - ./app:/var/www/html
    ports:
      - 8080:3000
    links:
      - mongo
  mongo:
    container_name: mongo
    image: mongo
    ports:
      - "27017:27017"
```

### Networking

```yaml
version: "3.7"

services: 
  ...
  ...

networks:
  network_name:
    name: <docker_network_name> # From version 3.5+
    driver: <network_driver>
    driver_opts:
      com.docker.network.bridge.name: <interface_name>

```
## Troubleshooting

### Exiting with code 0

Add in compose file the `command` as shown bellow: 
```bash
services:
  my-test:
    command: tail -f /dev/null
```
Then you can run a shell to get into the container using the following command:
`docker exec -it composename_my-test_1 bash`

## Appendix A - Sources

### General
- [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
- [Compose file versions and upgrading](https://docs.docker.com/compose/compose-file/compose-versioning/)

### Networking
- [Work with network commands](https://docs.docker.com/v17.09/engine/userguide/networking/work-with-networks/#create-networks)
- [docker network create](https://docs.docker.com/engine/reference/commandline/network_create/)
- [Bind container ports to the host](https://docs.docker.com/v17.09/engine/userguide/networking/default_network/binding/)
- [Docker - how to set iface name when creating a new network](https://stackoverflow.com/questions/43981224/docker-how-to-set-iface-name-when-creating-a-new-network)

### Tips
- [Docker Compose keep container running](https://stackoverflow.com/questions/38546755/docker-compose-keep-container-running)
