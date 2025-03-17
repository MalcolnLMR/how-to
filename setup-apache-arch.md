# How to setup a apache server (with PHP) in arch distro

## 1. Install dependencies

```bash
sudo pacman -S php apache php-apache
```

### Configure PHP

The main PHP configuration file is located in `/etc/php/php.ini`

- (recommended) setup your timezone like so:
```date.timezone = America/Sao_Paulo```

### Configure Apache

#### (optional) Setup apache user directory
User directories are available bu default, at `http://locahost/~username/` and shows the content of `~/public_html`. This can be changed in `/etc/httpd/conf/extra/httpd-userdir.conf`

- To disable, comment the following line in `/etc/httpd/conf/httpd.conf`
```Include conf/extra/httpd-userdir.conf```

- To make it work, you must setup permissions properly, just make them executable as following:
```bash
chmod o+x ~
chmod o+x ~/public_html
chmod -R o+r ~/public_html
```

#### (optional) Setup Virtual Hosts

- First, create the folder:
```bash 
mkdir /etc/httpd/conf/vhosts
```

- Then, place single configuration files in it (name it as needed):
```
touch /etc/httpd/conf/vhosts/domain1.com
touch /etc/httpd/conf/vhosts/domain2.com
```

- At last, `Include` the configuration files in your `/etc/httpd/conf/httpd.conf` file:
```bash
# Enable Vhosts
Include conf/vhosts/domain1.com
Include conf/vhosts/domain2.com
```

A example of a vhost config file:
```
<VirtualHost *:80>
    ServerAdmin webmaster@domainname1.dom
    DocumentRoot "/home/user/http/domainname1.dom"
    ServerName domainname1.dom
    ServerAlias domainname1.dom
    ErrorLog "/var/log/httpd/domainname1.dom-error_log"
    CustomLog "/var/log/httpd/domainname1.dom-access_log" common

    <Directory "/home/user/http/domainname1.dom">
        Require all granted
    </Directory>
</VirtualHost>

<VirtualHost *:443>
    ServerAdmin webmaster@domainname1.dom
    DocumentRoot "/home/user/http/domainname1.dom"
    ServerName domainname1.dom:443
    ServerAlias domainname1.dom:443
    SSLEngine on
    SSLCertificateFile "/etc/httpd/conf/server.crt"
    SSLCertificateKeyFile "/etc/httpd/conf/server.key"
    ErrorLog "/var/log/httpd/domainname1.dom-error_log"
    CustomLog "/var/log/httpd/domainname1.dom-access_log" common

    <Directory "/home/user/http/domainname1.dom">
        Require all granted
    </Directory>
</VirtualHost>
```

#### Setup php-apache
The Apache configuration file is located in `/etc/httpd/conf/httpd.conf`

- comment this line:
```#LoadModule mpm_event_module modules/mod_mpm_event.so```

- uncomment this the line:
```LoadModule mpm_prefork_module modules/mod_mpm_prefork.so```

- put this at the end of the `LoadModule` list:
```
LoadModule php_module modules/libphp.so
AddHandler php-script .php
```

- put this at the end of the `Include` list:
```
Include conf/extra/php_module.conf
```

## 2. Enable httpd service

```bash
systemctl enable --now httpd.service
```
