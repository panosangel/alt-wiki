# Install Ubuntu 22.04 Server

## Installation settings

- **OS:** Ubuntu 22.04 LTS
- **Hostname:** <your-hostname> i.e. zeus
- **Domain:** <your-domain> i.e. olympus.local
- **Local user:** <local-user> i.e. hercules
- **IP:** Managed via DHCP static assign
- **Disk Partitions:**
  - `/boot` - 2G (ext4 file system)
  - `/` - the rest (ext4 file system)
- **Preinstalled services:** OpenSSH

## First commands after installation

Update system:
```bash
sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
sudo reboot
```

Add system user to sudoers (if not already):
```bash
sudo usermod -aG sudo <local-user>
```

Check date and time:
```bash
date
```
If it's not correct, set the appropriate timezone. In my case it is `Europe/Athens`.
```bash
sudo timedatectl set-timezone Europe/Athens
```

## Install packages:

### Basic:

```bash
sudo apt-get install dstat net-tools
```

### Optional:

```bash
sudo apt-get install build-essential
```

## Setup a swap

- [TechRepublic - How to enable the zRAM module for faster swapping on Linux](https://www.techrepublic.com/article/how-to-enable-the-zram-module-for-faster-swapping-on-linux/)
- [How to Configure ZRAM on Your Ubuntu Computer](https://www.maketecheasier.com/configure-zram-ubuntu/)
- [ZRAM as swap on Ubuntu 22.04](https://unixcop.com/zram-as-swap-on-ubuntu-22-04/)
- Check the [System - Swap](System%20-%20Swap.md) guide

## Move /tmp to ram

Add in `/etc/fstab` the following line:
```
tmpfs    /tmp  tmpfs   rw,nodev,nosuid,size=2G 0 0
```

## SSH

Follow the [Services - SSH](./Service%20-%20SSH.md) guide

## Appendix A - Sources

- [How to Install Ubuntu Server 22.04 LTS Step by Step](https://www.linuxtechi.com/install-ubuntu-server-22-04-step-by-step/)
- [How to Install/Configure Unattended Upgrades on Ubuntu 22.04 LTS](https://www.linuxcapable.com/how-to-install-configure-unattended-upgrades-on-ubuntu-22-04-lts/)
- [archlinux - tmpfs](https://wiki.archlinux.org/title/tmpfs)

## Appendix B - Transition

- [The 5 places cron jobs are saved](https://cronitor.io/cron-reference/5-places-cron-jobs-live)
