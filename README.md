Install Apache and Allow in Firewall

sudo apt-get update
sudo apt-get install apache2

Set Global ServerName to Suppress Syntax Warnings

sudo apache2ctl configtest

Output
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK

sudo nano /etc/apache2/apache2.conf

Inside, at the bottom of the file, add a ServerName directive

ServerName 192.168.0.1

Next, check for syntax errors by typing:

sudo apache2ctl configtest

Restart Apache to implement your changes:

sudo systemctl restart apache2

or

sudo systemctl reload apache2.service


Remove mysl completely

sudo deluser mysql
sudo delgroup mysql

So in total do this:

sudo service mysql stop  #or mysqld
sudo killall -9 mysql
sudo killall -9 mysqld
sudo apt-get remove --purge mysql-server mysql-client mysql-common
sudo apt-get autoremove
sudo apt-get autoclean
sudo deluser mysql
sudo rm -rf /var/lib/mysql
sudo apt-get purge mysql-server-core-5.5
sudo apt-get purge mysql-client-core-5.5
sudo rm -rf /var/log/mysql
sudo rm -rf /etc/mysql


Install MySQL

sudo apt-get install mysql-server

Install phpMyAdmin

sudo apt-get install phpmyadmin

To use phpMyAdmin to administer a MySQL database hosted on another server, adjust the following in /etc/phpmyadmin/config.inc.php: 

$cfg['Servers'][$i]['host'] = 'db_server';

Another important configuration file is /etc/phpmyadmin/apache.conf, this file is symlinked to /etc/apache2/conf-available/phpmyadmin.conf, and, once enabled, is used to configure Apache2 to serve the phpMyAdmin site. The file contains directives for loading PHP, directory permissions, etc. From a terminal type:

sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
sudo a2enconf phpmyadmin.conf
sudo systemctl reload apache2.service

Install PHP

sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql

We want to tell our web server to prefer PHP files, so we'll make Apache look for an index.php file first.

To do this, type this command to open the dir.conf file in a text editor with root privileges:

sudo nano /etc/apache2/mods-enabled/dir.conf


<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

sudo systemctl restart apache2

sudo apt-get install php-cli

Test PHP Processing on your Web Server

sudo nano /var/www/html/info.php

This will open a blank file. We want to put the following text, which is valid PHP code, inside the file:

<?php

phpinfo();

?>

http://localhost/info.php
