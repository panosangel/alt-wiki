# Docker

## Installation

### Docker Engine

- [Official Docs - Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Official Docs - Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)

### Docker Compose

[Official Docs](https://docs.docker.com/compose/install/)

## Add managing users

```shell
sudo usermod -aG docker <my-local-username>
```

## Test docker

Verify that the docker service is running:
```shell
systemctl status docker
```

 Run the first container:
```shell
docker run --rm hello-world
```

## Tuning

Edit `/etc/docker/daemon.json`, create it if it doesn't exist, and add the following:

### Restrict network pools size

```json
{
  "default-address-pools": [
    {
      "base": "172.16.0.0/12",
      "size": 24
    }
  ]
}
```

### Increate container shutdown timer (default 10s)

```json
{
  "shutdown-timeout": 15
}
```

## Enjoy!!
