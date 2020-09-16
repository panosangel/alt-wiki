# FTP Server - VSFTPD #

## Basic Info ##

**Description:** Install FTP server vsftpd  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**VSFTPD version:** 3.0.3  
**Date:** 2016.11.27

**Documentation:** [VSFTPD.CONF](http://vsftpd.beasts.org/vsftpd_conf.html)  
**Read more:** [Ubuntu Official Documentation - FTP Server](https://help.ubuntu.com/lts/serverguide/ftp-server.html)  
**Read more:** [ArchWiki - Very Secure FTP Daemon](https://wiki.archlinux.org/index.php/Very_Secure_FTP_Daemon)  
**Source:** [How To Set Up vsftpd for a User's Directory on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-vsftpd-for-a-user-s-directory-on-ubuntu-16-04)

## Install FTP Server ##

```sh
sudo apt-get install vsftpd
```

## Setup FTP user ##

Create a system user

```sh
sudo adduser --system --home /home/userftp --group userftp
```

Set password

```sh
sudo passwd userftp
```

Edit system shells and add the following line:

```sh
sudo nano /etc/shells
```

```
/usr/sbin/nologin
```

Edit user shell access according to ftp server requirements. In our case, we don't allow userftp to login to the system. It is a necessary precautionary measure along with chroot.

```sh
sudo chsh -s /usr/sbin/nologin userftp
```

## Basic server configuration ##

Backup the original configuration file

```sh
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.BAK
```

```sh
sudo nano /etc/vsftpd.conf
```

Add/edit the following settings.  

1) Changing the listen port to something else than 21 and disallowing anonymous logins are recommended.

```
listen_port=8021
anonymous_enable=NO
local_enable=YES
write_enable=YES
ftpd_banner="Welcome home aligator!".
```

2) Specify a passive port range. It's helpful when you set firewall rules.

```
pasv_min_port=40000
pasv_max_port=50000
```
## Setting the firewall ##

Check status.

```sh
sudo ufw status
```

Allow port 8021 for the FTP (listen_port), port 990 for TLS and ports 40000-50000 for the passive connections.

```sh
sudo ufw allow 8021/tcp
sudo ufw allow 990/tcp
sudo ufw allow 40000:50000/tcp
sudo ufw status
```

## Increase security - Userlist ##

```sh
sudo nano /etc/vsftpd.conf
```

Enabling userlist with the following settings disallows users that are not specifically chosen to access the FTP service.

```
userlist_enable=YES
userlist_deny=NO
userlist_file=/etc/vsftpd.userlist
```

Create /etc/vsftpd.userlist and add user "userftp"

```sh
echo "userftp" | sudo tee -a /etc/vsftpd.userlist
```

## Increase security - Enable CHROOT ##

**Before continuing be sure you understand what chroot is and how it affects the security of the system.**

We suppose we have named our user "userftp". In order to enable chroot we need to change the ownership of the home folder as follows.

```sh
sudo chown nobody:nogroup /home/userftp  
sudo chmod 555 /home/userftp  
```

Inside the home folder we can now create a new folder which is the one we can manipulate with our ftp user.

```sh
sudo mkdir /home/userftp/RandomFolder  
sudo chown userftp:userftp /home/userftp/RandomFolder  
sudo chmod 750 /home/userftp/RandomFolder  
```

```sh
sudo nano /etc/vsftpd.conf
```

Add/edit the following settings.

```
chroot_local_user=YES
allow_writeable_chroot=YES
user_sub_token=$USER
local_root=/home/$USER
```

## [Only for testing] Increase security - Enable SSL ##

Creating a new ssl certificate.

```sh
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem
sudo chmod 600 /etc/ssl/private/vsftpd.pem
```

Comment out any existing ssl certs and add our newly created certificates.

```sh
sudo nano /etc/vsftpd.conf
```

```
# rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
# rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key

rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem
ssl_enable=YES
```

Adding even more settings into the configuration file.

```
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES

ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO

require_ssl_reuse=NO
ssl_ciphers=HIGH
```

**Tip:** Do not forget to restart the service to apply the changes using `sudo service vsftpd restart`
