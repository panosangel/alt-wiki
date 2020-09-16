# Web Server - Apache2 - Multi-Processing Modules (MPMs) #

## Basic Info ##

**Description:** Complete LAMP installation.  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**Date:** 2017.08.19

## Theory ##

Switching to a multi-processing module (MPM) will take into advantage the modern hardware and will favorite performance.  

Apache 2.4 offers various multi-processing modules (MPMs) to handle connections. The default MPM is the event module, although the prefork module is still recommended if you’re using standard PHP. [Source](https://www.linode.com/docs/web-servers/apache/apache-web-server-debian-8)  

**Tip:** Only one mpm module can be enabled at any time.

**Read more:** [Apache2 Docs - Multi-Processing Modules (MPMs)](http://httpd.apache.org/docs/current/mpm.html)  
**Read more:** [serverfault - How do I select which Apache MPM to use?](https://serverfault.com/questions/383526/how-do-i-select-which-apache-mpm-to-use)  

## Enable Prefork MPM ##

```sh
sudo a2enmod mpm_prefork
```

Edit the following file accordingly:

```sh
sudo nano /etc/apache2/mods-available/mpm_prefork.conf
```

## Enable Event MPM ##

```sh
sudo a2enmod mpm_event
```

Edit the following file accordingly:

```sh
sudo nano /etc/apache2/mods-available/mpm_event.conf
```

**Read more:** [Apache2 Docs - Apache MPM Common Directives](http://httpd.apache.org/docs/current/mod/mpm_common.html)  
**Read more:** [Linode - Apache Web Server on Debian 8 (Jessie)](https://www.linode.com/docs/web-servers/apache/apache-web-server-debian-8)
