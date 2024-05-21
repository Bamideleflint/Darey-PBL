# Project Documentation: Automating Deployment of an E-commerce Website

## Capstone Project: CI/CD Mastery

### Project Scenario

The technology consulting firm is transitioning to a cloud architecture for its software applications. As a DevOps engineer, your objective is to design and implement a robust CI/CD pipeline using Jenkins to automate the deployment of a web application. The primary goals are continuous integration, continuous deployment, scalability, and reliability of the applications.

### Pre-requisites

- Knowledge of Jenkins essentials.
- Completion of Introduction to Jenkins, Jenkins Freestyle Project, and Jenkins Pipeline Job mini-projects.

### Project Deliverables

**Documentation:**

- Detailed documentation for each Jenkins component setup.
- Explanation of security measures implemented at each step.

### Project Components

1. **Jenkins Server Setup**

**Objective:** Configure Jenkins server for CI/CD pipeline automation.

**Steps:**
- Install Jenkins on a dedicated server (AWS).

![Installed Jenkins on AWS](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a322fd42-1dc9-4311-a09f-b0da72c67b0c)  
  

- SSH into server (Jenkins) on a terminal.

![SSH Jenkins on Terminal](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2159a5c3-c173-4832-a52a-abbe4eb88d44)
  

```
# Update package lists
sudo apt update
```
 

```
# Install Java (required for Jenkins)
sudo apt install default-jre -y
```

![Installed Java for Jenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/8f3127b6-38b3-44c4-bde9-6ce10d6631aa)

  
```
# Download and install Jenkins
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl status jenkins
```

![Jenkins is running](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/97e6e05b-2c5e-4ea4-906c-94cc9240f615) 

  
- Edit Inbound rules in the security group of the Jenkins EC2 instance

![Inbound rule 8080](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d452d259-9f68-4e4d-8dd0-cb8128e44e67)

  
- Opening port 8080 in the AWS security group's inbound rules allows incoming traffic on that port to reach the Jenkins server. Since Jenkins operates its web interface on port 8080, without this rule, external users would not be able to access Jenkins via their web browsers.

Ensuring that this port is accessible is crucial for managing Jenkins effectively, especially when setting up CI/CD pipelines and managing projects through the Jenkins dashboard.

  
- Open Jenkins in your browser (replace "your-server-ip" with your server's IP address):

```
http://your-server-ip:8080
```


![Jenkins1stPage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2234ed96-a97d-4242-bd6a-0137652f1131)

  
- Follow the on-screen instructions to complete the initial Jenkins setup.

**Get the initial password from the below file**  
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`


![JenkinsAdminPassword](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ac59dccd-2d6f-4c50-9389-c23f1c054345)  

![CustomizeJenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/908abeac-e652-4da1-97ef-3f188af6f902)  

![ReadytouseJenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/44bae905-0014-40bf-9c3b-1fccd6a431f4)


Install necessary plugins from the Jenkins dashboard (Manage Jenkins > Plugins > Available Plugins).

Set up necessary plugins (Git, Docker, etc.).

  
![DockerpluginJenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/024cbedf-f0d8-49d6-b92f-c9064177a486)  

  
2. **Source Code Management, Repository Integration**

**Objective:** Connect Jenkins to the version control system for source code management.

**Steps:**

- Integrate Jenkins with the source code management repository (e.g., GitHub, Bitbucket).
- Install Git on your Jenkins server if not already installed:

`sudo apt install git -y`
- Set up credentials in Jenkins to access your Git repository.

- Configure webhooks for automatic triggering of Jenkins build.
- In the GitHub repository settings, we should set up a webhook to trigger Jenkins builds automatically on code changes, by using the jenkins ip address and port.
- 

![SettingUpWebhook](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a5de03fc-b248-498c-ac80-78e7bd4f14bf) 
 

- On the Jenkins, go to configuration of the particular job, go to build triggers and select ‘GitHub hook trigger for GITScm polling’
  

![BuildTriggers](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ecaed535-250d-4c74-a40f-c006ad9c49ef) 
  

3. **Jenkins Freestyle Jobs for Build and Unit Tests**

**Objective:** Create Jenkins freestyle jobs for building a web application and running unit tests.

**Steps:**

- Set up a freestyle job for building the application.
- From the dashboard menu, click new item, create a freestyle job and name it.

![JenkinsFreestyleJob](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/26f0f5e4-7487-499d-978f-9e8637430f78)  

  
- Create a new GitHub repository called jenkins-scm with a README[.md](https://readme.md "https://readme.md") file

- Connect Jenkins to Source Code Management
  

![JenkinsToSourcecodeMgt](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3b3e9a20-f924-41fb-94e3-7d8d917aea6c)  

  
- Connect ‘jenkins’(JENKINS) to ‘jenkins-scm’(GitHub Repo) by pasting the repository url in the picture below, Make sure the current branch is ‘**main’** and save the configuration.

![JenkinsSCMUrl](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/7242f442-6c6c-40cd-aabb-95263bf9ac25) 

![PipelineRepoUrl](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c28e860d-638f-4693-b29e-be6150f62297) 

  
- Make changes to any file in the GitHub repository, push the changes to the main branch and there will be new build in Jenkins automatically because of the webhook settings.

![JenkinsJobRun](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/fd46345c-ff92-45fd-90bb-299368f47ffa)

              
4. **Jenkins Pipeline for Web Application**

**Objective:** Develop a Jenkins pipeline for running a web application.

**Steps:**

- Create a Jenkins pipeline script to run a web application.
- From the dashboard menu, click new item, create a pipeline job and name it.


![CreatePipelineJob](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/68c1ddbc-9670-41b3-8d99-a576b7611e54) 

  
- Configure the build trigger on Jenkins and webhook on the GitHub repository for an automatic build of the pipeline job.


![BuildTriggerforPipelineJob](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/e678a91f-3294-4225-8c11-2f03b8132e32) 
  

- Use the provided Jenkinsfile or create your own.
- The pipeline script defines and orchestrates the stages of a continuous integration and continuous delivery(CI/CD).
- The first stage is to connect to our GitHub repository, and checkout the source code using the main branch.
- The second stage is to build the Docker image with a ‘dockerfile’ in our GitHub repository named ‘jenkins-scm’.
- The third stage runs a Docker container named ‘nginx’ in detached mode (itd), the container is mapped to port 8081 on the host machine (-p 8081:80), the Docker imaged used is the one built in the second stage(dockerfile).

```
pipeline {
    agent any

    stages {
        stage('Connect To Github') {
            steps {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Bamideleflint/jenkins-scm.git']])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t dockerfile .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -itd -p 8081:80 dockerfile'
                }
            }
        }
    }
}
```

  
- Copy the pipeline script and paste like it’s shown below.

![PipelineScript](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a7177a23-fb4a-4d0c-93c8-bdb49101c5eb)

  
- Generating a syntax for our GitHub repository.
- Click on the ‘pipeline syntax’
- Click on the drop down and search for ‘checkout: check out from version control’


![CheckoutVersionControl](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/21283b3c-ff9e-49e2-b9cf-d571ba6e1327)  
  

- Paste repositry url and be sure the branch is main.


![PipelineRepoUrl](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ee067f96-46ea-4cad-ad04-90bb5e93f083)  


- Generate the pipeline script, replace script for connecting jenkins with GitHub in the stage one of our script.


![GeneratePipelineScript](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d352d9b6-2091-400a-9793-31bcb17fa6b4) 

  
5. **Docker Image Creation and Registry Push**

**Objective:** Automate the creation of Docker images for the web application and push them to a container registry such as Docker Hub.

**Steps:**

- Configure Jenkins to build Docker images.
- Install Docker on your Jenkins server if not already installed:


![dockerRunnig](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/084278d1-1f82-44f4-92d8-83d2fe1f5086)

  
- Create a file and name it [docker.sh](https://docker.sh "https://docker.sh")


![DockerShFile](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/9ea0affc-8abe-4fc9-8d4c-af179176268e)


- Open file and paste the script below

```
sudo apt-get update -y
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update -y
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo systemctl status docker
```

- Make file executable
```
chmod u+x docker.sh
```

- Execute the file
```
./docker.sh
```

- Create a new file named ‘dockerfile’

![Dockerfile](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c55b831a-76f2-4147-a2e2-b23b759719c7) 

  
- Paste code below in the file
```
# Use the official NGINX base image
FROM nginx:latest

# Set the working directory in the container
WORKDIR  /usr/share/nginx/html/

# Copy the local HTML file to the NGINX default public directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 to allow external access
EXPOSE 80
```

- Create an ‘index.html’ file and paste the content below or content of your choice

```
Congratulations, You have successfully run your first pipeline code.
Bamidele
```

Pushing these files ‘dockerfile’ and ‘index.html’ to our Jenkins-scm repo in GitHub will trigger jenkins to automatically run a new build for our pipeline.

If you don’t get the below output interface after pipeline build success, go to Manage Jenkins > Plugins > Available plugins and download ‘Pipeline Stage View’ plugin.

![PipelineJobSuccess](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d46e97a5-1872-4c40-83b6-77feb390391c)  


To access the content of ‘index.html’ on our web browser, we need to edit the inbound rules in the security group of our Jenkins server(AWS) and open port **8081** which we mapped our container to.

![Port8081](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2c62688e-db9c-43bd-91c9-70f2fbb5da7e)


We can then access the content of ‘index.html’ on our web browser. 

```
http://jenkins-ip-address:8081
```

![WebBrowserSuccess](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/38de34bb-0bf2-4b04-af58-b933aa1347ac)

  

## END OF PROJECT.
