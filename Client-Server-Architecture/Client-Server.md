# TASK-Implement a Client Server Architect using MySQL Database Management System (DBMS)

To demmonstarted a basic client-server using MySQL RDBS, follow the bellow instructions

__1.__ Create and configure two Linux-base virtual servers (EC2 instances in AWS)

`Server A name -  'mysql server' `

`Server B name -  'mysql client'`

![mysql_instances](images/instances.png)

__2.__ On `mysql server` install MySQL server software

```bash
sudo apt-get update
sudo apt-get install mysql-server
```
![mysql_instances](images/mysql_server_installation.png)

__3.__ On `mysql client` Linux Server install MySQL client software.

```bash
sudo apt-get update
sudo apt-get install mysql-client
```

![mysql_instances](images/install_client.png)


__4.__ Use  `mysql server's` local IP address to connect from `mysql client.` MySQL sever uses `TCP PORT 3306 ` by default. Hence, add a new in `Inbound rule` in `mysql server`

![server_SG](images/Server_SG.png)

__5.__ On mysql server, Edit the mysql configuration file to allow remote connections; replace 127.0.0.1 to 0.0.0.0

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
![server_SG](images/mysql_config.png)

__6.__ From `mysql client` Linux Server connect remotely to `mysql server` Database Engine without using ssh. You must use the mysql utility to perform this action. We do this by:

__a.__ Create a remote user (on the server hosting the database): 
Use the mysql command-line tool on the server to create a new user with appropriate permissions for the client to access the database.

```bash
mysql --version
```

- Connect to the MySQL server locally:

```bash
mysql -u root -p
```

- Create a new user:
To create a new user with remote access privileges, run the following SQL command:

```bash
CREATE USER 'remote_user'@'client_ip' IDENTIFIED BY 'strong_password';
```
- Grant privileges to the remote user:
To grant privileges to the remote user, run the following SQL command:
```bash
GRANT <permissions> ON <database>.* TO 'remote_user'@'client_ip' WITH GRANT OPTION;
```
- Flush privileges:
After creating the user and granting privileges, flush the privileges to ensure the changes take effect:
```bash
FLUSH PRIVILEGES;
```
Exit the MySQL prompt:
Type exit to `exit` the MySQL prompt.



__7.__ Connect from the MySQL Client:
On the Linux server with the MySQL client, use the `mysql` utility to connect remotely to the MySQL server database engine. Use the appropriate connection parameters as follows:

```bash
 mysql -h <remote_mysql_server_ip> -u <username> -p
```
![remote_Connection](images/remote_connection.png)

__8.__ Check that you have successfully connected to a remote MySQL server and can perform SQL queries:

```bash
Show databases;
```
![show_database](images/show_database.png)

__8.__ To ensure functionality, let's utilize the MySQL client to create and drop databases and tables, and to insert and select records from them.

- Create a Database: To create a new database. This will create a new database named "testdb". You can use the following command:
```bash
CREATE DATABASE testdb;
```
- Use the Database: After creating the database, you need to switch to it using the USE command: 
```bash
USE testdb;
```
- Create a Table: Let's create a simple table called "users" with two columns: "id" and "name".
```bash
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50)
);
```
- Insert Records: You can insert records into the "users" table using the INSERT INTO command:
 ```bash
INSERT INTO users (name) VALUES ('Ayodeole'), ('Uche'), ('Musa');
```
 - Select Records: You can retrieve records from the "users" table using the 
  SELECT command:
```bash
SELECT * FROM users;
 ```
 - Drop Table and Database (Optional): If you want to clean up, you can drop the table and the database:
  
```bash
 DROP DATABASE testdb;
```

- Exit MySQL: Finally, when you're finished, you can exit the MySQL command-line interface by typing:
  
```bash
exit
```

![database_test](images/test_database.png)

## Challenges I Faced & How I Overcame Them:

## Configuring Remote Access for MySQL 🔐
Challenge: By default, MySQL only allows local connections. I needed to enable remote access so that the client could connect to the server over the network.
Solution: I edited the MySQL configuration file on the server (/etc/mysql/mysql.conf.d/mysqld.cnf) to listen on all IP addresses by changing bind-address from 127.0.0.1 to 0.0.0.0. I also added a security group rule to allow traffic on port 3306. This opened the MySQL service to connections from the client server.

## Managing Firewall & Security Group Rules 🛡️
Challenge: Ensuring the MySQL server allowed traffic from the client’s IP while keeping it secure was tricky.
Solution: I created a specific inbound rule in AWS to allow MySQL traffic (port 3306) from the MySQL client’s private IP address only, rather than opening it to all IPs. This kept the server secure while allowing legitimate connections.

## Setting Up Remote User Access 👥
Challenge: MySQL users are usually set up to connect locally. Creating a remote user required specific permissions.
Solution: I used the GRANT command to give the client machine permission to access MySQL remotely and gave the user specific privileges for database operations. I then flushed the privileges to ensure the changes took effect immediately.

## Congratulations!
You have sucessfully deployed a fully functional MySQL Client-Server set up.
