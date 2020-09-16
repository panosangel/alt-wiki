# Service - MongoDB

## Prerequisites
Using the power of `Docker` we can install `MongoDB` in a moment!
Docker and docker-compose must be already installed. 
_See the appropriate guide for more!_

## Install MongoDB
1. Get it from [DockerHub - Docker Official Images Mongo](https://hub.docker.com/_/mongo)
2. Create the `docker-compose.yml`
    ```bash
    touch ~/docker/mongo/docker-compose.yml
    ```
3.  Add _partial_ content from the link (1)
    ```bash
     nano ~/docker/mongo/docker-compose.yml
    ```
    Template:
    ```docker
     # Use root/example as user/password credentials
     version: '3.1'

     services:

     mongo:
       image: mongo
       restart: always
       environment:
        MONGO_INITDB_ROOT_USERNAME: root
        MONGO_INITDB_ROOT_PASSWORD: example
    ```
    And how it finally looks:

    ```docker
    # Use root/example as user/password credentials
    version: '3.1'
    services:
      mongo:
        image: mongo:latest
        container_name: mongo
        environment:
          - MONGO_INITDB_ROOT_USERNAME:root
          - MONGO_INITDB_ROOT_PASSWORD:example
        volumes:
          - ./data:/data/db
        ports:
          - "27017:27017"
        restart: always
    ```

## Connect to the server
1. Download the appropriate `TGZ` package for your system from [MongoDB Download Center](https://www.mongodb.com/download-center/community)
2. Extract it and run:
 - local: `./bin/mongo`
 - remote: `./bin/mongo mongodb://<hostname>:<port>`
3. Congratulations!

## Appendix A - Sources
- [The mongo Shell](https://docs.mongodb.com/manual/mongo/)
