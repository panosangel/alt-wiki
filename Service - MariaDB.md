# Service - MariaDB

## Prerequisites
Using the power of `Docker` we can install `MariaDB` in a moment!
Docker and docker-compose must be already installed. 
_See the appropriate guide for more!_

## Install MariaDB
1. Get it from [DockerHub - linuxserver/mariadb](https://hub.docker.com/r/linuxserver/mariadb)
2. Create the `docker-compose.yml`
    ```bash
    touch ~/docker/mariadb/docker-compose.yml
    ```
3.  Add content from the link (1)
    ```docker
    version: "2"
    services:
      mariadb:
        image: linuxserver/mariadb
        container_name: mariadb
        environment:
          - PUID=1001
          - PGID=1001
          - MYSQL_ROOT_PASSWORD=<DATABASE PASSWORD>
          - TZ=Europe/London
        volumes:
          - <path to data>:/config
        ports:
          - 3306:3306
        mem_limit: 4096m
        restart: unless-stopped
    ```
    And how it finally looks:
    ```docker
    version: '2'
    
    services:
      mariadb:
        image: linuxserver/mariadb:latest
        container_name: mariadb
        environment:
          - PUID=1000
          - PGID=1000
          - MYSQL_ROOT_PASSWORD=12345
          - TZ=Europe/Athens
        volumes:
          - ./config:/config
        ports:
          - 3306:3306
        mem_limit: 4096m
        restart: always
    ```

## Connect to the database
```bash

```

## User Management

### Create New User
```bash
CREATE USER '<user>'@'<hostname>' IDENTIFIED BY '<password>'
```
For example:
```bash
CREATE USER 'randomguy'@'localhost' IDENTIFIED BY 'password';
```

### Set user permissions
```bash
GRANT ALL PRIVILEGES ON * . * TO '<user>'@'<domain>';
```
and save them:
```bash
FLUSH PRIVILEGES;
```

## Appendix A - Sources:
- [How To Create a New User and Grant Permissions in MySQL](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql)
- TODO: https://stackoverflow.com/questions/16287559/mysql-adding-user-for-remote-access
