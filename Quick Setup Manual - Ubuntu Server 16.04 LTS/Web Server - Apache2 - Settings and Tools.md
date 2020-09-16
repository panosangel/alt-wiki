# Web Server - Apache2 - Settings and Tools #

## Basic Info ##

**Description:** Apache2 settings and tools  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**Apache version:** 2.4.18  
**Date:** 2017

## Find Installed Version ##

```sh
apache2 -v
```

## Modules compiled with ##

Finding out with which modules Apache was compiled. The `-l` option does not list the dynamically loaded modules.

```sh
apache2 -l
```

List **static** and **shared** modules.

```sh
apache2 -M
```

**Tip:** Look for Apache Multiprocessing Modules: Worker, Event (Default), Prefork

## Check for syntax errors ##

```sh
apache2ctl configtest
```

The following command will detect any syntax errors in your configuration.

```sh
apache2ctl -t
```

**Source:** [How to configure virtual hosts in Apache HTTP server](http://xmodulo.com/configure-virtual-hosts-apache-http-server.html)

## Checking Virtual Host Definitions ##

```sh
apache2ctl -S
```

**Source:** [Linode - Troubleshooting Common Apache Issues](https://www.linode.com/docs/troubleshooting/troubleshooting-common-apache-issues/)


## Configuration file for the environment variables ##

These are read when Apache is started. This is the `envvars` file, located at /etc/apache2/envvars .

**Source:** [Book] Servers for Hackers (Chris Fidao, Leanpub 2014) (Chapter Webservers)

## Apache Module mod_info ##

`info_module` provides a comprehensive overview of the server configuration.

```sh
sudo nano /etc/apache2/conf-available/info_status.conf
```

Add/edit the following lines.

```
  <Location "/server-info">
      SetHandler server-info
      # Allow access from server itself
      Require ip 127.0.0.1

      # Additionally, allow access from local workstation
      Require ip x.x.x.x
  </Location>
```

Enable the configuration file.

```
sudo a2enconf info_status
```

**Read more:** [Apache Docs - Apache Module mod_info](http://httpd.apache.org/docs/2.4/mod/mod_info.html)

## Apache Module mod_status ##

`status_module` provides information on server activity and performance.

```sh
sudo nano /etc/apache2/conf-available/info_status.conf
```

Add/edit the following lines:

```
  <Location "/server-status">
      SetHandler server-status
      Require ip 127.0.0.1

      # Additionally, allow access from local workstation
      Require ip x.x.x.x
  </Location>
```

Enable the configuration file.

```
sudo a2enconf info_status
```

**Read more:** [Apache Docs - Apache Module mod_status](http://httpd.apache.org/docs/2.4/mod/mod_status.html)

## Apache HTTP Benchmarking ##

Debian Package: apache2-utils  
RPM Package: httpd-tools

Make 20,000 connections to a web server, 200 at a time, and measure the response time.

```sh
ab -n 20000 -c 200 http://example.com
```

**Source:** [Book] Practical Linux Infrastructure (Syed Ali, Apress 2014) (Ch6 Apache for Enterprise-Level Fault Tolerance)  
**Read more:** [Apache Docs - ab - Apache HTTP server benchmarking tool](https://httpd.apache.org/docs/2.4/programs/ab.html)
