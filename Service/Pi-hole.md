# Pi-hole

## Installation

Better using a docker image

## Use recursive DNS

First verify that in the local system there is no process blocking port 53

```shell
sudo lsof -i :53
```

In Ubuntu 22.04 and relevant distros there is a stub process listing on the port.
In order to free the port we do the following:

1. Edit `/etc/systemd/resolved.conf`. Change the following
    ```text
    DNS=127.0.0.1
    ...
    DNSStubListener=no
    ... 
    ```
2. Add symbolic link `/etc/resolv.conf`
    ```shell
    sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
    ```
3. Restart service `sudo systemctl restart systemd-resolved`
4. Verify port 53 is not occupied anymore.
5. Now docker container can mount to port 53

### Revere system changes

In order to revert the procedure take the following steps.

1. Edit `/etc/systemd/resolved.conf`. Change `DNSStubListener=no` to `#DNSStubListener=yes`
2. Add symbolic link `/etc/resolv.conf`
    ```shell
    sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
    ```
3. Restart service `sudo systemctl restart systemd-resolved`

### Libvirt-dnsmasq

Edit default network configuration `sudo virsh net-edit default` and add the following
```text
<dns enable="no"/>
```

## Sources

- [Official Pi-hole](https://pi-hole.net/)
- [Pi-hole documentation](https://docs.pi-hole.net/)
- [Series: What Is Pi-Hole & Why Would You Want To Use It?](https://www.techaddressed.com/reviews/what-is-pi-hole-why-would-you-use-it/)
- [Pi-Hole + Unbound on Docker](https://github.com/chriscrowe/docker-pihole-unbound)

### Troubleshooting

- [Ubuntu: How To Free Up Port 53, Used By systemd-resolved](https://www.linuxuprising.com/2020/07/ubuntu-how-to-free-up-port-53-used-by.html)
- [Setup on Pi in Docker - Bind Error](https://discourse.pi-hole.net/t/setup-on-pi-in-docker-bind-error/19137)
- [Docker unable to bind to port 53](https://discourse.pi-hole.net/t/docker-unable-to-bind-to-port-53/45082/5)
- [Disable or change port of dnsmasq service in libvirt](https://serverfault.com/questions/937188/disable-or-change-port-of-dnsmasq-service-in-libvirt)
