## Service - Samba

## Install Samba
```bash
sudo apt-get install samba
```

## Create a public folder to share
Next, create the public folder where everyone should have access to as is defined in the Samba configuration.
```bash
sudo mkdir -p /mnt/storage/public
```
Set the permissions so that everyone can read and write to it.
```bash
sudo chown -R nobody:nogroup /mnt/storage/public
sudo chmod -R 0775 /mnt/storage/public
```

## Samba group & users
Create a user group for the private group shares:
```bash
sudo addgroup <common-group>
```
Then add an **existing system user** to the group by running the commands below:
```bash
sudo usermod -aG <common-group> <username>
```
Finally, all users who need to access a protected samba share will need to type a password. Samba doesn't use the system account password, so we need to set up a Samba password for our user account:
```bash
sudo smbpasswd -a <username>
```
Edit an **existing samba user's** password with:
```bash
sudo smbpasswd <username>
```
Finally, list all samba users:
```bash
sudo pdbedit -L -v
```

## Configure a group folder to share
If not done already (_See the Users guide for more!_): 
- Create the a common group folder.
- Set ownership of the folder to `root:common`
- Set the permissions to `0770` so that only group `common` members can read and write to it.

## Samba private shares
If not done already (_See the Users guide for more!_): 
- Create private user's (i.e jim) folder.
- Set ownership of the folder to `jim:jim`
- Set the permissions to `0770` so that only user `jim` user can read and write to it.

## Configure Samba
Make a backup copy of the configuration file and edit it:
```bash
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
  writable = yes
  create mask = 0664
  directory mask = 0775
  force user = nobody

[common]
  comment = Storage area for the few
  path = /mnt/storage/common
  writable = yes
  valid users = @commom
  create mask = 0660
  directory mask = 0770

[homes]
  comment = Home storage area of %S
  path = /mnt/storage/%S
  writable = yes
  valid users = %S
  create mask = 0660
  directory mask = 0770
```
Restart the daemon:
```bash
sudo systemctl restart smbd
```

## Access Samba

### Linux
TODO

### Windows
TODO

### macOS
Finder + Go -> Connect to Server.
Tested on macbook pro and it works fine on read/write!
Permissions as well.

### Android
Total Commander + LAN plugin.
Tested on Xiaomi A2 and it works fine on read/write!
Permissions as well.

## Firewall rules
```bash
sudo ufw allow from 192.168.1.0/24 to any port 139,445 proto tcp
sudo ufw allow from 192.168.1.0/24 to any port 137,138 proto udp
```

## Appendix A - Sources
- [smb.conf — The configuration file for the Samba suite](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)
- [Ubuntu Official Tutorials - Install and Configure Samba](https://tutorials.ubuntu.com/tutorial/install-and-configure-samba)
- [Samba Setup on Ubuntu 16.04 / 17.10 / 18.04 with Windows Systems](https://websiteforstudents.com/samba-setup-on-ubuntu-16-04-17-10-18-04-with-windows-systems/)
- [How to configure Samba Server share on Ubuntu 18.04 Bionic Beaver Linux](https://linuxconfig.org/how-to-configure-samba-server-share-on-ubuntu-18-04-bionic-beaver-linux)
- [Ubuntu Official - Samba](https://help.ubuntu.com/lts/serverguide/samba.html.en)
- [Ubuntu 18.04 - Samba shares problem fix](https://www.dedoimedo.com/computers/ubuntu-beaver-samba-shares.html)
-[Samba - What is the difference between %S, %u and %U variables in sbm.comf](https://lists.samba.org/archive/samba/2012-November/169927.html)
- [How do I define a samba share so that every user can only see its own home?](https://unix.stackexchange.com/questions/36853/how-do-i-define-a-samba-share-so-that-every-user-can-only-see-its-own-home)
- [O'Reilly Samba book - Table C.1: Variables in Alphabetic Order](https://www.oreilly.com/openbook/samba/book/appc_01.html#appc-88529)
- [Linux Mint Tutorials - Using the \[homes\] Share in Samba](https://forums.linuxmint.com/viewtopic.php?f=42&t=77063&sid=464f3d114dc81a360b841436997d9edc)
- [Configure Samba to Bind to Specific Interfaces](https://wiki.samba.org/index.php/Configure_Samba_to_Bind_to_Specific_Interfaces)
- [Samba AD DC Port Usage](https://wiki.samba.org/index.php/Samba_AD_DC_Port_Usage)

## Appendix B - Default configuration settings
Note that the following settings are set by default to:
```
browseable = yes
guest ok = no
read only = yes # Inverted synonym writable
writeable = no
```
