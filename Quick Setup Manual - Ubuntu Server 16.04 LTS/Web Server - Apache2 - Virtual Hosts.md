# Web Server - Apache2 - Virtual Hosts #

## Basic Info ##

**Description:** Apache2 virtual hosts  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**Apache version:** 2.4.18  
**Date:** 2017.12.16  

**Source:** [DigitalOcean - How To Set Up Apache Virtual Hosts on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-16-04)  
**Source:** [Linode - Hosting a Website](https://www.linode.com/docs/websites/hosting-a-website)  
**Read more:** [Rackspace - Set up Apache virtual hosts on Ubuntu](https://support.rackspace.com/how-to/set-up-apache-virtual-hosts-on-ubuntu/)  
**Read more:** [Apache Docs - Directive Quick Reference](http://httpd.apache.org/docs/current/mod/quickreference.html)  

## Create the Directory Structure ##

```sh
sudo mkdir -p /var/www/example.com/{public_html,logs,backup}
```

**Read more:** [Stack Overflow - Why has the apache2 www dir been moved to /var/www/html?](https://stackoverflow.com/questions/21660621/why-has-the-apache2-www-dir-been-moved-to-var-www-html)  

## Grant Permissions - Ownership & Filesystem ##

Setting the correct ownership of the files and the appropriate permissions are critical to the security and the productivity.  
In this setup we have assumed that there is **only one user account** that access the files through sftp.

Verify that `/var/www` is owned by **root:root**.  

```sh
ls -alh /var/www

```

Our regular user should be able to read and write inside the project filesystem. Give the following permissions:

```sh
sudo chown -R $USER:www-data /var/www//example.com
```

```sh
sudo find /var/www/example.com -type f -exec chmod 0660 {} \;
sudo find /var/www/example.com -type d -exec chmod 2770 {} \;
```

**Read more:** [Ask Ubuntu - How to avoid using sudo when working in /var/www?](https://askubuntu.com/questions/46331/how-to-avoid-using-sudo-when-working-in-var-www)  
**Read more:** [DigitalOcean - Proper permissions for web server's directory](https://www.digitalocean.com/community/questions/proper-permissions-for-web-server-s-directory)  
**Read more:** [Cubic - Secure web server permissions that just work](http://cubicspot.blogspot.gr/2017/05/secure-web-server-permissions-that-just.html)  
**Read more:** [superuser - Ensuring new files in a directory belong to the group](https://superuser.com/questions/277775/ensuring-new-files-in-a-directory-belong-to-the-group)  

## Create a Demo Page ##

```sh
nano /var/www/example.com/public_html/index.html
```

```html
<html>
  <head>
    <title>Welcome to Example.com!</title>
  </head>
  <body>
    <h1>Success! The example.com virtual host is working!</h1>
  </body>
</html>
```

## Create New Virtual Host Files ##

Use the default vhost configuration file as a skeleton.

```sh
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf
```

Edit the newly created vhost file.

```sh
sudo nano /etc/apache2/sites-available/example.com.conf
```

```apacheconf
# domain: example.com
# public: /var/www/example.com/public_html/

<VirtualHost *:80>
    # Admin email, Server Name (domain name), and any aliases
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com

    # Index file and Document Root (where the public files are located)
    DirectoryIndex index.html index.php
    DocumentRoot /var/www/example.com/public_html

    # Log file locations
    ErrorLog /var/www/example.com/log/error.log
    CustomLog /var/www/example.com/log/access.log combined

    <Directory /var/www/example.com/public_html>

        # Default Settings
        #Options FollowSymlinks
        #AllowOverride None
        #Order Deny,Allow

        # Required for applications than provide their own .htaccess
        AllowOverride All
        Require all granted

    </Directory>

</VirtualHost>
```
**Read more:** [Linode - Apache Configuration Structure](https://www.linode.com/docs/web-servers/apache-tips-and-tricks/apache-configuration-structure)  
**Read more:** [DigitalOcean - How To Migrate your Apache Configuration from 2.2 to 2.4 Syntax](https://www.digitalocean.com/community/tutorials/migrating-your-apache-configuration-from-2-2-to-2-4-syntax)  
**Read more:** [Apache Docs - Upgrading to 2.4 from 2.2](https://httpd.apache.org/docs/2.4/upgrading.html)  
**Read more:** [Apache Docs - Apache Core Features - Options ](http://httpd.apache.org/docs/current/mod/core.html#options)  
**Read more:** [Apache Docs - Apache Core Features - AllowOverride](http://httpd.apache.org/docs/current/mod/core.html#allowoverride)  
**Read more:** [Apache Docs - Apache Module mod_access_compat - Order ](http://httpd.apache.org/docs/current/mod/mod_access_compat.html#order)  
**Read more:** [Apache Docs - Apache Module mod_access_compat - Allow](http://httpd.apache.org/docs/current/mod/mod_access_compat.html#allow)  
**Read more:** [Apache Docs - Apache Module mod_access_compat - Deny](http://httpd.apache.org/docs/current/mod/mod_access_compat.html#deny)  
**Read more:** [Apache Docs - Apache Module mod_authz_core - Require](http://httpd.apache.org/docs/current/mod/mod_authz_core.html#require)  

## Enable the New Virtual Host Files ##

```sh
sudo a2ensite example.com.conf
```

Disable the default site.

```sh
sudo a2dissite 000-default.conf
```

```sh
sudo systemctl restart apache2
```


## Set Up Local Hosts File (Optional) ##

```sh
sudo nano /etc/hosts
```

```
127.0.0.1   localhost
x.x.x.x     example.com
```

## Checking Virtual Host Definitions ##

```sh
apache2ctl -S
```

## Tips ##

-
-
-
