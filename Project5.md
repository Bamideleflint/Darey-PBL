# **IMPLEMENTING A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).**

### **TASK – Implement a Client Server Architecture using MySQL Database Management System (DBMS).**

![ClientServerImage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/912dea06-5487-4434-a70d-968ed028c7fe)

The Client-Server architecture is a networking model where multiple computers are linked together over a network to exchange requests and responses. In this communication setup, each machine has a designated role: the computer initiating requests is known as the "Client," and the computer responding or serving those requests is referred to as the "Server." This architecture enables efficient and organized communication between connected devices, allowing for the seamless exchange of data and services.

![Client-server2 image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3626bdb4-8de9-41a8-af2c-eef047d06095)

In this scenario, our Web Server functions as a "Client," establishing connections to a Database (DB) Server (such as MySQL, MongoDB, Oracle, SQL Server, or others). The communication between these servers occurs over a Local Network, although it can also take place via the Internet. It is a common practice to position the Web Server and DB Server in close proximity within the local network.

The diagram above illustrates a standard, versatile Web Stack architecture that we have previously employed in previous projects (LAMP, LEMP, MEAN, MERN). Nevertheless, this architecture is adaptable and can be implemented using a wide array of technologies. It supports various Web and DB servers, accommodating everything from small Single-page applications (SPAs) to large, intricate portals.

**Step 1:** Set up two **EC2** Instances, one for the MySQL server and the other for the MySQL client.

Server A name - `MySQL-Server`
Server B name - `Client-Server`

Configure the security group for the **MySQL-Server** instance, open **port 3306** for inbound traffic from the **MySQL Client-Server** local IP address. By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate with each other using local IP addresses. Use **MySQL-Server** local IP address to connect from **MySQL Client-Server**. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.
![MySqlServer InboundRuleEdit](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c4caaa4d-0806-4267-a665-08f974b840dc)

**Step 2:** Install MySQL Server on MySQL-Server Instance (Server A)

1. Connect to the MySQL server instance using SSH.
2. Update the package list: `sudo apt update`
3. Install MySQL Server: `sudo apt install mysql-server`
4. Start MySQL service: `sudo service mysql start`
![MySqlServer ServiceEnablednRunning](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2a5c8de2-5a16-4c2b-a948-0d0e17e36850)

**Step 3:** Configure MySQL Server for Remote Connections

1. Open the MySQL configuration file: `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`
2. Find the line bind-address = `127.0.0.1` and replace it with bind-address = `0.0.0.0`
![MySqlServer BindAddress](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/6fa9cc45-dba0-4e6a-81fb-c66166d66c29)
3. Save and exit the configuration file.
4. Restart MySQL service: `sudo service mysql restart`
**On MySQL server create a user and a database**
- [ ] Access the MySQL Server: `mysql -u root -p`
- [ ] Create a Database: `CREATE DATABASE your_database_name;`
- [ ] Show Database created: `show databases;`
      
 ![ShowMyNewDatabaseCreated](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d8f3bf16-11f6-4225-a63f-5c31b7748d8b)
- [ ] Create a User: `CREATE USER 'your_username'@'localhost' IDENTIFIED BY 'your_password';`
![CreateNewUserWithIPNew](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/8f6bec79-59f6-4771-8a47-c05569242d9e)
- [ ]  Grant Privileges to the User: `GRANT ALL PRIVILEGES ON mydb.* TO 'myuser'@'localhost';`
- [ ] Flush Privileges: `FLUSH PRIVILEGES;`
- [ ] Exit MySQL: `EXIT;`

Step 4: Install MySQL Client on MySQL Client Instance (Server B)
1. Connect to the MySQL client instance using SSH.
Install MySQL Client: `sudo apt install mysql-client`
2. Connect to the MySQL Server: `mysql -u your_username -h your_mysql_server_ip -p`
3. Enter Password: After executing the command, the MySQL client will prompt you to enter the password for the user "myuser" (or the username you specified). Enter the password and press Enter.
4. Connected to MySQL Server: If the credentials are correct, you will be connected to the MySQL server, and you'll see a MySQL prompt where you can start running MySQL commands.
5. Run MySQL Commands: Now that we are connected to the MySQL server from the client, we can run MySQL commands. For example, we can show the databases, and see the database we created using the command `SHOW DATABASES;`
![ResultInClientServer](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/07ce4626-f50c-4c2b-bf1b-4768070e0ccf)
If we observe an output similar to the image above, it indicates the successful completion of this project. We have now deployed a fully functional MySQL Client-Server setup.

# **THANK YOU!**
