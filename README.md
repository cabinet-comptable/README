# Facnote

Fork the Facnote repository
--------------------
go to https://github.com/cabinetcomptable/Facnote.git
![fork button](https://help.github.com/assets/images/help/repository/fork_button.jpg)

Clone Git repository
--------------------
```
#configuration git locale.
# git config --global user.email "you@example.com"
# git config --global user.name "Your Name"
# cd /var/www
# gti init
# git remote add upstream https://github.com/cabinetcomptable/Facnote.git
# git remote add orign https://github.com/[YOUR-GITHUB-USERNAME]/Facnote.git
# git clone https://github.com/[YOUR-GITHUB-USERNAME]/Facnote.git
# git checkout develop
# git fetch
# gti pull orign develop
```
##The .git/config file Exemple
```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
[user]
	email = you@example.com
[remote "upstream"]
	url = https://github.com/cabinetcomptable/facnote.git
	fetch = +refs/heads/*:refs/remotes/upstream/*
[remote "origin"]
	url = https://github.com/[YOUR-GITHUB-USERNAME]/facnote.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

Syncing the fork
--------------------
Sync a fork of a repository to keep it up-to-date with the upstream repository.
https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/syncing-a-fork

Requirements
------------
* PHP >= 7.2
* php7.2-fpm.service - The PHP 7.2 FastCGI Process Manager
* Symfony 3.4
* Web Server Recommended :
     - Nginx
     - Apache with required/recommended modules:
    ```
    - mod_rewrite
    - mod_env
    - mod_setenvif
    - mod_expires
    ```
     - Varnish 5.1 with varnish-modules
     - Redis
* Database server (MySQL 5.5+ or MariaDB 10.0+)
* Composer
* Curl
* Git (for development)
* PHP extensions/modules :
```
- php-common
- php-cli
- php-fpm
- php-mysql
- php-xml
- php-mbstring
- php-intl
- php-curl
- php-json
- php-readline
- php-zip
- php-iconv
- php-xdebug (for development)
- php-imagick
```
System requirements  
--------------------
```
# sudo php bin/symfony_requirements 
# sudo apt-get install poppler-utils
```

PHP-FPM Configurations
--------------------
```
# ln -s /var/www/config/dev/fpm_facnote.conf /etc/php/7.2/fpm/pool.d/facnote.conf
# service php7.2-fpm restart
```
Nginx Configurations 
--------------------
```
# rm -rf /etc/nginx/sites-enabled/default
# ln -s /var/www/config/dev/facnote-ssl.conf /etc/nginx/sites-enabled/facnote.conf
# service nginx restart
```
Apache Configurations
--------------------
```
# rm -rf /etc/apache2/sites-enabled/000-default.conf
# echo > /etc/apache2/ports.conf
# ln -s /var/www/conf/dev/facnote.conf /etc/apache2/sites-enabled/facnote.conf
# a2enmod proxy_fcgi
# a2enmod mod_expires
# a2enmod expires
# a2enmod rewrite
# service apache2 restart

```
Varnish Configurations
------------------
```
# mv /etc/varnish/default.vcl /etc/varnish/default.vcl_old
# mv /etc/default/varnish /etc/default/varnish_old
# ln -s /var/www/conf/inte/facnote.vcl /etc/varnish/default.vcl
# ln -s /var/www/conf/dev/varnish /etc/default/varnish
```
For Debian (v8+) / Ubuntu (v15.04+)
--------------------
```  
##Change the Vranish port :6081 by :81
# vi /etc/systemd/system/varnish.service
# systemctl daemon-reload
# service varnish restart
```
Installe le package "varnish-modules" pour importé les libs "std" et "xkey" dans le ficher vcl.
--------------------
```
# sudo apt-get update
# sudo apt-get install varnish-modules
# service varnish restart
```
Mysql Configurations
--------------------
```
# mysql > CREATE USER 'facnote'@'localhost' IDENTIFIED BY 'FacNote2018!';
# mysql > CREATE USER 'facnote'@'%' IDENTIFIED BY 'FacNote2018!';
# mysql > GRANT ALL PRIVILEGES ON facnote_v3.* TO 'facnote'@'%';
# mysql > GRANT ALL PRIVILEGES ON facnote_v3.* TO 'facnote'@'localhost';
```

Install composer
--------------------
```
# sudo curl -s https://getcomposer.org/installer | php
# sudo mv composer.phar /usr/local/bin/composer
```

Install project dependencies
--------------------
```
# php -d memory_limit=-1 /usr/local/bin/composer install --no-interaction --ignore-platform-reqs
# sudo -u www-data php -d memory_limit=-1 bin/console c:c --env=dev
```

Fixing File permission
--------------------
```
# chown -R root:www-data /var/www/facnote
# chown -R www-data:www-data /var/www/facnote/var /var/www/facnote/web
# find /var/www/facnote -type d -print0 | xargs -0 chmod 2750 || true
# find /var/www/facnote -type f -print0 | xargs -0 chmod 0754 || true
# find /var/www/upload /var/www/facnote/var/cache /var/www/facnote/var/logs /var/www/facnote/var/sessions /var/www/facnote/web/ -type d -print0 | xargs -0 chmod 2770 || true
# find /var/www/upload /var/www/facnote/var/cache /var/www/facnote/var/logs /var/www/facnote/var/sessions -type f -print0 | xargs -0 chmod 0660 || true
# chmod 2770 /var/www/facnote
```

Install Wkhtmltopdf v0.12.4
--------------------
```
# apt-get update 
# apt-get install -y --force-yes xvfb 
# wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz 
# tar xvf wkhtmltox-0.12.4_linux-generic-amd64.tar.xz 
# mv wkhtmltox/bin/wkhtmltopdf /usr/bin 
# rm wkhtmltox-0.12.4_linux-generic-amd64.tar.xz  && rm -rf wkhtmltox 
```
