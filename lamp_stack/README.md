# LAMP Stack Project

## Project Overview
This project demonstrates how to set up a fully functional LAMP (Linux, Apache, MySQL, PHP) stack and build a dynamic web application.

## What I Learned
- **Linux** server management and configuration.
- **Apache** web server setup and security.
- **MySQL** database management.
- Building dynamic web applications with **PHP**.

## Project Setup

### 1. Install Linux
I used Ubuntu Linux for this project. The first step was setting up a virtual machine and installing Ubuntu as the operating system.



## 2. Update and Upgrade System Packages

```bash
sudo apt update
sudo apt upgrade
```


### 3. Apache Setup
Apache was installed using the following command:

```bash
sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl start apache2
```

## Verify Apache2 Status

```bash
sudo systemctl status apache2
```

### 4. Install MySQL
 ```
 sudo apt install mysql-server

 sudo systemctl start mysql 

sudo systemctl enable mysql
```

## Log into mysql
```
sudo mysql
```
## Password for root user 
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

## After installation, the database was secured using:
```
sudo mysql_secure_installation
```

## Log back to console
```
exit
```

### 5. PHP Installation
```
sudo apt install php libapache2-mod-php php-mysql
```
## Verify php
```
php -v
```
## Setup Apache Configuration for Project
```
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
```
## Configure Apache Virtual Host

# Edit the Apache configuration file:
```
sudo vi  /etc/apache2/sites-available/projectlamp.conf
```
# Add the following content to the file:
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName projectlamp
    ServerAlias www.projectlamp
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
## Verify Configuration File in Sites-Available
```
Sudo ls  /etc/c/projectlamp.conf
```
# Enable new virtual host 
```
sudo a2ensite projectlamp
```
# Disable the default virtual host 
```
sudo a2dissite 000-default.conf
```
# Test the Configuration
```
sudo apache2ctl configtest
```
## Restart apache2
```
sudo systemctl restart apache2
```

## Enable php on the website
```
sudo vi  /etc/apache2/mods-enabled/dir.conf
```
# Ensure the file contains the following:
```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
## Reload apache2
```
sudo systemctl restart apache2
```
### Test PHP Setup
```
sudo vi  /var/www/html/index.php
```
## Add the following content to the file:
```
<?php
phpinfo();
?>
```
### 6. Verify PHP in Browser

## Access the file via your browser to confirm PHP is working, then remove the test file:
```
sudo rm /var/www/html/index.php
```
### 7. Linkedin post 
[My LinkedIn Post on LAMP Stack](https://www.linkedin.com/posts/abayomi-robert-onaawole_lamp-mysql-php-activity-7239876642544750592-hNNA?utm_source=share&utm_medium=member_desktop)















