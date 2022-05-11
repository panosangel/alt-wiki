# Service - SSH

## Create strong cryptographic keys on the server

Use a **stronger** Diffie-Hellman Algorithm
```bash
sudo ssh-keygen -G /tmp/moduli.strong -b 2048
sudo ssh-keygen -T /etc/ssh/moduli -f /tmp/moduli.strong
sudo rm /tmp/moduli.strong
```

Re-generate the RSA and ED25519 keys
```
sudo rm /etc/ssh/ssh_host_*  
sudo ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key 
sudo ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key
```

## Create strong cryptographic keys on the client

To create a new strong (4096bit) ssh key-pair run on the **local machine**:
```bash
ssh-keygen -t rsa -b 4096 -a 6000 -C "username@localmachine" -f ~/.ssh/id_rsa_username
```

Copy the newly created public key on the remote machine:
```bash
ssh-copy-id -i ~/.ssh/id_rsa_username.pub <remote_username>@<remote_machine>:<port>
```
**Note:** The remore server should still allow password-logins at this point in order to use `ssh-copy-id`.
Otherwise login to the server and copy user's public key manually. 

## Server configuration

Edit config file:
```bash
sudo nano /etc/ssh/sshd_config
```

Recommended settings:
```
Port 8022 # Change it to anything else than the default 22 to avoid too much spam traffic
ListenAddress XXX.XXX.XXX.XXX # to be changed with the real server IP

HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ed25519_key

LoginGraceTime 120
PermitRootLogin no
StrictModes yes
MaxAuthTries 6
AllowGroups localuser common
PubkeyAuthentication yes

HostbasedAuthentication no
IgnoreRhosts yes
PasswordAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding no

ClientAliveInterval 300
ClientAliveCountMax 3

Compression delayed
AllowTcpForwarding no
Match Group common
  AllowTcpForwarding yes
```
Restart daemon to apply new settings:
```bash
sudo systemctl restart ssh
```

## Access SSH mounts (SFTP)

### Linux

Embedded. No extra software is needed.

### Windows

- [SFTP Net Drive](https://www.nsoftware.com/sftp/netdrive/) (Free for personal use)
- [Cyberduck](https://cyberduck.io) (License needed)
- [Windows sshfs clients](https://nelsonslog.wordpress.com/2017/07/19/windows-sshfs-clients/)
- [How To Use SSHFS to Mount Remote File Systems Over SSH](https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh)
- [How to mount remote directory on Windows using SSHFS-Win](https://codeyarns.com/2018/05/03/how-to-mount-remote-directory-on-windows-using-sshfs-win/)

### macOS

You can install SSHFS on Mac OSX. You will need to download FUSE and SSHFS from the [osxfuse site](http://osxfuse.github.io/)
[Mounting a remote folder on OS X over SSH (Yosemite)](https://amaral.northwestern.edu/resources/guides/mounting-remote-folder-os-x-over-ssh-yosemite)

### Android

Total Commander + plugins support: sFTP(SSH), WebDAV, SAMBA

### Bonus - Android SSH CLI

JuiceSSH gives access to SSH command line

## Monitor Connections

### Show active connections

```bash
sudo netstat -tnpa | grep 'ESTABLISHED.*sshd'
```

### Show active tunnels

```bash
sudo lsof -i -n | egrep '\<ssh\>'
```

## Firewall rules

If the port is set to the default (22): 
```bash
sudo ufw allow ssh
```
Otherwise:
```bash
sudo ufw allow from any to any port <sshd_port> proto tcp
```

## Appendix A - Sources

- [Use Advanced OpenSSH Features to Harden Access to Your Linode](https://www.linode.com/docs/guides/advanced-ssh-server-security/)
- [Harden the World - OpenSSH](http://docs.hardentheworld.org/Applications/OpenSSH/)
- [Hardening SSH](https://medium.com/@jasonrigden/hardening-ssh-1bcb99cd4cef)
- [Limit access to openssh features with the Match option](https://raymii.org/s/tutorials/Limit_access_to_openssh_features_with_the_Match_keyword.html)
