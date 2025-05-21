# WordPress Localhost Setup on Ubuntu 20.04

## 🧰 Stack
- Ubuntu 20.04
- Apache
- MySQL 8.0
- PHP
- WordPress 6.x (latest)

## 🔧 Steps

### Install LAMP Stack

$ sudo apt update
$ sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql

### Setup MySQL for WordPress

$ sudo mysql

CREATE DATABASE wp_db;
CREATE USER 'wp_rinku'@'localhost' IDENTIFIED BY '1qaz';
GRANT ALL PRIVILEGES ON wp_db.* TO 'wp_rinku'@'localhost';
FLUSH PRIVILEGES;
EXIT;

### Download WordPress

$ cd /var/www/html
$ sudo wget https://wordpress.org/latest.tar.gz
$ sudo tar -xvzf latest.tar.gz
$ sudo chown -R www-data:www-data wordpress
$ sudo chmod -R 755 wordpress

### Apache Virtual Host

$ sudo vim /etc/apache2/sites-available/wordpress.conf

  <VirtualHost *:80>
      ServerAdmin admin@localhost
      ServerName wordpress.rinku.test
      DocumentRoot /var/www/html/wordpress
      <Directory /var/www/html/wordpress>
          AllowOverride All
      </Directory>
  </VirtualHost>

$ sudo a2ensite wordpress
$ sudo systemctl reload apache2

### Add to /etc/hosts

$ vim /etc/hosts
  127.0.0.1 wordpress.rinku.test

### WordPress Setup in Browser

Go to http://wordpress.rinku.test and follow:

DB Name: wp_db
Username: wp_rinku
Password: 1qaz
Host: localhost
Table Prefix: wp_

** Paste config manually if prompted, save as wp-config.php, and ensure permissions:

$ sudo chown www-data:www-data wp-config.php
$ sudo chmod 644 wp-config.php


✅ Result

WordPress installed and accessible via http://wordpress.rinku.test.


 





