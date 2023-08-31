# IMPLEMENTING WEB SOLUTION WITH WORDPRESS

In this project, we will be tasked to prepare storage infrastructure on two Linux servers, and we will implement a basic web solution using WordPress. WordPress, a free and open-source content management system written in PHP, will be paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS). The project consists of two parts:

We will configure the storage subsystem for the Web and Database servers based on the Linux OS. The main focus of this part is to gain practical experience in working with disks, partitions, and volumes in Linux.

We will also install WordPress and connect it to a remote MySQL database server. This part of the project will solidify our skills in deploying the Web and DB tiers of the Web solution.

The project follows the Three-tier Architecture, a client-server software architecture pattern with 3 separate layers:  

Presentation Layer (PL): This is the user interface, such as the client-server or browser on our laptop.

Business Layer (BL): This is the backend program that implements business logic, like the Application or Webserver.

Data Access or Management Layer (DAL): This layer is responsible for computer data storage and data access, like the Database Server or File System Server (e.g., FTP server or NFS Server).

Throughout the project, we will gain hands-on experience showcasing the Three-tier Architecture. Moreover, we will ensure that the disks used to store files on the Linux servers are adequately partitioned and managed through programs like gdisk and LVM, respectively. We will work with several storage and disk management concepts.

The 3-Tier Setup for the project will be as follows:

 ![ThreeTierArchitecture](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/89f3930e-ac1f-42b9-8441-a7e5da66d3da) 

  

1. A Laptop or PC will serve as a client.
2. An EC2 Linux Server will act as a web server where we will install WordPress.
3. Another EC2 Linux server will serve as a database (DB) server.

- [ ] In previous projects, we used ‘Ubuntu’; however, it is essential to be well-versed with various Linux distributions. Therefore, for this project, we will use a very popular distribution called ‘RedHat’ (which also has a fully compatible derivative – CentOS). When connecting to the RedHat server via SSH/Putty or any other tool, we will use the 'ec2-user' user. The connection string will look like 'ec2-user@'.

  

 **SET UP AN EC2 INSTANCE TO FUNCTION AS A "WEB SERVER".**

  

Phase 1 — Preparing the Web Server

Initiate the launch of an EC2 instance designated as your "Web Server." Generate three volumes within the same Availability Zone as your Web Server EC2, each with a capacity of 10 GiB.

Successively affix all three volumes to your Web Server EC2 instance.

![AttachVolume](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d49bb005-1e20-4b9c-b880-5527e865edb2) 

  

Commence the configuration process by accessing the Linux terminal.

Utilize the "`lsblk`" command to examine the attached block devices on the server. Take note of the identifiers for your freshly established devices. 

![PartitionList](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/8f7fe421-1e36-4293-81d0-ff4c54c3cf00)

  

In Linux, all devices are located in the /dev/ directory. Confirm their presence by executing "`ls /dev/`" and verifying the existence of all three recently added block devices—likely labeled as xvdf, xvdh, and xvdg.

![lsblkVolume](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0679c6b7-5ad7-42c6-b6fa-2d4f0189a233)

  

Employ the "`df -h`" command to visualize all mounts and available space on your server.

Utilize the "gdisk" utility to craft a single partition on each of the three disks.

Execute the command "`sudo gdisk /dev/xvdf`", “`sudo gdisk /dev/xvdg`” and “`sudo gdisk /dev/xvdh`”to achieve this.

![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a24b45cd-3f18-4b3a-b2fb-02a3919216d8)  

  

Utilize the "`lsblk`" utility to observe the recently configured partition on each of the three disks.

Install the "lvm2" package using the command "`sudo yum install lvm2`." Execute "`sudo lvmdiskscan`" to examine available partitions.

Note: While Ubuntu employs the "apt" command for package installation, RedHat/CentOS uses "yum" as its package manager.

Utilize the "pvcreate" utility to designate each of the three disks as physical volumes (PVs) for use by LVM.

```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```

  

Confirm the successful creation of the physical volumes by executing "`sudo pvs`."

![PhysicalVolumeCreate](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/49cc65f6-6901-4676-98de-7e7d0f810388)  

  

Apply the "vgcreate" utility to aggregate all three PVs into a volume group (VG) named "webdata-vg."

`sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`

Confirm the successful creation of the volume group by running "`sudo vgs`."

![WebDataCreate](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/f052c250-9db4-4678-b19b-e53efb7d064b)  

  

Utilize the "lvcreate" utility to establish two logical volumes. The first, "apps-lv," should consume half of the PV size, while the second, "logs-lv," will utilize the remaining space.⁠

```
⁠⁠sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

  

Verify the successful creation of the logical volumes by executing "`sudo lvs`."  

![LVinto2](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3e37e7e8-5287-4ce4-b47b-ccb8cbec911a)  

  

Authenticate the entire setup:

```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk
```

![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/98ae8c86-7b56-4552-8cd2-481466bde6fe)  

  

Use "mkfs.ext4" to format the logical volumes with the ext4 filesystem.

```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

  

Create the "/var/www/html" directory for storing website files: `sudo mkdir -p /var/www/html`

⁠Establish the "/home/recovery/logs" directory to house log data backups: `sudo mkdir -p /home/recovery/logs`

Mount the "/var/www/html" directory on the "apps-lv" logical volume: `sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

Use the "rsync" utility to backup files from the "/var/log" directory to "/home/recovery/logs" (This is crucial before mounting the file system):

`sudo rsync -av /var/log/. /home/recovery/logs/`

Mount the "/var/log" directory on the "logs-lv" logical volume. Note that existing data in "/var/log" will be deleted (That is why create /var/www/html directory to store website files is important): `sudo mount /dev/webdata-vg/logs-lv /var/log`  

Restore the log files to the "/var/log" directory: `sudo rsync -av /home/recovery/logs/log/. /var/log`

Update the "/etc/fstab" file to ensure the mount configuration persists after server restarts. The UUID of the device will be used for this purpose: `sudo blkid`

![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d1e41046-1c9e-42a8-aa37-e85013c5791c)  

  

`sudo vi /etc/fstab`

Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.

![OwnUUID](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ddca9e75-624d-4dca-b29a-c60bef20ec89)  

  

Test the configuration and reload the daemon:

```
sudo mount -a
sudo systemctl daemon-reload
```

  

Confirm the setup by executing "`df -h`"; the output should resemble the following format.

![UUIDSetupConfirm](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2dc6a912-32e3-40d3-a31a-ca08d0df883c)  

  

### PREPARING THE DATABASE SERVER

  

Phase 2 — Setting Up the Database Server

Initiate the launch of another RedHat EC2 instance, which will play the role of the 'DB Server.'

Repeat the identical steps applied to the Web Server. However, instead of creating an 'apps-lv' logical volume, establish a 'db-lv' volume and mount it to the '/db' directory in place of '/var/www/html/'.

  

Phase 3 — Installing WordPress on Your Web Server EC2  

Update the repository: `sudo yum -y update`

Install wget, Apache, and their dependencies: `sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`

Start Apache and enable it to launch on startup: 

```
sudo systemctl enable httpd
sudo systemctl start httpd
```

  

For installing PHP and its dependencies, proceed with the following commands:

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo setsebool -P httpd_execmem 1
```

  

Restart Apache to apply the changes: `sudo systemctl restart httpd`

![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/e9028324-c9ed-461e-bc23-b339078603af)  

  

Download WordPress and copy it to the '/var/www/html' directory:

```
mkdir wordpress
cd wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php
cp -R wordpress /var/www/html/
```

  

Configure SELinux Policies:  

```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```

  

Phase 4 — Installing MySQL on Your DB Server EC2  

```
sudo yum update
sudo yum install mysql-server
```

  

Ensure the MySQL service is operational and start it at boot:

```
sudo systemctl status mysqld # verify the service status
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

![MySQLRunningDBServer](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/04056547-cc3a-4adb-b0c0-21f3d34eee62)  

  

Phase 5 — Configuring the Database for WordPress

```
sudo mysql
```

  

Within the MySQL shell, execute the following commands:

```
CREATE DATABASE wordpress;
CREATE USER 'myuser'@'Web-Server-Private-IP-Address' IDENTIFIED BY 'mypass';
GRANT ALL ON wordpress.* TO 'myuser'@'Web-Server-Private-IP-Address';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```

![ShowDatabase](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/56df72f9-ca1c-4090-acd8-4b2f26568d01)  

  

Phase 6 — Configuring WordPress to Connect to Remote Database

Note: Remember to open MySQL port 3306 on the DB Server EC2. For added security, allow access to the DB server solely from your Web Server's IP address, specifying the source as /32 in the Inbound Rule configuration.

![MySqlInboundRule](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ec7eb9cd-fcc9-4539-9407-069504d97b98)  

  

Install the MySQL client and verify connectivity from the Web Server to the DB server:

```
sudo yum install mysql
sudo mysql -u admin -p -h DB-Server-Private-IP-address
```

  

Confirm successful execution of the "`SHOW DATABASES;`" command, displaying the list of existing databases.

![ShowDatabaseOnMyWebServer](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0a07e46f-e7d4-44c3-bc6f-9ba11764cb52)  

Adjust permissions and configuration to enable Apache's usage of WordPress.  
Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (either allow from everywhere 0.0.0.0/0 or your workstation's IP).  
Access your WordPress link in your browser, typically [http://Web-Server-Public-IP-Address/wordpress/.](http://Web-Server-Public-IP-Address/wordpress/.)  

![wordpress1](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0ecabeb0-ede5-459f-a786-0f4bc2e52dec)  

Fill in your DB credentials.

![wordpresswelcome](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/6d20b182-9986-4fd5-8e8c-65d93e71c0f6)  

Upon encountering the message in the picture below, it signifies that your WordPress has successfully established a connection with your remote MySQL database.

![WordpressFullPage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/b803e153-3969-466e-996e-de9752960faa)




# **The End**
