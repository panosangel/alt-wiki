# Raspberry Pi 4

## Install OS

## Enable System Services

## Install Kodi

## Configure Kodi

## Automount remote disk via SSHFS

### Optimize wait-online service
Let's ignore all none important network interfaces by adding the `--ignore <interface_name>` in the `ExecStart`
Edit `/lib/systemd/system/systemd-networkd-wait-online.service`
```bash
[Service]
...
ExecStart=/lib/systemd/systemd-networkd-wait-online --ignore eth0
...
```

### Create mount unit
`sudo nano /lib/systemd/system/mnt-name.mount`
```bash
[Unit]
Description=Mount remote <Name> fs with sshfs
Wants=systemd-networkd-wait-online.service
After=systemd-networkd-wait-online.service

[Install]
WantedBy=multi-user.target

[Mount]
What=<user>@<remote_host>:<remote_mount>
Where=/mnt/<name>
Type=fuse.sshfs
Options=_netdev,delay_connect,allow_other,IdentityFile=</pah/to/id_rsa_key>,reconnect,ServerAliveInterval=30,ServerAliveCountMax=5,uid=1000,gid=1000,sshfs_debug
TimeoutSec=60
```
**Note:** Filename must be the same as the `Where` name i.e if `mnt-something.mount` then `Where=/mnt/something`

### Enable systemd unit
```bash
sudo systemctl daemon-reload
sudo systemctl enable mnt-name.mount
sudo systemctl start mnt-name.mount
```

### Sources
[Mount Remote Filesystems Using sshfs and systemd](https://www.buggycoder.com/mount-remote-fs-sshfs-systemd/)  
[NFS mounts fail on startup (netctl needed wait-online)](https://bbs.archlinux.org/viewtopic.php?pid=1728151#p1728151)  
