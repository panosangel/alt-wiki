# Web Server - Apache2 - Hardening Security  #

## Basic Info ##

**Description:** Hardening Apache2 web server  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**Apache version:** 2.4.18  
**Date:** 2016

**Read more:** [Apache Docs - Security Tips](http://httpd.apache.org/docs/2.4/misc/security_tips.html)

## Increase security - Limit timeout values ##

Some level of protection against DOS attacks in apache.
Timeout Value in apache tells the amount of time in seconds to wait (before closing the connection), before the server receives a message from the client.

```sh
sudo nano /etc/apache2/apache2.conf
```

```
Timeout 10
KeepAlive On
KeepAliveTimeout 15
MaxKeepAliveRequests 100
```

**Source:** [How To Secure Apache Web Server](http://www.slashroot.in/how-to-secure-apache-web-server)

## Increase security - Prevent information disclosure ##

```sh
sudo a2enmod headers
```

```sh
sudo nano /etc/apache2/conf-enabled/security.conf
```

```
ServerTokens Prod
ServerSignature Off
TraceEnable Off
Header unset Server
Header unset X-Powered-By
```

**Source:**: [Book] Servers for Hackers (Chris Fidao, Leanpub 2014) (Chapter Webservers)  
**Source:** [Quick Guide – Hardening Apache2](https://www.lastbreach.com/blog/quick-guide-hardening-apache2)

## Increase security - Exclude specific file types ##

```sh
sudo nano /etc/apache2/apache2.conf
```

```
<FilesMatch "\.(bak|BAK|conf|config|old|OLD|ORIGINAL)$">
    # Apache 2.2
    #Order allow,deny
    #Deny from all

    # Apache 2.4
    Require all denied
</Files>
```

**Source:** [GitHub - .htaccess Snippets](https://github.com/phanan/htaccess#deny-access-to-backup-and-source-files)  

## Increase security - Exclude repositories ##

Preventing access to your version control directories.
You can change that “.git” to “.svn” if you need. That rule will be applied server-wide.

```sh
sudo nano /etc/apache2/apache2.conf
```

```
<DirectoryMatch "/\.git">
    Require all denied
</DirectoryMatch>
```

**Source:**: [Book] Servers for Hackers (Chris Fidao, Leanpub 2014) (Chapter Webservers)

## Increase security - Options Directive ##

It is recommended to exclude from the `Options` directive, whenever possible the `Indexes`.

```
.
Options -Indexes
.
```

## Increase security - Fine-tune SSL settings ##

```sh
sudo nano /etc/apache2/conf-enabled/security.conf
```

Disable SSL version 2

```
SSLProtocol –ALL +SSLv3 +TLSv1
```

Disable weaker SSL ciphers

```
SSLCipherSuite HIGH:!MEDIUM:!aNULL:!MD5:!RC4
```

**Source:** [How To Secure Apache Web Server](http://www.slashroot.in/how-to-secure-apache-web-server)
