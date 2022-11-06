# Service - Samba

## Install Samba

```shell
sudo apt install samba smbclient cifs-utils
```

## Create a public folder to share

Next, create the public folder where everyone should have access to as is defined in the Samba configuration.
```shell
sudo mkdir -p /mnt/storage/public
```

Set the permissions so that everyone can read and write to it.
```shell
sudo chown -R nobody:nogroup /mnt/storage/public
sudo chmod -R 0775 /mnt/storage/public
```

## Samba group & users

Create a user group for the private group shares:
```shell
sudo addgroup <common-group>
```

Then add an **existing system user** to the group by running the commands below:
```shell
sudo usermod -aG <common-group> <username>
```

Finally, all users who need to access a protected samba share will need to type a password.
Samba has its own user management system. However, any user existing on the samba user list must also exist within the /etc/passwd file.
Samba doesn't use the system account password, so we need to set up a Samba password for our user account:
```shell
sudo smbpasswd -a <username>
```

Edit an **existing samba user's** password with:
```shell
sudo smbpasswd <username>
```

Finally, list all samba users:
```shell
sudo pdbedit -L -v
```

## Configure a group folder to share

If not done already (_See the [Users & Filesystem](System%20-%20Users%20&%20Filesystem%20Layout.md)  guide for more!_): 
- Create a common group folder.
- Set ownership of the folder to `root:common`
- Set the permissions to `0770` so that only group `common` members can read and write to it.

## Samba private shares

If not done already (_See the [Users & Filesystem](System%20-%20Users%20&%20Filesystem%20Layout.md) guide for more!_): 
- Create private user's (i.e jim) folder.
- Set ownership of the folder to `jim:jim`
- Set the permissions to `0770` so that only user `jim` user can read and write to it.

## Configure Samba

Make a backup copy of the configuration file and edit it:
```shell
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.ORIGINAL
sudo nano /etc/samba/smb.conf
```

Configuration example:
```
[global]
  workgroup = WORKGROUP
  server string = %h has never left home!
  interfaces = lo enp3s0
  bind interfaces only = yes

  log file = /var/log/samba/%m.log
  max log size = 1000
  logging = file

  passwd program = /usr/bin/passwd %u
  passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .

  server role = standalone server
  obey pam restrictions = yes
  unix password sync = yes
  pam password change = yes
  map to guest = bad user

# Samba Shares

[public]
  comment = Public anonymous access
  path = /mnt/storage/public
  guest ok = yes
  guest only = yes
  writable = yes
  force create mode = 775
  force directory mode = 775
  force user = nobody

[common]
  comment = Storage area for the few
  path = /mnt/storage/common
  writable = yes
  valid users = @commom
  force create mode = 770
  force directory mode = 770
  inherit permissions = yes

[homes]
  comment = Home storage area of %S
  path = /mnt/storage/%S
  writable = yes
  valid users = %S
  force create mode = 770
  force directory mode = 770
```

It is  recommended that you verify the Samba configuration each time you update the /etc/samba/smb.conf file using the `testparm` utility
You can simply execute it as follows:
```
testparm
```

Restart the daemon:
```shell
sudo systemctl restart smbd
```

## Access Samba

### Linux

In file manager **Nemo** File -> Connect to server
```
Server: <server_ip>
Type: Windows share
```

### Windows
TODO

### macOS

Finder + Go -> Connect to Server.
- Tested on macbook pro. It works fine on read/write!
- Permissions as well.

### Android

Total Commander + LAN plugin.
- Tested on Xiaomi A2. It works fine on read/write!
- Tested on Poco X3 Pro. It works fine on read/write!
- Permissions as well.

## Firewall rules

```shell
sudo ufw allow from 192.168.1.0/24 to any port 139,445 proto tcp
sudo ufw allow from 192.168.1.0/24 to any port 137,138 proto udp
```

## Appendix A - Sources

- [smb.conf — The configuration file for the Samba suite](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)
- [Ubuntu Official Tutorials - Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba)
- [How to Access Shares on Windows 11 from Ubuntu](https://websiteforstudents.com/how-to-access-shares-on-windows-11-from-ubuntu/)
- [How to configure Samba Server share on Ubuntu 18.04 Bionic Beaver Linux](https://linuxconfig.org/how-to-configure-samba-server-share-on-ubuntu-18-04-bionic-beaver-linux)
- [How to configure Samba Server share on Ubuntu 22.04 Jammy Jellyfish Linux](https://linuxconfig.org/how-to-configure-samba-server-share-on-ubuntu-22-04-jammy-jellyfish-linux)
- [Ubuntu 18.04 - Samba shares problem fix](https://www.dedoimedo.com/computers/ubuntu-beaver-samba-shares.html)
- [How do I define a samba share so that every user can only see its own home?](https://unix.stackexchange.com/questions/36853/how-do-i-define-a-samba-share-so-that-every-user-can-only-see-its-own-home)
- [Linux Mint Tutorials - Using the \[homes\] Share in Samba](https://forums.linuxmint.com/viewtopic.php?f=42&t=77063&sid=464f3d114dc81a360b841436997d9edc)
- [Samba - What is the difference between %S, %u and %U variables in sbm.comf](https://lists.samba.org/archive/samba/2012-November/169927.html)
- [O'Reilly Samba book - Table C.1: Variables in Alphabetic Order](https://www.oreilly.com/openbook/samba/book/appc_01.html#appc-88529)
- [Configure Samba to Bind to Specific Interfaces](https://wiki.samba.org/index.php/Configure_Samba_to_Bind_to_Specific_Interfaces)
- [Samba AD DC Port Usage](https://wiki.samba.org/index.php/Samba_AD_DC_Port_Usage)
- [Easily Install and Configure Samba File Server on Ubuntu 22.04](https://kifarunix.com/easily-install-and-configure-samba-file-server-on-ubuntu-22-04/)
- [A big change for Samba in Ubuntu 22.04 and how to get around it](https://www.techrepublic.com/article/big-change-samba-ubuntu/)
