# Application - Docker

## Install Docker CE for Ubuntu
[Official Guide](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## Install Docker Compose
[Official Guide](https://docs.docker.com/compose/install/)

## Add managing users
```bash
sudo usermode -aG docker <my-local-username>
```

## Test docker
Verify that the docker service is running:
```bash
systemctl status docker
```
 Run the first container:
```bash
docker run --rm hello-world
```
Enjoy!!