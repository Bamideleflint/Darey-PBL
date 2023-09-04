# DEVOPS TOOLING WEBSITE SOLUTION

  

As a member of a DevOps team, you will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible.  
  
In this project you will implement a solution that consists of following components:  
  
Infrastructure: AWS  
Webserver Linux: Red Hat Enterprise Linux 8  
Database Server: Ubuntu 20.04 + MySQL  
Storage Server: Red Hat Enterprise Linux 8 + NFS Server  
Programming Language: PHP  
Code Repository: GitHub  

  

On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage. Even though the NFS server might be located on a completely separate hardware – for Web Servers it look like a local file system from where they can serve the same files.  
  
![Tooling-Website-Infrastructure](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/979774e0-e9dc-4cbe-84a8-3c992c556d08)

  

It is important to know what storage solution is suitable for what use cases, for this – you need to answer following questions: what data will be stored, in what format, how this data will be accessed, by whom, from where, how frequently, etc. Base on this you will be able to choose the right storage system for your solution.

  

### STEP 1 – PREPARE NFS SERVER

Spin up a new EC2 instance with RHEL Linux 8 Operating System.  
Based on your LVM experience from Project 6, Configure LVM on the Server.  
Instead of formating the disks as ext4 you will have to format them as xfs  
Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs  
Create mount points on /mnt directory for the logical volumes as follow:  
Mount lv-apps on /mnt/apps – To be used by webservers  
Mount lv-logs on /mnt/logs – To be used by webserver logs  
Mount lv-opt on /mnt/opt – To be used by Jenkins server in Project 8  
Install NFS server, configure it to start on reboot and make sure it is u and running  

```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service

```

  

Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, you will install your all three Web Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet for higher level of security.  
To check your subnet cidr – open your EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link:  

  

Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:  

```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service

```

  

Configure access to NFS for clients within the same subnet (example of Subnet CIDR – 172.31.32.0/20 ):  

```
sudo vi /etc/exports

/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

sudo exportfs -arv

```

  

Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)  

`rpcinfo -p | grep nfs⁠`  

![NFSportsUsed](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d40c2693-270c-4745-b460-8f188c2fc8ea)

  

**Important note:** In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049  

![NFSsecurityGroup](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/1fa032a5-a9eb-4185-8254-58bdedcc1805) 

  

### STEP 2 — CONFIGURE THE DATABASE SERVER

Install MySQL server  
Create a database and name it tooling  
Create a database user and name it webaccess: `⁠create user 'webaccess'@'172.31.0.0/20' identified by 'Password.1';`  
Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr: `⁠grant all privileges on tooling.* to 'webaccess'@'172.31.0.0/20';`

`⁠flush privileges;`  

```
show databases;
use tooling;
show tables;

```

  

### Step 3 — Prepare the Web Servers

We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case – NFS Server and MySQL database.  
You already know that one DB can be accessed for reads and writes by multiple clients. For storing shared files that our Web Servers will use – we will utilize NFS and mount previously created Logical Volume lv-apps to the folder where Apache stores files to be served to the users (/var/www).  
  
This approach will make our Web Servers stateless, which means we will be able to add new ones or remove them whenever we need, and the integrity of the data (in the database and on NFS) will be preserved.  
  
**During the next steps we will do following:**  
Configure NFS client (this step must be done on all three servers)  
Deploy a Tooling application to our Web Servers into a shared NFS folder  
Configure the Web Servers to work with a single MySQL database  
1\. Launch a new EC2 instance with RHEL 8 Operating System  

1. Install NFS client

`sudo yum install nfs-utils nfs4-acl-tools -y`  

  

1. Mount /var/www/ and target the NFS server’s export for apps

```
sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www

```

  

1. Verify that NFS was mounted successfully by running `df -h`. Make sure that the changes will persist on Web Server after reboot:

  

```
sudo vi /etc/fstab
```

add following line

```
<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0
```

  

5\. Install [Remi’s repository](http://www.servermom.org/how-to-enable-remi-repo-on-centos-7-6-and-5/2790/), Apache and PHP  

```
sudo yum install httpd -y

sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo dnf module reset php

sudo dnf module enable php:remi-7.4

sudo dnf install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

sudo setsebool -P httpd_execmem 1
```

  

⁠⁠

**Repeat steps 1-5 for another 2 Web Servers.**

1. Verify that Apache files and directories are available on the Web Server in `/var/www` and also on the NFS server in `/mnt/apps`. If you see the same files – it means NFS is mounted correctly. You can try to create a new file `touch test.txt` from one server and check if the same file is accessible from other Web Servers.
2. Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №4 to make sure the mount point will persist after reboot.
3. Fork the tooling source code from [Darey.io Github Account](https://github.com/darey-io/tooling.git) to your Github account. (Learn how to fork a repo [here](https://youtu.be/f5grYMXbAV0))
4. Deploy the tooling website’s code to the Webserver. Ensure that the **html** folder from the repository is deployed to `/var/www/html`

**Note 1:** Do not forget to open TCP port 80 on the Web Server.

**Note 2:** If you encounter 403 Error – check permissions to your `/var/www/html` folder and also disable SELinux `sudo setenforce 0`  
To make this change permanent – open following config file `sudo vi /etc/sysconfig/selinux` and set `SELINUX=disabled` then restart httpd.

![ForkToolingFromGithub](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2844d9ea-1c0c-4e97-b94f-6bf7bf48f6e1)

  

1. Update the website’s configuration to connect to the database (in `/var/www/html/functions.php` file). Apply `tooling-db.sql` script to your database using this command `mysql -h <databse-private-ip> -u <db-username> -p <db-pasword> < tooling-db.sql`
2. Create in MySQL a new admin user with username: `myuser` and password: `password`:

INSERT INTO ‘users’ (‘id’, ‘username’, ‘password’, ’email’, ‘user\_type’, ‘status’) VALUES  
\-> (1, ‘myuser’, ‘5f4dcc3b5aa765d61d8327deb882cf99’, [‘user@mail.com’,](mailto:‘user@mail.com’, "mailto:‘user@mail.com’,") ‘admin’, ‘1’);

1. Open the website in your browser `http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php` and make sure you can login into the website with `myuser` user.

  
![LogIn](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/9c501d3b-7af7-453d-942b-da3ac03409c6)

![Webpage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/429c43f8-e0b2-48d5-8b08-a7362b5714c9)

  

### Congratulations!

You have just implemented a web solution for a DevOps team using LAMP stack with remote Database and NFS servers.
