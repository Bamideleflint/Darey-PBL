# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS

  

In Project 8, we introduced the concept of horizontal scalability, allowing us to add new web servers to our Tooling Website. You've successfully deployed a setup with two web servers and a load balancer for traffic distribution. While configuring just a couple of servers manually is manageable, envision having to repeat this task for dozens or even hundreds of servers.

DevOps emphasizes agility and swift software and web solution releases. Automation of routine tasks is key to ensuring fast and consistent deployments.

In this project, we're starting to automate some of our routine tasks using Jenkins, a free and open-source automation server. Jenkins is among the most popular CI/CD tools and was initially known as "Hudson," created by former Sun Microsystems developer Kohsuke Kawaguchi.

Continuous integration (CI), as defined by Circle CI, is a software development approach that accelerates development speed while maintaining code quality. Developers make frequent small commits, often daily or multiple times a day. These commits are automatically built and tested before merging them into the shared repository.

In our project, we'll harness Jenkins CI capabilities to ensure that every code change made to the source code in our GitHub repository ([https://github.com/](https://github.com/)<yourname>/tooling) is automatically updated on the Tooling Website.

Additionally, for further understanding, please explore Continuous Integration, Continuous Delivery, and Continuous Deployment on your own.

### Task:

Enhance the Project 8 architecture by adding a Jenkins server and configuring a job to automate the deployment of source code changes from Git to the NFS server.

Upon completion of this project, your updated architecture will resemble the following:

![IntroToJenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/075428bd-7077-435c-88c5-13183a67cc54)  

  

#### Step 1 – Install Jenkins server

1. Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it “Jenkins”
2. Install [JDK](https://en.wikipedia.org/wiki/Java_Development_Kit) (since Jenkins is a Java-based application)

```
sudo apt update
sudo apt install default-jdk-headless
```

1. Install Jenkins

```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins
```

Make sure Jenkins is up and running

```
sudo systemctl status jenkins
```

1. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group

![Security8080](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3700f09c-6f25-43bd-9ddc-7346b2b42384) 

1. Perform initial Jenkins setup.

From your browser access `http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080`

You will be prompted to provide a default admin password

![JenkinsAdmin](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/e9362bc2-5e0b-4180-9da0-dcf6a7d57103) 

  

Retrieve it from your server:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Then you will be asked which plugins to install – choose suggested plugins.

![jenkins\_plugins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5ff86ac7-5884-43cf-a4f6-5ad0ed25557d)  

  

Once plugins installation is done – create an admin user and you will get your Jenkins server address.

The installation is completed!

![jenkins\_ready](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/e9a11528-a918-4c4b-9698-d7deed784937)  

  

#### Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks

In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will will be triggered by GitHub [webhooks](https://en.wikipedia.org/wiki/Webhook) and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

1. Enable webhooks in your GitHub repository settings

![webhook\_github](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/729a32e5-71e1-44d2-bf84-4f558f5cb4f3)  

1. Go to Jenkins web console, click “New Item” and create a “Freestyle project”

![create\_freestyle](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/25274bf1-437a-4485-98e4-2633fb001b5f)

To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself

![github\_url](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/14944049-a93e-4d1e-8e0d-eb4cb2a869a4)

In configuration of your Jenkins freestyle project choose Git repository, provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

![github\_add\_jenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/83ede878-4ee2-4809-b7bc-10447a90fde4)

Save the configuration and let us try to run the build. For now we can only do it manually.  
Click “Build Now” button, if you have configured everything correctly, the build will be successfull and you will see it under `#1`  
![jenkins\_run1](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/24998259-24c9-49ce-a194-4d2d3b0fe177)

You can open the build and check in “Console Output” if it has run successfully.

![FirstBuildConsoleOutput](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/e2f24988-c6ad-4e15-affe-ff477450519c)  

If so – congratulations! You have just made your very first Jenkins build!

But this build does not produce anything and it runs only when we trigger it manually. Let us fix it.

1. Click “Configure” your job/project and add these two configurations

Configure triggering the job from GitHub webhook:

![jenkins\_trigger](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/84991c8c-6192-4aee-88fb-0ba6f35c09c9)  

Configure “Post-build Actions” to archive all the files – files resulted from a build are called “artifacts”.  

![archive\_artifacts](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/7bd81760-8e53-421c-b515-523d8d138bab)  

Now, go ahead and make some change in any file in your GitHub repository (e.g. `README.MD` file) and push the changes to the master branch.

You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.

![GitHubEditConsoleOutput](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0eb7182c-f790-4298-ad8b-ba4181a5de42)

You have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstreadm) from another (upstream), poll GitHub periodically and others.

By default, the artifacts are stored on Jenkins server locally

```
ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/
```

####   

#### Step 3 – Configure Jenkins to copy files to NFS server via SSH

Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to `/mnt/apps` directory.

Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called [“Publish Over SSH”](https://plugins.jenkins.io/publish-over-ssh/).

1. Install “Publish Over SSH” plugin.

On main dashboard select “Manage Jenkins” and choose “Manage Plugins” menu item.

On “Available” tab search for “Publish Over SSH” plugin and install it 

![plugin\_ssh\_install](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/b4b57598-6fe9-4b52-bdd0-cf8a7241163c)

1. Configure the job/project to copy artifacts over to NFS server.

On main dashboard select “Manage Jenkins” and choose “Configure System” menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

1. Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
2. Arbitrary name
3. Hostname – can be `private IP address` of your NFS server
4. Username – `ec2-user` (since NFS server is based on EC2 with RHEL 8)
5. Remote directory – `/mnt/apps` since our Web Servers use it as a mointing point to retrieve files from the NFS server

Test the configuration and make sure the connection returns `Success`. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

![publish\_ssh\_config](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/783a10da-deb3-47fe-8bac-351ff60ecfeb)  

Save the configuration, open your Jenkins job/project configuration page and add another one “Post-build Action”  

![send\_build](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a2d5ae0c-16ea-4c7f-a30b-d5aa9fc98897) 

Configure it to send all files produced by the build into our previously define remote directory. In our case we want to copy all files and directories – so we use `**`.  
If you want to apply some particular pattern to define which files to send – [use this syntax](http://ant.apache.org/manual/dirtasks.html#patterns).  

![send\_build1](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d89fdb95-165c-4a15-87e9-698d3d27b356)  

Save this configuration and go ahead, change something in `README.MD` file in your GitHub Tooling repository.

Webhook will trigger a new job and in the “Console Output” of the job you will find something like this:

```
SSH: Transferred 25 file(s)
Finished: SUCCESS
```

![LastBuildConsoleOutput](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/24da3c5c-68c6-4f9d-895d-704c2481ebaa)

To make sure that the files in `/mnt/apps` have been updated – connect via SSH/Putty to your NFS server and check README.MD file

```
cat /mnt/apps/README.md
```

![CatReadMeOutput](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/9a58d2fe-0763-477f-8f68-b68ce6ec81f2)

If you see the changes you had previously made in your GitHub – the job works as expected.

  

### CONGRATULATIONS.
