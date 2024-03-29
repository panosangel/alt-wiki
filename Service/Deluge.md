# Deluge (headless)

## Prerequisites

Docker and docker-compose must be already installed. _See the relevant guide for more!_

## Install Deluge

1. Get it from [DockerHub - linuxserver/deluge](https://hub.docker.com/r/linuxserver/deluge)
2. Create the `docker-compose.yml`
    ```shell
    touch ~/docker/deluge/docker-compose.yml
    ```
3.  Add content from the link (1)
    ```shell
     nano ~/docker/deluge/docker-compose.yml
    ```
    Template:
    ```yaml
    ---
    version: "2.1"
    services:
      deluge:
        image: lscr.io/linuxserver/deluge:latest
        container_name: deluge
        environment:
          - PUID=1000
          - PGID=1000
          - TZ=Europe/London
          - DELUGE_LOGLEVEL=error #optional
        volumes:
          - /path/to/deluge/config:/config
          - /path/to/your/downloads:/downloads
        ports:
          - 8112:8112
          - 6881:6881
          - 6881:6881/udp
        restart: unless-stopped
    ```

4. Adjust the uid/gid, the volume paths and save.

## Initialize & Configure

Fire up:
```shell
docker-compose up
```

The first time the image will create all the necessary configuration files.
**Then bring the container down so we can edit the files safely.**

```shell
nano ~/docker/deluge/config/core.conf
```

Change the following according to your needs:
```
  "move_completed_path": "/downloads/completed",
  "rate_limit_ip_overhead": true,
  "max_active_limit": -1,
  "max_active_downloading": 3,
  "max_active_seeding": -1,
  "allow_remote": true,
  "download_location": "/downloads/incomplete",
  "prioritize_first_last_pieces": false,
  "max_upload_speed": 500.0,
  "seed_time_limit": -1,
  "share_ratio_limit": -1.0,
  "max_download_speed": 4000.0,
  "torrentfiles_location": "/downloads/torrentakia",
  "stop_seed_at_ratio": true,
  "max_upload_slots_global": -1,
  "enabled_plugins": [
    "Label",
    "Blocklist"
  ],
  "random_port": false,
  "autoadd_enable": true,
  "listen_ports": [
    51680,
    51690
  ],
  "stop_seed_ratio": 2.1
  "seed_time_ratio_limit": -1,
  "copy_torrent_file": true,
  "del_copy_torrent_file": true,
  "move_completed": true,
  "add_paused": true,
  "autoadd_location": "/downloads/autoadd",
```

**Save and restart container.**

## Firewall rules

Allow remote connections to the daemon (only from local machines):
```shell
sudo ufw allow from 192.168.1.0/24 to any port 58846 proto tcp
```

Range of incoming ports:
```shell
sudo ufw allow from any to any port 51680:51690 proto tcp
```

**Note:** The modem/router NAT should be configured accordingly to allow incoming connections to the server.

## Optional - Remote Deluge host

### Create link to remote deluge host

In the client's box run:
```shell
ssh -i ~/.ssh/id_rsa_user -fNT -L 58802:127.0.0.1:58846 <remote_user>@<deluge_running_host_ip>
```

### Deluge client
TODO

## Optional - Add proxy
Add/edit the `~/docker/deluge/config/core.conf` to include the following
```
  "proxies": {
    "peer": {
      "username": "",
      "password": "",
      "type": 2,
      "hostname": "127.0.0.1",
      "port": 9090
    },
    ...
  },
```

Create the tunnel with the proxy server.
```shell
ssh -p <sshd_port> -i <private_key_path> -D 9090 -f -C -q -N <remote_user>@<remote_proxy_ip>
```

or make it _persistent_ using autossh:
```shell
autossh -M 0 -f -p <sshd_port> -C -q -N -i <private_key_path> -D 9090 <remote_user>@<remote_proxy_ip>
```
Add the following .torrent in the client and it should give an 'error' followed by the current gateway ip.
```
http://checkmytorrentip.net/torrentip/checkMyTorrentIp.png.torrent
```
