# Server Basics - LAMP Installation #

## Basic Info ##

**Description:** Complete LAMP stack installation.  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**Date:** 2017.09.24

**Source:** [DigitalOcean - How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04)  
**Read more:** [Linode - How to Install a LAMP Stack on Ubuntu 16.04](https://www.linode.com/docs/websites/lamp/install-lamp-on-ubuntu-16-04)  
**Read more:** [Linode - Hosting a Website](https://www.linode.com/docs/websites/hosting-a-website)  

## Apache ##

### Install Apache ###

```sh
sudo apt-get install apache2 apache2-utils apache2-doc libapache2-modsecurity
```

**Read more:** [Linode - Apache Tips & Tricks](https://www.linode.com/docs/web-servers/apache-tips-and-tricks/)

### Edit Apache configuration file ###

```sh
sudo nano /etc/apache2/apache2.conf
```

Edit the following:

```
# This is a placeholder
```

### Enable Modules ###

Enable the Rewrite Module

```sh
sudo a2enmod rewrite
```

Enable SSL Module

```sh
sudo a2enmod ssl
```

## MySQL ##

### Install MySQL ###

```sh
sudo apt-get install mysql-server mysql-client
```

### Fine tune the DB server ###

```sh
sudo apt-get install mysqltuner
```
**Read more:** [Ubuntu Official Documentation - MySQL](https://help.ubuntu.com/lts/serverguide/mysql.html)

### Secure DB server ###

```sh
sudo mysql_secure_installation
```

```sh
sudo mysql_install_db # Deprecated as of MySQL 5.7.6
```

## PHP ##

### Install PHP ###

```sh
sudo apt-get install php php-mysql libapache2-mod-php
sudo apt-get install php-curl php-gd php-mcrypt php-memcache php-zip php-json php-cgi php-xml php-xmlrpc php-snmp php-imap php-mbstring php-gettext
```

**Warning:** [Deprecated features in PHP 7.1.x](http://php.net/manual/de/migration71.deprecated.php)  
**Read more:** [ How to Install PHP 7.1 & 7.0 on Ubuntu 16.10, 16.04 & 14.04 using PPA](http://tecadmin.net/install-php-7-on-ubuntu/)

### Enable PHP modules ###

```sh
sudo phpenmod mcrypt
sudo phpenmod mbstring
```

**Read more:** [AskUbuntu - Install laravel 5 on Ubuntu 16.04](https://askubuntu.com/questions/764782/install-laravel-5-on-ubuntu-16-04)  

### Install phpmyadmin ###

```sh
sudo apt-get install phpmyadmin
```

**Source:** [DigitalOcean - How To Install and Secure phpMyAdmin on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04)


### Edit PHP configuration file ###

```sh
sudo nano /etc/php/7.0/apache2/php.ini
```

Edit the following:

```
memory_limit = 128M
upload_max_filesize = 25M
post_max_size = 25M
max_execution_time = 120
max_input_time = 300
file_uploads = On
```

## Tips ##

- Do not forget to restart Apache (`sudo systemctl restart apache2`)
