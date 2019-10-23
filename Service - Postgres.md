# Service - Postgres

## Prerequisites
Using the power of `Docker` we can install `Postgres` in a moment!
Docker and docker-compose must be already installed. 
_See the appropriate guide for more!_

## Install Postgres
1. Get it from [DockerHub - Docker Official Images - postgres](https://hub.docker.com/_/postgres)
2. Create the `docker-compose.yml`:
    ```bash
    touch ~/docker/postgres/docker-compose.yml
    ```
3.  Add content from the link (1)
    ```docker
    # Use postgres/example user/password credentials
    version: '3.1'
    
    services:
    
      db:
        image: postgres
        restart: always
        environment:
          POSTGRES_PASSWORD: example
    
      adminer:
        image: adminer
        restart: always
        ports:
          - 8080:8080
    ```
    And how it finally looks:
    ```docker
    # Use postgres/12345 as user/password credentials
    version: "3.7"
    
    services:
      db:
        image: postgres
        container_name: postgres
        restart: always
        environment:
          - POSTGRES_PASSWORD=12345
        volumes:
          - ./data:/var/lib/postgresql/data
        ports:
          - "5432:5432"
    ```

## Connect to the server
1. Add PostgreSQL Apt Repository in Ubuntu following the [instructions](https://www.postgresql.org/download/).
2. Install the following packages to get the CLI client (version 11):
    ```bash
    sudo apt-get install postgresql-client-common postgresql-client-11
    ```
3. Fire the container up with `docker-compose up`
4. Run `psql -h localhost -U postgres`
5. Well done! Enjoy :)

## Appendix A - Sources
- [Getting Started With PostgreSQL 11 on Ubuntu 18.04](https://pgdash.io/blog/postgres-11-getting-started.html)
- [Don’t install Postgres. Docker pull Postgres](https://hackernoon.com/dont-install-postgres-docker-pull-postgres-bee20e200198)
- [Exploring PostgreSQL with Docker](https://markheath.net/post/exploring-postgresql-with-docker)
