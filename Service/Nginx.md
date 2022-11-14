# Nginx

## Prerequisites

Docker and docker-compose must be already installed. _See the appropriate guide for more!_

## Install NGINX

1. Get it from [DockerHub - linuxserver/nginx](https://hub.docker.com/r/linuxserver/nginx)
2. Create the `docker-compose.yml`
    ```shell
    touch ~/docker/nginx/docker-compose.yml
    ```
3.  Add content from the link (1)
    ```yaml
    ---
    version: "2.1"
    services:
      nginx:
        image: lscr.io/linuxserver/nginx:latest
        container_name: nginx
        environment:
          - PUID=1000
          - PGID=1000
          - TZ=Europe/London
        volumes:
          - </path/to/appdata/config>:/config
        ports:
          - 80:80
          - 443:443
        restart: unless-stopped
    ```
4. Adjust the uid/gid, the volume paths and save.

## Basic Configuration [TO BE VERIFIED]

Edit the main configuration file:
```shell
nano ~/docker/nginx/config/nginx/nginx.conf
```

Add/edit the following content:
```
worker_processes auto;
```
```
events {
    ...
    worker_connections 1024;
    ...
}
```
```
http {

    ##
    # Basic Settings
    ##

    server_tokens off;
    
    ##
    # SSL Settings
    ##
    
    ssl_protocols TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers ECDH+AESGCM:ECDH+AES256:ECDH+AES128:DHE+AES128:!ADH:!AECDH:!MD5;
    # Generate random dhparam: `openssl dhparam 4096 -out /etc/ssl/dhparam.pem`
    ssl_dhparam /config/keys/dhparam.pem;
    
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout 10m;
    ssl_session_tickets off;

    ##
    # Logging Settings
    ##

    error_log /config/log/nginx/error.log warn;

    ##
    # Virtual Host Configs
    ##

    include /config/nginx/site-confs/*.conf;
    
    ##
    # Enhancing Security Headers
    ##

    add_header X-Frame-Options SAMEORIGIN;
    add_header X-Content-Type-Options nosniff;
    add_header X-Xss-Protection "1; mode=block";
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;

    ##
    # Proxy Settings
    ##

    proxy_set_header   Host $host;
    proxy_set_header   X-Real-IP $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header   X-Forwarded-Host $server_name;
}
```

### What does the code above?

1. With `proxy_set_header` directives we bypass the default Nginx behavior and apply the most common production settings.
2. [OPTIONAL] Declare a service under the alias `python-dev` which resolves to `ip:port`.
If we connect to another docker container we change it to `container_name:port`
    ```
    upstream python-dev {
            server 172.17.0.1:8080;
    }
    ```

## Add Virtual Hosts

**Note:** Because of the line `include /config/nginx/site-confs/*.conf;` in the main config file,
in order for a vhost config to be enabled it must have `.conf` extension. Otherwise it is ignored!

Add a virtual host:
```shell
nano ~/docker/nginx/config/nginx/site-confs/dev.domain.tld.conf
```

```
server {
    server_name dev.domain.tld;
    listen 80;

    access_log /config/log/nginx/dev.domain.tld_access.log combined gzip;

    location / {
        proxy_pass      http://172.17.0.1:8080;
        proxy_redirect  off;
    }
}
```

### What does the code above?

1. The `server` directive registers... a server! By default the server `listen`'s on port 80.
Any request that is directed to `server_name` and the specific `location` are proxied to `proxy_pass` service.
2. [OPTIONAL] If an `upstream` directive has been declared in the main config, that alias can be used in the `proxy_pass`.

## Test
1. Run the nginx server using `docker-compose up` on the same folder where the `docker-compose.yml` file is.
2. Start a dev server on the host machine such as`python -m SimpleHTTPServer 8080`.
3. Add firewall rules to allow incoming packets to this port.
4. If needed enable port forwarding on your router to allow remote requests.
5. Run `wget dev.domain.tld`.
6. Enjoy!

## Optional - Enable SSL per vhost

1. Get the SSL certificates. _See the appropriate guide for more!_
2. We assume that they are stored at `/etc/letsencrypt`
3. Check the permissions of your docker user regarding the keys and make the appropriate changes.
4. Edit configuration file:
    ```bash
    nano ~/docker/nginx/config/nginx/site-confs/dev.domain.tld.conf
    ```
    Add/edit the following to enable https on the website:
    ```
    server {
        listen 80;
        server_name dev.domain.tld;
        return 301 https://$host$request_uri;
    }
    
    server {
        server_name dev.domain.tld;
    
        listen 443 ssl http2;
        ssl_certificate /etc/letsencrypt/live/domain.tld/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/domain.tld/privkey.pem;
        
        gzip off;
    
        access_log /config/log/nginx/dev.domain.tld_access.log combined gzip;
        
        location / {
            proxy_pass      http://172.17.0.1:8080;
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
6. Reload server to get new configuration and test it with [SSL Labs](https://www.ssllabs.com/index.html).

## Firewall rules

Allow incoming connections:
```shell
sudo ufw allow from any to any port 80,443 proto tcp
```
**Note:** The modem/router NAT should be configured accordingly to allow incoming connections to the server.

## Optional - Firewall rules for proxied services

Allow incoming connections from docker bridge interfaces (network_mode: bridge). This is the default docker mode.
```shell
sudo ufw allow from 172.0.0.0/8
```
or per service:
```shell
sudo ufw allow from 172.0.0.0/8 to any port <service_port> proto tcp
```

**Note 1:** Firewall rules are necessary for containers to communicate when they DON'T share the same network.  
**Note 2:** docker compose adds all containers in the same network so the above is not needed.

## Appendix A - Sources

- [Setting up a Reverse-Proxy with Nginx and docker-compose](https://dev.to/domysee/setting-up-a-reverse-proxy-with-nginx-and-docker-compose-29jg)
- [Docker compose : NGINX reverse proxy with multiple containers](https://www.bogotobogo.com/DevOps/Docker/Docker-Compose-Nginx-Reverse-Proxy-Multiple-Containers.php)
- [Use NGINX As A Reverse Proxy To Your Containerized Docker Applications](https://www.thepolyglotdeveloper.com/2017/03/nginx-reverse-proxy-containerized-docker-applications/)
- [Using Containers to Learn Nginx Reverse Proxy](https://medium.com/@joatmon08/using-containers-to-learn-nginx-reverse-proxy-6be8ac75a757)
- [Reverse proxy with Nginx](https://github.com/gitbucket/gitbucket/wiki/Reverse-proxy-with-Nginx)
- [Running an NGINX Reverse Proxy with Docker and Let's Encrypt on Google Compute Engine](https://cloud.google.com/community/tutorials/nginx-reverse-proxy-docker)
- [Cipherli.st Strong Ciphers for Apache, nginx and Lighttpd](https://cipherli.st/)
- [Nginx tuning tips: TLS/SSL HTTPS – Improved TTFB/latency](https://haydenjames.io/nginx-tuning-tips-tls-ssl-https-ttfb-latency/)
- [Getting a Perfect SSL Labs Score](https://michael.lustfield.net/nginx/getting-a-perfect-ssl-labs-score)
- [Getting Started with NGINX - Part 4: TLS Deployment Best Practices](https://www.linode.com/docs/web-servers/nginx/tls-deployment-best-practices-for-nginx/#increase-keepalive-duration)
- [How to properly configure your nginx for TLS](https://medium.com/@mvuksano/how-to-properly-configure-your-nginx-for-tls-564651438fe0)
- [GitHubGist - Best nginx configuration for improved security(and performance)](https://gist.github.com/plentz/6737338)
