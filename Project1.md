# Web stack implementation (lamp stack) in aws


**Step 1: Installing Apache and updating the firewall**

1.	Launch an EC2 instance using an appropriate Linux-based AMI.
2.	Connect to the instance using SSH.
3.	Update the package manager: `sudo apt` update.

![sudo-apt-update](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5ca2452a-6d68-424a-af1c-4eea2f054a9f)

4.	Install Apache web server: `sudo apt install` apache2

![Apache-2-Default-Page](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/83c7cbc3-6118-4303-a241-b3090db9a7cb)


**Step 2: Installing MySQL**

1.	Install MySQL: `sudo apt install mysql-server`.
2.  MySQL is connected and running.

![MySql-connection](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/6675c230-df11-406f-a29a-51fc57b55794)


**Step 3: Installing PHP**

1.	Install PHP and required modules: `sudo apt install php libapache2-mod-php php-mysql`
2.  Checking PHP version `php -v`

![PHP-Version](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/82770f91-976e-4a8e-81b7-c2f03866443f)


**Step 4: Creating a virtual host for my website using Apache**

1. Create Directory `sudo mkdir /var/www/projectlamp`
2. verify Apache is running

![verifying-apache2-running](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a35537f0-0d92-4dcc-a8f4-6198255dbc8a)

*Using DNS address to check*
![website-url-with-DNS-address](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5d77f6d0-dc7d-44d4-baed-7a5794c52fc3)

*Using IP address to check*
![website-url-with-IP-address](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/8bcb777a-fc82-44cf-af3c-cbfa955f9e47)


**Step 5: Enabling and testing our web application by accessing it through a web browser.**

1. By default index.html takes precedence in the directory index over index.php, we need to edit the file to allow the index.php to take precedence with `sudo vim /etc/apache2/mods-enabled/dir.conf`

2. After saving and closing the file, we need to reload Apache so the can take effect `sudo systemctl reload apache2`

3. Create a PHP test script to confirm that Apache is able to handle and process requests for PHP files with `vim /var/www/projectlamp/index.php`

![PHP-website](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/f3663ac4-a7e4-4c63-adda-f3eefeb913e2)
