# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

  

By now, we've gained an understanding of Load Balancing and have successfully set up an LB solution using Apache. However, as DevOps professionals, versatility is key, and it's important to be familiar with various alternative solutions for the same challenges. That's why, in this project, we'll take the next step and configure an Nginx Load Balancer solution.

Additionally, ensuring secure connections to our web solutions and encrypting information in transit is of paramount importance. In this project, we will also delve into securing connections over HTTPS (HTTP Secure) protocol, understanding its purpose, and the necessary steps to implement it.

When data traverses the Internet between a client's browser and a web server, it passes through multiple network devices. If this data is not encrypted, it becomes vulnerable to interception by malicious actors who have access to the intermediate equipment. This kind of security threat is commonly referred to as a Man-In-The-Middle (MITM) attack.

The threat of MITM attacks is very real. Users who share sensitive information like bank details or social media access credentials over non-secured channels are at risk of having their data compromised and exploited by cybercriminals.

SSL (Secure Sockets Layer) and its successor, TLS (Transport Layer Security), are security technologies that protect against MITM attacks by establishing an encrypted session between the browser and the web server. Throughout this project, we'll collectively refer to these cryptographic protocols as SSL/TLS, even though SSL has largely been replaced by TLS, the term is still widely used.

SSL/TLS relies on digital certificates to identify and validate a website. A browser reads the certificate issued by a Certificate Authority (CA) to ensure that the website is registered with the CA, making it trustworthy for establishing a secure connection.

There are various types of SSL/TLS certificates, and you can find more information about them here. To better understand how SSL works, you can refer to this tutorial or explore additional resources here.

In this project, one of our primary objectives is to register your website with the LetsEncrypt Certificate Authority. To automate the certificate issuance process, we will use a shell client recommended by LetsEncrypt called certbot.

This project consists of two main parts:

1. Configuring Nginx as a Load Balancer.
2. Registering a new domain name and configuring a secure connection using SSL/TLS certificates.

The architecture we aim to achieve will resemble the following: ![LOAD BALANCER SOLUTION WITH NGINX AND SSL TLS](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/b687dd28-0989-4d48-a2c2-55b5f0a72265)

  

## CONFIGURE NGINX AS A LOAD BALANCER

1. Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it `Nginx LB` (do not forget to open TCP port 80 for HTTP connections, also open TCP port **443** – this port is used for secured HTTPS connections)

![InboundRule](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/61a05fa8-f625-47ab-95c2-349d04d47f89)  

1. Update `/etc/hosts` file for local DNS with Web Servers’ names (e.g. `Web1` and `Web2`) and their local IP addresses

![etchostFile](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/cc7a4161-ef7e-4f35-ad0c-510cfa939d58) 

1. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

Update the instance and Install Nginx

```
sudo apt update
sudo apt install nginx
```

Configure Nginx LB using Web Servers’ names defined in `/etc/hosts`

Open the default nginx configuration file

`sudo vi /etc/nginx/nginx.conf`

```
#insert following configuration into http section

 upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }

server {
    listen 80;
    server_name www.domain.com;
    location / {
      proxy_pass http://myproject;
    }
  }

#comment out this line
#       include /etc/nginx/sites-enabled/*;

```

Restart Nginx and make sure the service is up and running

```
sudo systemctl restart nginx
sudo systemctl status nginx
```

  

### REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

Let us make necessary configurations to make connections to our Tooling Web Solution secured!

In order to get a valid SSL certificate – you need to register a new domain name, you can do it using any [Domain name registrar](https://en.wikipedia.org/wiki/Domain_name_registrar) – a company that manages reservation of domain names. The most popular ones are: [Godaddy.com](https://godaddy.com/), [Domain.com](https://www.domain.com/), [Bluehost.com](https://www.bluehost.com/).

1. Register a new domain name with any registrar of your choice in any domain zone (e.g. `.com`, `.net`, `.org`, `.edu`, `.info`, `.xyz` or any other)

![SiteName](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c35a88c3-bec2-45b5-90ce-e9e1068320de)

![CustomDNS](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ee70ffee-e60d-4497-947e-52160d1259d3)  

1. Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP

You might have noticed, that every time you restart or stop/start your EC2 instance – you get a new public IP address. When you want to associate your domain name – it is better to have a static IP address that does not change after reboot. Elastic IP is the solution for this problem, learn how to allocate an Elastic IP and associate it with an EC2 server [on this page](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html).

![ElasticIP](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/07da7663-2343-46d0-81b7-5157a25ad568)  

1. Update [A record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/) in your registrar to point to Nginx LB using Elastic IP address

Learn how to associate your domain name to your Elastic IP on [this page](https://medium.com/progress-on-ios-development/connecting-an-ec2-instance-with-a-godaddy-domain-e74ff190c233).

**Side Self Study:** Read about different [DNS record types](https://www.cloudflare.com/learning/dns/dns-records/) and learn what they are used for.

Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – `http://<your-domain-name.com>`

1. Configure Nginx to recognize your new domain name

Update your `nginx.conf` with `server_name www.<your-domain-name.com>` instead of `server_name www.domain.com`

1. Install [certbot](https://certbot.eff.org/) and request for an SSL/TLS certificate

Make sure [`snapd`](https://snapcraft.io/snapd) service is active and running

```
sudo systemctl status snapd
```

Install certbot

```
sudo snap install --classic certbot
```

Request your certificate (just follow the certbot instructions – you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from `nginx.conf` file so make sure you have updated it on step 4).

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
```

Test secured access to your Web Solution by trying to reach `https://<your-domain-name.com>`

You shall be able to access your website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string.  
Click on the padlock icon and you can see the details of the certificate issued for your website.     

1. Set up periodical renewal of your SSL/TLS certificate

By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

You can test renewal command in `dry-run` mode

```
sudo certbot renew --dry-run
```

Best practice is to have a scheduled job that to run `renew` command periodically. Let us configure a `cronjob` to run the command twice a day.

To do so, lets edit the crontab file with the following command:

```
crontab -e
```

Add following line:

```
* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
```

You can always change the interval of this cronjob if twice a day is too often by adjusting schedule expression.

  

### Congratulations!

You have just implemented an Nginx Load Balancing Web Solution with secured HTTPS connection with periodically updated SSL/TLS certificates.
