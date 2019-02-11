# Application - NGINX

## Prerequisites
Using the power of `Docker` we can install `Deluge` in a moment!
Docker and docker-compose must be already installed. 
_See the appropriate guide for more!_

## Install NGINX
1. Get it from [DockerHub - Official Nginx](https://hub.docker.com/_/nginx)
2. Create the `docker-compose.yml`
    ```bash
    touch ~/docker/nginx/docker-compose.yml
    ```
3. Add content as follows based on research (see sources):
    ```docker
    version: '3'
    
    services:
      reverse_proxy:
        image: nginx:latest
        container_name: nginx_reverse
        volumes:
          - ./nginx.conf:/etc/nginx/nginx.conf
          - ./logs:/var/logs/nginx
        ports:
          - "80:80"
          - "443:443"
        restart: always
    ```

## Basic Configuration
Create configuration file:
```bash
nano ~/docker/nginx/nginx.conf
```
Paste the following content:
```
worker_processes 1;

events { worker_connections 1024; }

http {

    sendfile on;

    error_log /var/logs/nginx/error.log warn;

    log_format compression '$remote_addr - $remote_user [$time_local] '
        '"$request" $status $upstream_addr '
        '"$http_referer" "$http_user_agent" "$gzip_ratio"';

    upstream python-dev {
        server 172.17.0.1:8080;
    }

    proxy_set_header   Host $host;
    proxy_set_header   X-Real-IP $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header   X-Forwarded-Host $server_name;

    server {
        server_name dev.domain.tld;
        listen 80;

        access_log /var/logs/nginx/dev.domain.tld_access.log compression;

        location / {
            proxy_pass      http://python-dev;
            proxy_redirect  off;
        }
    }
}
```

## What does the code above?
1. Declare a service under the alias `python-dev` which resolves to `ip:port`.
If we connect to another docker container we change it to `container_name:port`
    ```
    upstream python-dev {
            server 172.17.0.1:8080;
    }
    ```
2. With `proxy_set_header` directives we bypass the default nginx behavior and apply the most common production settings.
3. The `server` directive registers... a server! By default the server `listen` on port 80.
Any request that is directed to `server_name` and the specific `location` are proxied to `proxy_pass` service.

## Test
1. Run the nginx server using `docker-compose up` on the same folder where the `docker-compose.yml` file is.
2. Start a dev server on the host machine such as`python -m SimpleHTTPServer 8080`.
3. Add firewall rules to allow incoming packets to this port.
4. If needed enable port forwarding on your router to allow remote requests.
5. Run `wget dev.domain.tld`.
6. Enjoy!

## Optional - Enable SSL
1. Get the SSL certificates. _See the appropriate guide for more!_
2. We assume that they are stored at `/etc/letsencrypt`
3. Check the permissions of your docker user regarding the keys and make the appropriate changes.
4. Edit configuration file:
    ```bash
    nano ~/docker/nginx/nginx.conf
    ```
    Add/edit the following to enable https on the website:
    ```
    server {
        server_name dev.domain.tld;
    
        listen 443 ssl;
        ssl_certificate /etc/letsencrypt/live/domain.tld/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/domain.tld/privkey.pem;
        include /etc/letsencrypt/options-ssl-nginx.conf;
    
        access_log /var/logs/nginx/domain.tld_access.log compression;
    
        location / {
            proxy_pass      http://python-dev;
            proxy_redirect  off;
        }
    }
    ```
5. Edit `docker-compose.yml` and add the following line:
    ```docker
    volumes:
      ...
      - /etc/letsencrypt:/etc/letsencrypt
    ```
6. Create a file with common SSL directives:
    ```
    nano /etc/letsencrypt/options-ssl-nginx.conf
    ```
    Add the following content:
    ```
    ssl_protocols TLSv1.1 TLSv1.2;
    
    ssl_prefer_server_ciphers on;
    ssl_ciphers ECDH+AESGCM:ECDH+AES256:ECDH+AES128:DHE+AES128:!ADH:!AECDH:!MD5;`
    ```

## Appendix A - Sources
- [Setting up a Reverse-Proxy with Nginx and docker-compose](https://dev.to/domysee/setting-up-a-reverse-proxy-with-nginx-and-docker-compose-29jg)
- [Docker compose : NGINX reverse proxy with multiple containers](https://www.bogotobogo.com/DevOps/Docker/Docker-Compose-Nginx-Reverse-Proxy-Multiple-Containers.php)
- [Use NGINX As A Reverse Proxy To Your Containerized Docker Applications](https://www.thepolyglotdeveloper.com/2017/03/nginx-reverse-proxy-containerized-docker-applications/)
- [Using Containers to Learn Nginx Reverse Proxy](https://medium.com/@joatmon08/using-containers-to-learn-nginx-reverse-proxy-6be8ac75a757)
- [Reverse proxy with Nginx](https://github.com/gitbucket/gitbucket/wiki/Reverse-proxy-with-Nginx)
- [Running an NGINX Reverse Proxy with Docker and Let's Encrypt on Google Compute Engine](https://cloud.google.com/community/tutorials/nginx-reverse-proxy-docker)
