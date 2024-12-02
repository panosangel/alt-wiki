# SSH Server

## Create strong cryptographic keys on the server

Use a **stronger** Diffie-Hellman Algorithm
```shell
mkdir /tmp/moduli && cd /tmp/moduli
ssh-keygen -M generate -O bits=2048 moduli-2048.candidates
ssh-keygen -M screen moduli-2048 -f moduli-2048.candidates
cp moduli-2048 /etc/ssh/moduli
rm moduli-2048
```

Re-generate the RSA, ED25519 and ECDSA keys just to be on the safe side
```shell
sudo rm /etc/ssh/ssh_host_*  
sudo ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key 
sudo ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key
sudo ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key
```

## Create strong cryptographic keys on the client

To create a new strong (4096bit) ssh key-pair run on the **local machine**:
```shell
ssh-keygen -t rsa -b 4096 -a 6000 -C "username@localmachine" -f ~/.ssh/id_rsa_username
```
or use the newer algorithm based on elliptic curve cryptography:
```shell
ssh-keygen -t ed25519 -C "username@localmachine" -f /path/to/key/file
```

## Transfer public key to the server

Copy the newly created public key on the remote machine:
```shell
ssh-copy-id -i ~/.ssh/id_rsa_username.pub <remote_username>@<remote_machine>:<port>
```

If you have more than a couple of keys in your account and then you will probably face "too many authentication failures" response.
To avoid this add the `-o PubKeyAuthentication=no` option:
```shell
ssh-copy-id -i ~/.ssh/id_rsa_username.pub -o PubKeyAuthentication=no <remote_username>@<remote_machine>:<port>
```

**Note:** The remote server should still allow password-logins (`PasswordAuthentication yes`) at this point in order to use `ssh-copy-id`.
Otherwise, login to the server and copy user's public key manually. 

## Server configuration

Newer Linux distros are coming with a default configuration (`/etc/ssh/sshd_config`) which includes a new instruction (`Include /etc/ssh/sshd_config.d/*.conf`).
This instruction allow us to add our configuration as a separate file inside `/etc/ssh/sshd_config.d` with an ending of `.conf`.

Inside that file, enter the keywords and arguments you want to change from default, or ensure are explicitly addressed. Note that keywords are case-insensitive and arguments are case-sensitive.  
**Note:** It will supersede settings in the /etc/ssh/sshd_config file.

Add config file:
```shell
sudo nano /etc/ssh/sshd_config.d/10-my-settings.conf
```

Recommended settings:
```
Port 8022 # Change it to anything else than the default 22 to avoid too much spam traffic
ListenAddress XXX.XXX.XXX.XXX # to be changed with the real server IP

HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_ecdsa_key

PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3

X11Forwarding no

ClientAliveInterval 300
ClientAliveCountMax 3

AllowGroups localuser common

Match Group common
  AllowTcpForwarding yes
```

Test the syntactic validity of the configuration
```shell
sudo sshd -t
```

Restart daemon to apply new settings:
```shell
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

- [IBM docs - Moduli generation](https://www.ibm.com/docs/en/zos/3.1.0?topic=conversion-moduli-generation)
- [Vultr - How to Harden Server SSH Access Using Advanced OpenSSH Features](https://docs.vultr.com/how-to-harden-server-ssh-access-using-advanced-openssh-features)
- [Harden the World - OpenSSH](http://docs.hardentheworld.org/Applications/OpenSSH/)
- [Hardening SSH](https://medium.com/@jasonrigden/hardening-ssh-1bcb99cd4cef)
- [Limit access to openssh features with the Match option](https://raymii.org/s/tutorials/Limit_access_to_openssh_features_with_the_Match_keyword.html)

### Troubleshooting

- [Fixing ssh-keygen Unknown Option -G or -T on Ubuntu 20.04](https://chewett.co.uk/blog/2535/fixing-ssh-keygen-unknown-option-g-or-t-on-ubuntu-20-04/)
- [Using ssh-copy-id with Username and Password](https://curiouslynerdy.com/ssh-copy-id-too-many-authentication-failures/)
