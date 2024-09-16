## WEB STACK IMPLEMENTATION (LEMP STACK) IN AWS

### Introduction:

**LEMP** is an open-source web application stack used to develop web applications. The term LEMP is an acronym that represents L for the Linux Operating system, Nginx (pronounced as engine-x, hence the E in the acronym) web server, M for MySQL database, and P for PHP scripting language.

LEMP Stands For:
- L- Linux Operating System
- E- Nginx Server
- M- MySQL Database
- P- PHP

LEMP benefits from strong community support and is employed globally in numerous highly-scaled web applications. Nginx ranks as the second most utilized web server globally, trailing only Apache in popularity.

The LEMP stack is a combination of four open-source technologies that are used in web development. These technologies include:

- Linux: The operating system that runs the web server.
- Nginx: The web server software that handles HTTP requests.
- MySQL: The relational database management system that stores the website’s data.
- PHP: The programming language used to build dynamic web applications.


### How does the LEMP Stack work?

The LEMP stack operates by employing Nginx as its web server, responsible for receiving HTTP requests and directing them to the relevant PHP script. These PHP scripts generate responses, which are subsequently delivered back to the user through Nginx.

MySQL serves as the database management system for storing and organizing the website's data. PHP interacts with MySQL to fetch and store data as required.

### Why is the LEMP Stack popular in web development?

The LEMP stack garners popularity in web development for several compelling reasons:

- Exceptional Performance: Nginx stands out for its remarkable performance, rendering it an optimal solution for managing extensive traffic loads effectively.
  
-  Scalability: Renowned for its scalability, the LEMP stack proves advantageous for websites requiring robust handling of substantial visitor traffic.
  
-  Open-Source Nature: Each component of the LEMP stack is open-source, rendering it a cost-effective solution for web developers.
  
-  Adaptability: Offering flexibility, the LEMP stack can be tailored to suit the specific requirements of any given website.
  
- Enhanced Security: Renowned for its robust security measures, the LEMP stack, bolstered by Nginx, incorporates various security features such as SSL encryption and DDoS protection.

## Step 0: Prerequisites

__1.__ EC2 Instance of t2.micro type and Ubuntu 24.04 LTS (HVM) was lunched in the region of my choosing using the AWS console.

![Lunch Instance](/img/lemp_in.png)
![Lunch Instance](/img/ec2_instance_details.png)

__2.__ Created SSH key pair named __lempServer__ to access the instance on port 22.

![Lunch Instance](/img/Lemp-Key-pair.png)

__3.__ The security group was configured with the following inbound rules:

- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.
- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.

![Security Rules](/img/securitygroup.png)

__4.__ The default VPC and Subnet was used for the networking configuration.

![EC2 Network](/img/EC2_network.png)

__5.__ Launched Git Bash and ran the following command:
```
ssh -i Lemp-Server.pem ubuntu@54.172.190.153

```
__6.__ Permission Denied.The warning about the permissions of the private key file ****(Lemp-Server.pem)**** indicates that its permissions are too open. SSH requires strict permissions for private key files for security reasons. 

![Permission Denied](/img/Permission%20denied.png)

__7.__ To fix this, you need to change the permissions of the private key file. Run the following command in your terminal:

```
chmod 600 Lemp-Server.pem
```
![Permission change](/img/permission_change.png)

__8.__ After applying these fixes, try SSH-ing into the server again.

```
ssh -i Lemp-Server.pem ubuntu@54.172.190.153
```

![Launch Git Bash](/img/git_bash_launch.png)

## Step 1 - Install the Nginx Web Server

__1.__ __Update server's package index__
```
sudo apt update
sudo apt install nginx
```

![nginx_update](/img/nginx_update.png)


__2.__ __Install server's package index__

![nginx_launch](/img/nginx_install.png)

__3.__ __Verify that nginx was successfuly installed__
```
sudo systemctl status nginx
```
![nginx_status_check](/img/nginx_status.png)

**Note:** If it's green and running, you've followed all the steps correctly. Congratulations, you've just launched your first web server in the cloud!

__4.__ __The server is running and can be accessed locally in the ubuntu shell by running the command below:__

```
curl http://localhost:80
OR
curl http://127.0.0.1:80
```
![Local URL](/img/default_curl_page.png)

__5.__ __Test whether the Nginx server can respond to requests from the internet by testing it with the public IP address using the URL in a web browser.__
```
http://54.172.190.153:80
```
![Nginx Default Page](/img/nginx_default_broswer_page.png)

This indicates that the web server is properly installed and accessible through the firewall.

__6.__ __An alternative method to obtain the public IP address is by means other than checking the AWS console.__
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
After running the command above, there was an error __401 - Unauthorized__ output.
![Unauthorized Error](/img/unathorized_curl.png)
In troubleshooting this error, the following navigation was made from the ec2 instance page on the AWS console:

- Actions > Instance Settings > Modify instance metadata options.
- Then change the __IMDSv2__ from __Required__ to __Optional__.
![imds option](/img/imd_optional.png)

The command was run again, this time there was no error with the public IP address displayed.

```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
![public ip with curl](/img/public_ip_curl.png)

## Step 2 - Install MySQL

__1.__ __Install a relational database (RDB)__

MySQL was installed in this project. It is a popular relational database management system used within PHP environments.
```
sudo apt install mysql-server
```
![Install MySQL](/img/install_mysql.png)
When prompted, install was confirmed by typing y and then Enter.

__2.__ __Enable and verify that mysql is running with the commands below__
```
sudo systemctl enable --now mysql
sudo systemctl status mysql
```
![MySQL_Status_Check](/img/mysql_status.png)

__3.__ __Log in to mysql console__
```
sudo mysql
```
This connects to the MySQL server as the administrative database user __root__ infered by the use of __sudo__ when running the command.

![mysql_login](/img/mysql_login.png)

__4.__ __Set a password for root user using mysql_native_password as default authentication method.__

Here, the user's password was defined as "PassWord.1"
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
![User Password](/img/mysql_login_password.png)

Exit the MySQL shell
```
exit
```
__5.__ __Run an Interactive script to secure MySQL__

The security script is pre-installed with MySQL. It's designed to eliminate certain insecure settings and restrict access to the database system.
```
sudo mysql_secure_installation
```
![mysql_secure_installation](/img/mysql_secure.png)

Regardless of whether the **VALIDATION PASSWORD PLUGIN** is configured, the server will prompt you to choose and confirm a password for the MySQL root user.

__6.__ __After changing root user password, log in to MySQL console.__

After executing the command below, a command prompt for entering a password was observed.
```
sudo mysql -p
```
![mysql_password_test](/img/mysql_password_access.png)

Exit MySQL shell
```
exit
```

## Step 3 - Install PHP

__1.__ __Install php__

Sure, here's a rewrite:

PHP is the element of the setup responsible for processing code to present dynamic content from the web server to the end user.

The following were installed:
- php package
- php-fpm, which stands for "PHP fastCGI processing manager", and tell Nginx to pass PHP requests to this software for processing.
- php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
```
sudo apt install php-fpm php-mysql
```
![mysql_password_test](/img/php_installation.png)

Confirm the PHP version
```
php -v
```
![Confirm php version](/img/php_installation_confirmation.png)

## Step 4 - Configuring Nginx to Use PHP Processor

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more thab one domain on a single server. In this documentatio, I'll use **projectLEMP** as an example domain name.

The default directory serving the Nginx default page is /var/www/html. Create your document directory next to the default one.

Created the root web directory for **your_domain** **** using "mkdir" command
```
sudo mkdir /var/www/projectLEMP
```
![root_directory](/img/projectLEMP_root_directory.png)

__2.__ Then assign ownership of the directory with the $USER environment  variable, which will reference the current systen user 
```
sudo chown -R $USER:$USER /var/www/projectLEMP
```
![user_ownership](/img/ownership_$user.png)

__3.__ Open a new configuration file in Nginx's ***sites-avilable*** directory using ypour preffered command-line editor. Here, I'll use ***nano*** 
```
sudo nano /etc/nginx/sites-available/projectLEMP
```
![blankpage](/img/blankpage.png)

his will create a blank file> Paste in the following bare-bone configuration:  
```
# /etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```
![blankpage](/img/bare_bone_configuration.png)

**Note:** After making changes in the editor, you can save the file by pressing Ctrl + O, then press Enter to confirm the filename, and finally Ctrl + X to exit nano.

__3.__ Activate your configuration by linking to the config file from Nginx’s ***sites-enabled*** directory:
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
**Note:** The purpose of this command is to enable the server configuration file for your domain by creating a symbolic link to it in the sites-enabled directory. 

__4.__ Test your configuration for syntax errors by typing: 
```
sudo nginx -t
```
![Test Syntax Error](/img/nginx.conf_syntax.png)

__5.__ Disable default Nginx host that is currently configured to listen on port 80, for this run:
```
sudo unlink /etc/nginx/sites-enabled/default
```
__6.__ Reload Nginx to apply changes:
```
sudo systemctl reload nginx
```
![Test Syntax Error](/img/reload_nginx.png)

__7.__ Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:
```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```
![index html page](/img/index.html_page.png)

__8.__  Now go to your browser and access your server’s domain name or IP address, as listed within the server_name directive in your server block configuration file:

```
http://204.236.192.94 

or 

http://ec2-54-172-190-153.compute-1.amazonaws.com:80
```
![broswer home](/img/browser_home.png)


## Step 5 –Testing PHP with Nginx

Your LEMP stack setup is now complete. You can verify its functionality by testing whether Nginx properly passes .php files to your PHP processor.

You can do this by creating a test PHP file in your document root.

__1.__ Open a new file called ***info.php*** within your document root in your text editor:

```
vi /var/www/projectLEMP/info.php
```
Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:

```
<?php
phpinfo();
```
![php_index](/img/projectLEMP_index.html_info.php.png)

__2.__You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:

```
http://54.172.190.153/info.php
or 
serverdomain/info.php
```
![php_homepage](/img/php_home_page.png)

__3.__ After reviewing the pertinent details of your PHP server via that page, it's advisable to delete the file you generated since it harbors sensitive information regarding your PHP setup and your Ubuntu server. To accomplish this, you can utilize the `rm` command to delete the file.

```
sudo rm /var/www/projectLEMP/info.php
```
![php_homepage](/img/php_rm.png)
## Step 6 — Retrieving data from MySQL database with PHP

__1.__ First, connect to the MySQL console using the root account:

```
sudo mysql -u root -p
```
![mysql_database](/img/mysql_database.png)

__2.__ To create a new database, run the following command from your MySQL console:

```
mysql> CREATE DATABASE steghub_DB;
```
![mysql_database](/img/create_database-steghub.png)

__2.__The following command creates a new user named ***example_user***, using mysql_native_password as default authentication method. We’re defining this user’s password as password, but you should replace this value with a secure password of your own choosing.

```
mysql> CREATE USER 'fiyo_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
![mysql_database](/img/create_mysql_user.png)

```
mysql> GRANT ALL ON steghub_DB.* TO 'fiyo_user'@'%';
```
![mysql_database](/img/mysql_grant_all.png)

__3.__  Exit the MySQL shell with:

```
mysql> exit
```

__4.__  You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

```
mysql -u fiyo_user -p
```
![mysql_database](/img/fiyo_user.png)

__5.__ Confirm that you have access to the steghub_DB database:
```
mysql> SHOW DATABASES;
```
![show_databases](/img/show_databases.png)

__6.__ 1’ll create a test table named todo_list. From the MySQL console, run the following statement:

```
mysql> CREATE TABLE steghub_DB.todo_list (
mysql> item_id INT AUTO_INCREMENT,
mysql> content VARCHAR(255),
mysql> PRIMARY KEY(item_id)
);
```
![create to-do-list](/img/create_to_do_list.png)

__7.__ Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different values:
```
mysql> INSERT INTO steghub_DB.todo_list (content) VALUES ("My first important item");
mysql> INSERT INTO steghub_DB.todo_list (content) VALUES ("My second important item");
mysql> INSERT INTO steghub_DB.todo_list (content) VALUES ("My third important item");
mysql> INSERT INTO steghub_DB.todo_list (content) VALUES ("My fourth important item");
```
![insert_into_Steghub_DB](/img/insert_into_Steghub_DB.png)

__8.__To confirm that the data was successfully saved to your table, run:

```
mysql> SELECT * FROM steghub_DB.todo_list;
```

```bash
Output
+---------+--------------------------+
| item_id | content                  |
+---------+--------------------------+
|       1 | My first important item  |
|       2 | My second important item |
|       3 | My third important item  |
|       4 | My fourth important item |
+---------+--------------------------+
4 rows in set (0.00 sec)
```
![confirm](/img/confirm_todolist.png)

__9.__ After confirming that you have valid data in your test table, you can exit the MySQL console:
```
mysql> exit
```

Now you can create the PHP script that will connect to MySQL and query for your content.

__10.__ Create a new PHP file in your custom web root directory using your preferred editor. We’ll use nano for that:

```bash
nano /var/www/projectLEMP/todo_list.php
```
The following PHP script connects to the MySQL database and queries for the content of the todo_list table, exhibiting the results in a list. If there’s a problem with the database connection, it will throw an exception.

12. Copy this content into your todo_list.php script:

```bash
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>"; 
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```

_Save and close the file when you’re done editing._

__11.__ You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:

```
http://54.172.190.153/todo_list.php
```

![php](/img/php_todolist.png)

#### That means your PHP environment is ready to connect and interact with your MySQL server

## Conclusion

We've established a versatile base for delivering PHP websites and applications to your audience, leveraging Nginx as the web server and MySQL as the database system.<br><br>

# Thank You