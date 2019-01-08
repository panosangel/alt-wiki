# Application - Gitea

## Prerequisites
Using the power of `Docker` we can install `Gitea` in a moment or two ;)
Docker and docker-compose must be already installed. 
_See the appropriate guide for more!_

## Fetch Gitea
1. Get it from [DockerHub - gitea/gitea](https://hub.docker.com/r/gitea/gitea)
2. Read the instructions at the [Official Page](https://docs.gitea.io/en-us/install-with-docker/)
3.  Create `docker-compose.yml` and add content from the link (2)
    ```bash
    nano ~/docker/gitea/docker-compose.yml
    ```
    Template
    ```docker
    version: "2"
    
    networks:
      gitea:
        external: false
    
    services:
      server:
        image: gitea/gitea:latest
        environment:
          - USER_UID=1000
          - USER_GID=1000
        restart: always
        networks:
          - gitea
        volumes:
          - ./gitea:/data
        ports:
          - "3000:3000"
          - "222:22"
    ```
    And how it finally looks:
    ```docker
    version: "2"
    
    services:
      web:
        image: gitea/gitea:latest
        container_name: gitea    
        environment:
          - USER_UID=1000
          - USER_GID=1000
        volumes:
          - ~/docker/gitea/data:/data
        ports:
          - "3030:3000"
          - "2022:22"
        restart: unless-stopped
    ```

## Initialize application
Fire up:
```bash
docker-compose up
```
The first time the image will create all the necessary configuration files.

Then bring the container down so we can edit the files safely.
```bash
nano ~/docker/gitea/data/gitea/conf/app.ini
```
Change the following:
```
[server]
ROOT_URL         = http://<external-ip>:3030/
DISABLE_SSH      = true
```
Save and restart container.

## Installation
1. Open in the browser `http://<external-ip>:3030/` and click `Register`. That will trigger the installation procedure.
2. Leave the defaults for now and click `Install Gitea`.
3. Register a user. The first user you register is the Administrator.
4. Then bring the container down again so we can edit the files. 

## Basic Configuration
Edit configuration file:
```bash
nano ~/docker/gitea/data/gitea/conf/app.ini
```
Change the following to enhance security:
```
[service]
DISABLE_REGISTRATION              = true
DEFAULT_KEEP_EMAIL_PRIVATE        = true
DEFAULT_ALLOW_CREATE_ORGANIZATION = false

[openid]
ENABLE_OPENID_SIGNIN = false
ENABLE_OPENID_SIGNUP = false
```

## Optional - Enable SSL
1. Get the SSL certificates. _See the appropriate guide for more!_
2. We assume that they are stored at `/etc/letsencrypt`
3. Check the permissions of your docker user regarding the keys and make the appropriate changes.
4. Edit configuration file:
    ```bash
    nano ~/docker/gitea/data/gitea/conf/app.ini
    ```
    Add/edit the following to enable https on the website:
    ```
    [service]
    PROTOCOL         = https
    CERT_FILE        = /etc/letsencrypt/live/<yourdomain.tld>/cert.pem
    KEY_FILE         = /etc/letsencrypt/live/<yourdomain.tld>/privkey.pem
    ROOT_URL         = https://<external-ip>:3030/
    ```
5. Edit `docker-compose.yml` and add the following line:
    ```docker
    volumes:
          ...
          - /etc/letsencrypt:/etc/letsencrypt
    ```

## Firewall rules
Allow incoming connections :
```bash
sudo ufw allow from any to any port 3030 proto tcp
```
**Note:** The modem/router NAT should be configured accordingly to allow incoming connections to the server.

## Appendix A - Sources
- [Official Gitea Docs](https://docs.gitea.io/en-us/)