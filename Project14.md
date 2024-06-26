## EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP

  

This project has some initial theoretical concepts that must be well understood before moving on to the practical part. Please read it carefully as many times as needed to completely digest the most important and fundamental DevOps concepts. To successfully implement this project, it is crucial to grasp the importance of the entire CI/CD process, roles of each tool and success metrics

In previous projects, we have been deploying a tooling website directly into the MARKDOWN\_HASH directory on dev servers. Well, even though that worked fine, and we were able to access the website, it is not the best way to do it. Real world web application code written on [Java](https://en.wikipedia.org/wiki/Java(programming_language)), .NET or other compiled programming languages require a build stage to create an executable file. The executable file (e.g., jar file in case of Java) contains all the codes embedded, and the necessary library dependencies, which the application needs to run and work successfully.

Some other programs written languages such as PHP, JavaScript or Python work directly without being built into an executable file – these languages are called interpreted. That is why we could easily deploy the entire code from git into var/www/html and immediately the webserver was able to render the pages in a browser. However, it is not optimal to download code directly from Git onto our servers. There is a smarter way to package the entire application code, and track release versions. We can package the entire code and all its dependencies into an archive such as .tar.gz or .zip, so that it can be easily unpacked on a respective environment’s servers.

In this project, you will understand and get hands on experience around the entire concept around CI/CD from applications perspective. To fully gain real expertise around this idea, it is best to see it in action across different programming languages and from the platform perspective too. From the application perspective, we will be focusing on PHP here; there are more projects ahead that are based on Java, Node.js, .Net and Python. By the time you start working on Terraform, Docker and Kubernetes projects, you will get to see the platform perspective of CI/CD in action.

What is Continuous Integration?

In software engineering, Continuous Integration (CI) is a practice of merging all developers’ working copies to a shared mainline (e.g., Git Repository or some other version control system) several times per day. Frequent merges reduce chances of any conflicts in code and allow to run tests more often to avoid massive rework if something goes wrong. This principle can be formulated as Commit early, push often.

The general idea behind multiple commits is to avoid what is generally considered as Merge Hell or Integration hell. When a new developer joins a new project, he or she must create a copy of the main codebase by starting a new feature branch from the mainline to develop his own features (in some organization or team, this could be called a develop, main or master branch). If there are tens of developers working on the same project, they will all have their own branches created from mainline at different points in time. Once they make a copy of the repository it starts drifting away from the mainline with every new merge of other developers’ codes. If this lingers on for a very long time without reconciling the code, then this will cause a lot of code conflict or Merge Hell, as rightly said. Imagine such a hell from tens of developers or worse, hundreds. So, the best thing to do, is to continuously commit & push your code to the mainline. As many times as tens times per day. With this practice, you can avoid Merge Hell or Integration hell.

CI concept is not only about committing your code. There is a general workflow, let us start it…

Run tests locally: Before developers commit their code to a central repository, it is recommended to test the code locally. So, Test-Driven Development (TDD) approach is commonly used in combination with CI. Developers write tests for their code called unit-tests, and before they commit their work, they run their tests locally. This practice helps a team to avoid having one developer’s work-in-progress code from breaking other developers’ copy of the codebase.

Compile code in CI: After testing codes locally, developers commit and push their work to a central repository. Rather than building the code into an executable locally, a dedicated CI server picks up the code and runs the build there. In this project we will use, already familiar to you, Jenkins as our CI server. Build happens either periodically – by polling the repository at some configured schedule, or after every commit. Having a CI server where builds run is a good practice for a team, as everyone has visibility into each commit and its corresponding builds.

Run further tests in CI: Even though tests have been run locally by developers, it is important to run the unit-tests on the CI server as well. But, rather than focusing solely on unit-tests, there are other kinds of tests and code analysis that can be run using CI server. These are extremely critical to determining the overall quality of code being developed, how it interacts with other developers’ work, and how vulnerable it is to attacks. A CI server can use different tools for Static Code Analysis, Code Coverage Analysis, Code smells Analysis, and Compliance Analysis. In addition, it can run other types of tests such as Integration and Penetration tests. Other tasks performed by a CI server include production of code documentation from the source code and facilitate manual quality assurance (QA) testing processes.

Deploy an artifact from CI: At this stage, the difference between CI and CD is spelt out. As you now know, CI is Continuous Integration, which is everything we have been discussing so far. CD on the other hand is Continuous Delivery which ensures that software checked into the mainline is always ready to be deployed to users. The deployment here is manually triggered after certain QA tasks are passed successfully. There is another CD known as Continuous Deployment which is also about deploying the software to the users, but rather than manual, it makes the entire process fully automated. Thus, Continuous Deployment is just one step ahead in automation than Continuous Delivery.

### Continuous Integration in The Real World

To emphasize a typical CI Pipeline further, let us explore the diagram below a little deeper.


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a3f7cedc-f622-4992-9da2-b51ff92ad2a2)


Version Control: This is the stage where developers’ code gets committed and pushed after they have tested their work locally. Build: Depending on the type of language or technology used, we may need to build the codes into binary executable files (in case of compiled languages) or just package the codes together with all necessary dependencies into a deployable package (in case of interpreted languages).

Unit Test: Unit tests that have been developed by the developers are tested. Depending on how the CI job is configured, the entire pipeline may fail if part of the tests fails, and developers will have to fix this failure before starting the pipeline again. A Job by the way, is a phase in the pipeline. Unit Test is a phase, therefore it can be considered a job on its own.

Deploy: Once the tests are passed, the next phase is to deploy the compiled or packaged code into an artifact repository. This is where all the various versions of code including the latest will be stored. The CI tool will have to pick up the code from this location to proceed with the remaining parts of the pipeline.

Auto Test: Apart from Unit testing, there are many other kinds of tests that are required to analyse the quality of code and determine how vulnerable the software will be to external or internal attacks. These tests must be automated, and there can be multiple environments created to fulfil different test requirements. For example, a server dedicated for Integration Testing will have the code deployed there to conduct integration tests. Once that passes, there can be other sub-layers in the testing phase in which the code will be deployed to, so as to conduct further tests. Such are User Acceptance Testing (UAT), and another can be Penetration Testing. These servers will be named according to what they have been designed to do in those environments. A UAT server is generally be used for UAT, SIT server is for Systems Integration Testing, PEN Server is for Penetration Testing and they can be named whatever the naming style or convention in which the team is used. An environment does not necessarily have to reside on one single server. In most cases it might be a stack as you have defined in your Ansible Inventory. All the servers in the inventory/dev are considered as Dev Environment. The same goes for inventory/stage (Staging Environment) inventory/preprod (Pre-production environment), inventory/prod (Production environment), etc. So, it is all down to naming convention as agreed and used company or team wide.

Deploy to production: Once all the tests have been conducted and either the release manager or whoever has the authority to authorize the release to the production server is happy, he gives green light to hit the deploy button to ship the release to production environment. This is an Ideal Continuous Delivery Pipeline. If the entire pipeline was automated and no human is required to manually give the Go decision, then this would be considered as Continuous Deployment. Because the cycle will be repeated, and every time there is a code commit and push, it causes the pipeline to trigger, and the loop continues over and over again.

Measure and Validate: This is where live users are interacting with the application and feedback is being collected for further improvements and bug fixes. There are many metrics that must be determined and observed here. We will quickly go through 13 metrics that MUST be considered.

### Common Best Practices of CI/CD

Before we move on to observability metrics – let us list down the principles that define a reliable and robust CI/CD pipeline:

Maintain a code repository

Automate build process

Make builds self-tested

Everyone commits to the baseline every day

Every commit to baseline should be built

Every bug-fix commit should come with a test case

Keep the build fast

Test in a clone of production environment

Make it easy to get the latest deliverables

Everyone can see the results of the latest build

Automate deployment (if you are confident enough in your CI/CD pipeline and willing to go for a fully automated Continuous Deployment)

### WHY ARE WE DOING EVERYTHING WE ARE DOING? – 13 DEVOPS SUCCESS METRICS

After all, DevOps is all about continuous delivery or deployment, and being able to ship out quality code as fast as possible. This is a very ambitious thing to desire; therefore, we must be careful not to break things as we are moving very fast. By tracking these metrics, we can determine our delivery speed and bottlenecks before breaking things. Ultimately, the goals of DevOps are enhanced Velocity, Quality, and Performance. But how do we track these parameters? Let us have a look at the 13 metrics to watch out for.

1. Deployment frequency: Tracking how often you do deployments is a good DevOps metric. Ultimately, the goal is to do more smaller deployments as often as possible. Reducing the size of deployments makes it easier to test and release. I would suggest counting both production and non-production deployments separately. How often you deploy to QA or pre-production environments is also important. You need to deploy early and often in QA to ensure enough time for testing.

2. Lead time: If the goal is to ship code quickly, this is a key DevOps metric. I would define lead time as the amount of time that occurs between starting on a work item until it is deployed. This helps you know that if you started on a new work item today, how long would it take on average until it gets to production.

3. Customer tickets: The best and worst indicator of application problems is customer support tickets and feedback. The last thing you want is your users reporting bugs or having problems with your software. Because of this, customer tickets also serve as a good indicator of application quality and performance problems.

4. Percentage of passed automated tests: To increase velocity, it is highly recommended that the development team makes extensive usage of unit and functional testing. Since DevOps relies heavily on automation, tracking how well automated tests work is a good DevOps metrics. It is good to know how often code changes break tests.

5. Defect escape rate: Do you know how many software defects are being found in production versus QA? If you want to ship code fast, you need to have confidence that you can find software defects before they get to production. Defect escape rate is a great DevOps metric to track how often those defects make it to production.

6. Availability: The last thing we ever want is for our application to be down. Depending on the type of application and how we deploy it, we may have a little downtime as part of scheduled maintenance. It is highly recommended to track this metric and all unplanned outages. Most software companies build status pages to track this. Such as this Google Products Status Page

7. Service level agreements: Most companies have some service level agreement (SLA) that they promise to the customers. It is also important to track compliance with SLAs. Even if there are no formally stated SLAs, there probably are application non-functional requirements or expectations to be met.

8. Failed deployments: We all hope this never happens, but how often do our deployments cause an outage or major issues for the users? Reversing a failed deployment is something we never want to do, but it is something you should always plan for. If you have issues with failed deployments, be sure to track this metric over time. This could also be seen as tracking \*Mean Time To Failure (MTTF).

9. Error rates: Tracking error rates within the application is super important. Not only they serve as an indicator of quality problems, but also ongoing performance and uptime related issues. In software development, errors are also known as exceptions, and proper exception handling is critical. If they are not handled nicely, we can figure it out while monitoring the rate of errors.

10. Application usage & traffic: After a deployment, we want to see if the number of transactions or users accessing our system looks normal. If we suddenly have no traffic or a giant spike in traffic, something could be wrong. An attacker may be routing traffic elsewhere, or initiating a DDOS attack

11. Application performance: Before we even perform a deployment, we should configure monitoring tools like Retrace, DataDog, New Relic, or AppDynamics to look for performance problems, hidden errors, and other issues. During and after the deployment, we should also look for any changes in overall application performance and establish some benchmarks to know when things deviate from the norm.

It might be common after a deployment to see major changes in the usage of specific SQL queries, web service or HTTP calls, and other application dependencies. These monitoring tools can provide valuable visualizations like this one below that helps make it easy to spot problems.

12. Mean time to detection (MTTD): When problems happen, it is important that we identify them quickly. The last thing we want is to have a major partial or complete system outage and not know about it. Having robust application monitoring and good observability tools in place will help us detect issues quickly. Once they are detected, we also must fix them quickly!

13. Mean time to recovery (MTTR): This metric helps us track how long it takes to recover from failures. A key metric for the business is keeping failures to a minimum and being able to recover from them quickly. It is typically measured in hours and may refer to business hours, not calendar hours.

These are the major metrics that any DevOps team should track and monitor to understand how well CI/CD process is established and how it helps to deliver quality application to the users.

### SIMULATING A TYPICAL CI/CD PIPELINE FOR A PHP BASED APPLICATION

As part of the ongoing infrastructure development with Ansible started from Project 11, you will be tasked to create a pipeline that simulates continuous integration and delivery. Target end to end CI/CD pipeline is represented by the diagram below. It is important to know that both Tooling and TODO Web Applications are based on an interpreted (scripting) language (PHP). It means, it can be deployed directly onto a server and will work without compiling the code to a machine language.

The problem with that approach is, it would be difficult to package and version the software for different releases. And so, in this project, we will be using a different approach for releases, rather than downloading directly from git, we will be using Ansible uri module.


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/07832028-eb88-4a49-87cb-7195d8d3b5c3)


### Set Up

This project is partly a continuation of your Ansible work, so simply add and subtract based on the new setup in this project. It will require a lot of servers to simulate all the different environments from dev/ci all the way to production. This will be quite a lot of servers altogether (But you don’t have to create them all at once. Only create servers required for an environment you are working with at the moment. For example, when doing deployments for development, do not create servers for integration, pentest, or production yet).

This is still NOT a cloud-focus project yet. AWS cloud end to end project begins from project-15. Therefore, do not worry about advanced AWS or GCP configuration. All we need here is virtual machines that can be accessed over SSH.

To minimize the cost of cloud servers, you don not have to create all the servers at once, simply spin up a minimal server set up as you progress through the project implementation and have reached a need for more.

start with the Ansible server (RHEL8 instance)

Install Git on the ANSIBLE server


![Install Git on Ansible Server](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/b390e9ec-e78b-4adf-a5c5-dcbbe07c1329)


clone the ansible config mgt repo to the new ansible instance server

Install Epel and Remi Repo Release as well as Java


![Install Epel](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/8275ee2a-e236-41b5-a92c-f68cdda725fa)


updating bash profile to export java for ease of usage


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a720992f-b28f-4e74-9630-407b9f213b46)
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/27a04379-652a-48a1-bf9a-9acd5473424a)

installing Jenkins


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/99c35a02-d7e1-4fe3-9eb4-f4d2d7cf1004)
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/4da150ff-7773-4be1-a8b3-e88e03841b95)


log in to Jenkins on browser using public Ip and password from terminal


To get started, we will focus on these environments initially.

Ci

Dev

Pentest

Both SIT – For System Integration Testing and UAT – User Acceptance Testing do not require a lot of extra installation or configuration. They are basically the webservers holding our applications. But Pentest – For Penetration testing is where we will conduct security related tests, so some other tools and specific configurations will be needed. In some cases, it will also be used for Performance and Load testing. Otherwise, that can also be a separate environment on its own. It all depends on decisions made by the company and the team running the show.

What we want to achieve, is having Nginx to serve as a reverse proxy for our sites and tools. Each environment setup is represented in the below table and diagrams.


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d5e8463d-89f3-4f83-9c2f-6089435ce53b)
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/1d88d1dd-2f81-49a1-91a4-321fe97a3540)
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/495f560e-c237-483d-9389-910a7869dd66)


Ansible Inventory should look like this


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d12f9800-e818-4f14-81a3-6b44ec7b3c39)


ci inventory file


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/fdc80555-0e64-4e3e-814e-acadb8ad3a95)


dev Inventory file


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/e6ea1fb1-f863-471a-a54a-f7fb8bacd03f)


pentest inventory file


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5844bfe0-5846-41e6-9873-321b6417faee)


### Observations:

You will notice that in the pentest inventory file, we have introduced a new concept pentest:children This is because, we want to have a group called pentest which covers Ansible execution against both pentest-todo and pentest-tooling simultaneously. But at the same time, we want the flexibility to run specific Ansible tasks against an individual group.

The db group has a slightly different configuration. It uses a RedHat/Centos Linux distro. Others are based on Ubuntu (in this case user is ubuntu). Therefore, the user required for connectivity and path to python interpreter are different. If all your environment is based on Ubuntu, you may not need this kind of set up. Totally up to you how you want to do this. Whatever works for you is absolutely fine in this scenario.

This makes us to introduce another Ansible concept called group\_vars. With group vars, we can declare and set variables for each group of servers created in the inventory file.

For example, If there are variables we need to be common between both pentest-todo and pentest-tooling, rather than setting these variables in many places, we can simply use the group\_vars for pentest. Since in the inventory file it has been created as pentest:children Ansible recognizes this and simply applies that variable to both children.

## ANSIBLE ROLES FOR CI ENVIRONMENT

Now go ahead and Add two more roles to ansible:

SonarQube

Artifactory

### Why do we need SonarQube?

SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality, it is used to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities. Watch a short description here. There is a lot more hands on work ahead with SonarQube and Jenkins. So, the purpose of SonarQube will be clearer to you very soon.

#### Why do we need Artifactory?

Artifactory is a product by JFrog that serves as a binary repository manager. The binary repository is a natural extension to the source code repository, in that the outcome of your build process is stored. It can be used for certain other automation, but we will it strictly to manage our build artifacts.

### Configuring Ansible For Jenkins Deployment

In previous projects, you have been launching Ansible commands manually from a CLI. Now, with Jenkins, we will start running Ansible from Jenkins UI.

To do this,

Navigate to Jenkins URL

Install & Open Blue Ocean Jenkins Plugin


![Install Blue Ocean Plugin](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/f628b897-cf88-4da3-bf38-a66566a123ba)


Create a new pipeline


![Create New Pipeline](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ce102ede-ad78-4613-84e4-cc013f82a761)


Select GitHub

Connect Jenkins with GitHub

Login to GitHub & Generate an Access Token

Copy Access Token

Paste the token and connect


![Access Token](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c6f3e727-9342-4737-9393-8f4b6234ce53)


Create a new Pipeline

At this point you may not have a Jenkinsfile in the Ansible repository, so Blue Ocean will attempt to give you some guidance to create one. But we do not need that. We will rather create one ourselves. So, click on Administration to exit the Blue Ocean console.

Here is our newly created pipeline. It takes the name of your GitHub repository.


![Create New Pipeline](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/dff85ac4-de9b-4380-94a9-5c53aef719c2)


#### Let us create our Jenkinsfile

Inside the Ansible project, create a new directory deploy and start a new file Jenkinsfile inside the directory.


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/cd750485-f1e6-4103-be65-7a1baff44412)


Add the code snippet below to start building the Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage

```
pipeline { 
    agent any

  stages { 
    stage('Build') { 
      steps { 
        script { 
          sh 'echo "Building Stage"' 
        } 
      } 
    } 
    } 
}
```


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/716fa1ba-f31e-47d5-b3e3-ca9fe5599bc9)


Deploy folder reflecting in GitHub repo

Now go back into the Ansible pipeline in Jenkins, and select configure


![Ansible Pipeline Jenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/696248a8-1756-4389-b269-dab21983cd4f)


Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile

Back to the pipeline again, this time click "Build now"

This will trigger a build and you will be able to see the effect of our basic Jenkinsfile configuration by going through the console output of the build.

To really appreciate and feel the difference of Cloud Blue UI, it is recommended to try triggering the build again from Blue Ocean interface.

Click on Blue Ocean

Select your project

Click on the play button against the branch


![Blue Ocean Play Button](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5122b8b1-4690-4821-99aa-497246ad95fe)


Notice that this pipeline is a multibranch one. This means, if there were more than one branch in GitHub, Jenkins would have scanned the repository to discover them all and we would have been able to trigger a build for each branch.

Let us see this in action.

Create a new git branch and name it feature/jenkinspipeline-stages


![New Git Branch](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/75300ded-fb88-4844-bde7-ef11a5b0e2b2)


Currently we only have the Build stage. Let us add another stage called Test. Paste the code snippet below and push the new changes to GitHub.

```
 pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
    }
}
```


![Testing Stage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2785d4bd-d0d9-4d8d-a637-4f18f5d3b3a9)


To make your new branch show up in Jenkins, we need to tell Jenkins to scan the repository.

Click on the "Administration" button

Navigate to the Ansible project and click on "Scan repository now"

Refresh the page and both branches will start building automatically. You can go into Blue Ocean and see both branches there too.

In Blue Ocean, you can now see how the Jenkinsfile has caused a new step in the pipeline launch build for the new branch.


![Stages Working](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a064edeb-9523-44c2-af34-5d3fe949cfc9)


### A QUICK TASK FOR US!

1. Create a pull request to merge the latest code into the main branch
2. After merging the PR, go back into your terminal and switch into the main branch.
3. Pull the latest change.
4. Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
1. Package
2. Deploy
3. Clean up

1. Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch


![Stages Working](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a064edeb-9523-44c2-af34-5d3fe949cfc9)


1. Eventually, your main branch should have a successful pipeline like this in blue ocean


![Successful Pipeline](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c5e61494-e952-4935-b5a0-c7dee4372dc1)

### RUNNING ANSIBLE PLAYBOOK FROM JENKINS

Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:

Installing Ansible on Jenkins


![Installing Ansible on Jenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/264fbd66-98d8-470a-8dc6-fbeeede4cdb4)


Installing Ansible plugin in Jenkins UI


![Ansible Plugin Jenkins](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/71ffb8a0-5cdd-4fe7-911a-5b098b078d7d)


Creating Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)


![Creating Jenkins File 1](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3b204fbe-7241-4a64-9030-bb99d8bdb638)
![Creating Jenkins File 2](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/4fc113b9-af7f-45e7-9e03-492401636ee7)
![Creating Jenkins File 3](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2b617443-e5b7-43ca-ab00-8c9512d305e8)
![Creating Jenkins File 4](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c7b88af9-e4bc-4bca-8f5d-c5d33908ddf8)

  
```
pipeline {
  agent any

  environment {
      ANSIBLE_CONFIG="${WORKSPACE}/deploy/ansible.cfg"
    }

  parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }

  stages{
      stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

      stage('Checkout SCM') {
         steps{
            git branch: 'main', url: 'https://https://github.com/Bamideleflint/ansible-config-mgt'
         }
       }

      stage('Prepare Ansible For Execution') {
        steps {
          sh 'echo ${WORKSPACE}' 
          sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg' 
        }
     }

      stage('Run Ansible playbook') {
        steps {
           ansiblePlaybook become: true, colorized: true, credentialsId: 'key-pair', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory/dev', playbook: 'playbooks/site.yml'
         }
      }

      stage('Clean Workspace after build'){
        steps{
          cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
        }
      }
   }

}
```

  
- Jenkins needs to export the ANSIBLE\_CONFIG environment variable. You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory. This way, anyone can easily identify that everything in there relates to deployment. Then, using the Pipeline Syntax tool in Ansible, generate the syntax to create environment variables to set. Enter this into the ancible.cfg file

```
[defaults]
roles_path= /home/ec2-user/ansible-config-mgt/roles
timeout = 160
callback_whitelist = profile_tasks
log_path=~/ansible.log
host_key_checking = False
gathering = smart
ansible_python_interpreter=/usr/bin/python3
allow_world_readable_tmpfiles=true


[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=30m -o ControlPath=/tmp/ansible-ssh-%h-%p-%r -o ServerAliveInterval=60 -o ServerAliveCountMax=60 -o ForwardAgent=yes
```
 

Note: Ensure that Ansible runs against the Dev environment successfully.

  
![Ansible runs Dev 1](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/daa6c89c-a36c-4d94-9970-88ecde3d3cc5)
![Ansible runs Dev 3](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0b894638-5d9a-4171-925c-9f55441dd16d)
![Ansible runs Dev 4](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5c7dd463-b927-4d60-9bcb-8df19f723f5f)
![Ansible runs Dev 5](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/7f7df280-f956-4f01-bf09-427a363bcb7d)


If everything goes well for you, it means, the Dev environment has an up-to-date configuration. But what if we need to deploy to other environments?

Are we going to manually update the Jenkinsfile to point inventory to those environments? such as sit, uat, pentest, etc. Or do we need a dedicated git branch for each environment, and have the inventory part hard coded there. Think about those for a minute and try to work out which one sounds more like a better solution.

Manually updating the Jenkinsfile is definitely not an option. And that should be obvious to you at this point. Because we try to automate things as much as possible.

Well, unfortunately, we will not be doing any of the highlighted options. What we will be doing is to parameterise the deployment. So that at the point of execution, the appropriate values are applied.

### Parameterizing Jenkinsfile For Ansible Deployment

To deploy to other environments, we will need to use parameters.

Update sit inventory with new servers


![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/81df6d1e-a609-4129-a31f-afecfe3c5c37)


Update Jenkinsfile to introduce parameterization. Below is just one parameter. It has a default value in case if no value is specified at execution. It also has a description so that everyone is aware of its purpose.

```
pipeline {
    agent any

    parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }
...
```

In the Ansible execution section, remove the hardcoded inventory/dev and replace with \`${inventory} From now on, each time you hit on execute, it will expect an input.

  
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/818f8186-b337-41e6-879c-aefc126a6830)

  
Notice that the default value loads up, but we can now specify which environment we want to deploy the configuration to. Simply type sit and hit Run

  
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/75a26a55-efd2-4160-840c-e4b812edd13a)

  
Add another parameter. This time, introduce tagging in Ansible. You can limit the Ansible execution to a specific role or playbook desired. Therefore, add an Ansible tag to run against webserver only. Test this locally first to get the experience. Once you understand this, update Jenkinsfile and run it from Jenkins.

  
![Ansible Execution 1](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/099c6200-e1fb-45ea-ae97-e3b90978425c)
![Ansible Execution 2](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/2487cddd-4110-4362-8ccb-00d7016a3b36)


## CI/CD PIPELINE FOR TODO APPLICATION

We already have tooling website as a part of deployment through Ansible. Here we will introduce another PHP application to add to the list of software products we are managing in our infrastructure. The good thing with this particular application is that it has unit tests, and it is an ideal application to show an end-to-end CI/CD pipeline for a particular application.

Our goal here is to deploy the application onto servers directly from Artifactory rather than from git. If you have not updated Ansible with an Artifactory role, simply use this guide to create an Ansible role for Artifactory (ignore the Nginx part). Configure Artifactory on Ubuntu 20.04

### Phase 1 – Prepare Jenkins

Fork the repository below into your GitHub account


⁠[https://github.com/darey-devops/php-todo.git](https://github.com/darey-devops/php-todo.git "https://github.com/darey-devops/php-todo.git")  


On you Jenkins server, install PHP, its dependencies and Composer tool (Feel free to do this manually at first, then update your Ansible accordingly later)

  
![Install PHP](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ebb5356b-dde3-4987-bc85-9be162f09a36)


####   

#### Install Jenkins plugins

Plot plugin
 

![Plot Plugin](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/49469aa9-fc0d-4575-bd32-1d4c2e8c33bc)

  
Artifactory plugin

  
![Artifactory Plugin](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5d821bed-7a6a-4155-830d-d14bee0f79ef)

  
We will use plot plugin to display tests reports, and code coverage information.

The Artifactory plugin will be used to easily upload code artifacts into an Artifactory server.

In Jenkins UI configure Artifactory

  
![Jenkins UI Artifactory](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/fddeb288-d8f8-41a7-ab2a-740d7e3a6357)


Configure the server ID, URL and Credentials, run Test Connection.

### Phase 2 – Integrate Artifactory repository with Jenkins

  
![Integrate Artifactory](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/13544cd1-ecf6-4ec3-a40c-e4e512b0221f)

  
Create a dummy Jenkinsfile in the repository

  
![Dummy Jenkinsfile](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3967060d-c89d-4b8c-8928-5b0e5e95bee1) 


Using Blue Ocean, create a multibranch Jenkins pipeline

On the database server, create database and user

`Create database homestead;` `CREATE USER 'homestead'@'%' IDENTIFIED BY 'sePret^i';` `GRANT ALL PRIVILEGES ON * . * TO 'homestead'@'%';`

Update the database connectivity requirements in the file .env.sample

Update Jenkinsfile with proper pipeline configuration

```
pipeline {
    agent any

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }
  
    stage('Checkout SCM') {
      steps {
            git branch: 'main', url: 'https://github.com/darey-devops/php-todo.git'
      }
    }

    stage('Prepare Dependencies') {
      steps {
             sh 'mv .env.sample .env'
             sh 'composer install'
             sh 'php artisan migrate'
             sh 'php artisan db:seed'
             sh 'php artisan key:generate'
      }
    }
  }
}
```

  
Notice the Prepare Dependencies section

The required file by PHP is .env so we are renaming .env.sample to .env

Composer is used by PHP to install all the dependent libraries used by the application

php artisan uses the .env file to setup the required database objects – (After successful run of this step, login to the database, run show tables and you will see the tables being created for you)

Update the Jenkinsfile to include Unit tests step

```
    stage('Execute Unit Tests') 
      steps {
             sh './vendor/bin/phpunit'
      } 
```

  
### Phase 3 – Code Quality Analysis

This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is phploc. Read the article here for more

The data produced by phploc can be ploted onto graphs in Jenkins.

Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file.

```
    stage('Code Analysis') {
      steps {
            sh 'phploc app/ --log-csv build/logs/phploc.csv'

      }
    }
```

  
Plot the data using plot Jenkins plugin.

This plugin provides generic plotting (or graphing) capabilities in Jenkins. It will plot one or more single values variations across builds in one or more plots. Plots for a particular job (or project) are configured in the job configuration screen, where each field has additional help information. Each plot can have one or more lines (called data series). After each build completes the plots’ data series latest values are pulled from the CSV file generated by phploc.

```
    stage('Plot Code Coverage Report') {
      steps {

            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Lines of Code (LOC),Comment Lines of Code (CLOC),Non-Comment Lines of Code (NCLOC),Logical Lines of Code (LLOC)                          ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'A - Lines of code', yaxis: 'Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Directories,Files,Namespaces', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'B - Structures Containers', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Average Class Length (LLOC),Average Method Length (LLOC),Average Function Length (LLOC)', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'C - Average Length', yaxis: 'Average Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Cyclomatic Complexity / Lines of Code,Cyclomatic Complexity / Number of Methods ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'D - Relative Cyclomatic Complexity', yaxis: 'Cyclomatic Complexity by Structure'      
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Classes,Abstract Classes,Concrete Classes', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'E - Types of Classes', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Methods,Non-Static Methods,Static Methods,Public Methods,Non-Public Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'F - Types of Methods', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Constants,Global Constants,Class Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'G - Types of Constants', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Test Classes,Test Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'I - Testing', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Logical Lines of Code (LLOC),Classes Length (LLOC),Functions Length (LLOC),LLOC outside functions or classes ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'AB - Code Structure by Logical Lines of Code', yaxis: 'Logical Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Functions,Named Functions,Anonymous Functions', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'H - Types of Functions', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Interfaces,Traits,Classes,Methods,Functions,Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'BB - Structure Objects', yaxis: 'Count'


      }
    }
```

  
You should now see a Plot menu item on the left menu. Click on it to see the charts. (The analytics may not mean much to you as it is meant to be read by developers. So, you need not worry much about it – this is just to give you an idea of the real-world implementation).

  
![Plot Build Data](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c78a6ad8-de52-46bf-9f8e-5b1578dd9648)
![Plot Build Bloc](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/ec52df4c-96d6-4133-b3ae-54cc3f6c5453)

  
Bundle the application code for into an artifact (archived package) upload to Artifactory

```
stage ('Package Artifact') {
    steps {
            sh 'zip -qr php-todo.zip ${WORKSPACE}/*'
     }
    }
```

Publish the resulted artifact into Artifactory

```
stage ('Upload Artifact to Artifactory') {
          steps {
            script { 
                 def server = Artifactory.server 'artifactory-server'                 
                 def uploadSpec = """{
                    "files": [
                      {
                       "pattern": "php-todo.zip",
                       "target": "<name-of-artifact-repository>/php-todo",
                       "props": "type=zip;status=ready"

                       }
                    ]
                 }""" 

                 server.upload spec: uploadSpec
               }
            }
  
        }
```

Deploy the application to the `dev` environment by launching Ansible pipeline

```
stage ('Deploy to Dev Environment') {
    steps {
    build job: 'ansible-project/main', parameters: [[$class: 'StringParameterValue', name: 'env', value: 'dev']], propagate: false, wait: true
    }
  }
```

  
The `build job` used in this step tells Jenkins to start another job. In this case it is the `ansible-project` job, and we are targeting the `main` branch. Hence, we have `ansible-project/main`. Since the Ansible project requires parameters to be passed in, we have included this by specifying the `parameters` section. The name of the parameter is `env` and its value is `dev`. Meaning, deploy to the Development environment.

But how are we certain that the code being deployed has the quality that meets corporate and customer requirements? Even though we have implemented **Unit Tests** and **Code Coverage** Analysis with `phpunit` and `phploc`, we still need to implement **Quality Gate** to ensure that ONLY code with the required code coverage, and other quality standards make it through to the environments.

To achieve this, we need to configure **SonarQube** - An open-source platform developed by **SonarSource** for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities.

### SONARQUBE INSTALLATION

Before we start getting hands on with SonarQube configuration, it is incredibly important to understand a few concepts:

Software Quality – The degree to which a software component, system or process meets specified requirements based on user needs and expectations.

Software Quality Gates – Quality gates are basically acceptance criteria which are usually presented as a set of predefined quality criteria that a software development project must meet in order to proceed from one stage of its lifecycle to the next one.

SonarQube is a tool that can be used to create quality gates for software projects, and the ultimate goal is to be able to ship only quality software code.

Despite that DevOps CI/CD pipeline helps with fast software delivery, it is of the same importance to ensure the quality of such delivery. Hence, we will need SonarQube to set up Quality gates. In this project we will use predefined Quality Gates (also known as The Sonar Way). Software testers and developers would normally work with project leads and architects to create custom quality gates.

Install SonarQube on Ubuntu 20.04 With PostgreSQL as Backend Database

  
![Sonarqube EC2](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/440641e4-03e4-43d4-b02a-e5aa5e527ca5)


CONFIGURE SONARQUBE

  
![Configure Sonarqube](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/17dbcb59-9274-4a41-9803-597580272abf)

  
We cannot run SonarQube as a root user, if you run using root user it will stop automatically. The ideal approach will be to create a separate group and a user to run SonarQube


Access SonarQube

To access SonarQube using browser, type server’s IP address followed by port 9000

[http://server\_IP:9000](http://server_IP:9000) OR [http://localhost:9000](http://localhost:9000)

  
![Sonarqube Browser](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/921d39bc-2a9c-4aed-bc6f-d82eb8f903a8)

  
Login to SonarQube with default administrator username and password – admin

Now, when SonarQube is up and running, it is time to setup our Quality gate in Jenkins.

  
### CONFIGURE SONARQUBE AND JENKINS FOR QUALITY GATE

In Jenkins, install SonarScanner plugin

  
![Sonarqube Scanner Plugin](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/55bbcb19-cf8c-4cad-800d-51e92c72c9a0)

  
Navigate to configure system in Jenkins. Add SonarQube server as shown below:

  
![Add Sonarqube URL](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/9f749b36-7916-41a4-bc24-186c16500576)


Manage Jenkins > Configure System

Generate authentication token in SonarQube

User > My Account > Security > Generate Tokens

Configure Quality Gate Jenkins Webhook in SonarQube – The URL should point to your Jenkins server [http://{JENKINS\_HOST}/sonarqube-webhook/](http://{JENKINS_HOST}/sonarqube-webhook/)

Administration > Configuration > Webhooks > Create

  
![Sonarqube Webhooks](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/c5a450fb-4999-4a65-b233-0a5aa28c258b)


Setup SonarQube scanner from Jenkins – Global Tool Configuration

  
![Sonarqube Scanner Jenkins Global Tool Config](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/6b9ec075-2025-4951-aab4-64e58ffefc80)

  
Manage Jenkins > Global Tool Configuration

Update Jenkins Pipeline to include SonarQube scanning and Quality Gate

Below is the snippet for a Quality Gate stage in Jenkinsfile.

```
    stage('SonarQube Quality Gate') {
        environment {
            scannerHome = tool 'SonarQubeScanner'
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
            }

        }
    }
```


NOTE: The above step will fail because we have not updated sonar-scanner properties

  
![Sonarqube Analysis Error](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0f862f00-6e3d-4646-ba25-11ea1fb5b6d1)


Configure sonar-scanner properties – From the step above, Jenkins will install the scanner tool on the Linux server. You will need to go into the tools directory on the server to configure the properties file in which SonarQube will require to function during pipeline execution.

`cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/conf/`

Open sonar-scanner.properties file

`sudo vi sonar-scanner.properties`

Add configuration related to php-todo project

sonar.host.url=[http://:9000](http://:9000) sonar.projectKey=php-todo #----- Default source code encoding sonar.sourceEncoding=UTF-8 sonar.php.exclusions=**/vendor/** sonar.php.coverage.reportPaths=build/logs/clover.xml sonar.php.tests.reportPath=build/logs/junit.xml


![Pipeline Overview](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/913f59c0-920a-4143-8087-16d6c6f203af)
![Sonar Scanner PHP](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/1c6df71f-702e-48cc-b6a8-d1c3d113e7c1) 


HINT: To know what exactly to put inside the sonar-scanner.properties file, SonarQube has a configurations page where you can get some directions.

A brief explanation of what is going on the the stage – set the environment variable for the scannerHome use the same name used when you configured SonarQube Scanner from Jenkins Global Tool Configuration. If you remember, the name was SonarQubeScanner. Then, within the steps use shell to run the scanner from bin directory.

To further examine the configuration of the scanner tool on the Jenkins server – navigate into the tools directory

`cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/bin`

ist the content to see the scanner tool sonar-scanner. That is what we are calling in the pipeline script.

Output of ls -latr

  
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3f4c5c18-ae8e-48e8-8212-e195a2c12ed4)

  
So far you have been given code snippets on each of the stages within the Jenkinsfile. But, you should also be able to generate Jenkins configuration code yourself.

To generate Jenkins code, navigate to the dashboard for the php-todo pipeline and click on the Pipeline Syntax menu item

Dashboard > php-todo > Pipeline Syntax

  
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/9af01a47-0e1b-4c9e-8cc2-fe24efebc2f2)


Click on Steps and select withSonarQubeEnv – This appears in the list because of the previous SonarQube configurations you have done in Jenkins. Otherwise, it would not be there.

  
![image](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/4dd910b9-ea9e-48e6-9b2c-4f4118d9e2a7)

  
Within the generated block, you will use the sh command to run shell on the server. For more advanced usage in other projects, you can add to bookmarks this SonarQube documentation page in your browser.

  
## End-to-End Pipeline Overview

Indeed, this has been one of the longest projects from Project 1, and if everything has worked out for you so far, you should have a view like below:

  
![Sonar Scanner Success](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/b2eda470-61e8-412a-996f-4f760ef90e77)


But we are not completely done yet!

The quality gate we just included has no effect. Why? Well, because if you go to the SonarQube UI, you will realise that we just pushed a poor-quality code onto the development environment.

Navigate to php-todo project in SonarQube

There are bugs, and there is 0.0% code coverage. (code coverage is a percentage of unit tests added by developers to test functions and objects in the code)

If you click on php-todo project for further analysis, you will see that there is 6 hours’ worth of technical debt, code smells and security issues in the code.

  
![PHP todo Analysis](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/42d735e5-353f-47b8-8b1c-e4b42d31d19f)

  
In the development environment, this is acceptable as developers will need to keep iterating over their code towards perfection. But as a DevOps engineer working on the pipeline, we must ensure that the quality gate step causes the pipeline to fail if the conditions for quality are not met.

### Conditionally deploy to higher environments

In the real world, developers will work on feature branch in a repository (e.g., GitHub or GitLab). There are other branches that will be used differently to control how software releases are done. You will see such branches as:

Develop

Master or Main (The \* is a place holder for a version number, Jira Ticket name or some description. It can be something like Release-1.0.0)

Feature/\*

Release/\*

Hotfix/\*

etc.

There is a very wide discussion around release strategy, and git branching strategies which in recent years are considered under what is known as GitFlow

Assuming a basic gitflow implementation restricts only the develop branch to deploy code to Integration environment like sit.

Let us update our Jenkinsfile to implement this:

First, we will include a When condition to run Quality Gate whenever the running branch is either develop, hotfix, release, main, or master

```
when { branch pattern: "^develop*|^hotfix*|^release*|^main*", comparator: "REGEXP"}
```

Then we add a timeout step to wait for SonarQube to complete analysis and successfully finish the pipeline only when code quality is acceptable.

```
    timeout(time: 1, unit: 'MINUTES') {
        waitForQualityGate abortPipeline: true
    }
```

The complete stage will now look like this:

```
    stage('SonarQube Quality Gate') {
      when { branch pattern: "^develop*|^hotfix*|^release*|^main*", comparator: "REGEXP"}
        environment {
            scannerHome = tool 'SonarQubeScanner'
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-project.properties"
            }
            timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
        }
    }
```

  
To test, create different branches and push to GitHub. You will realise that only branches other than develop, hotfix, release, main, or master will be able to deploy the code.

If everything goes well, you should be able to see something like this:

  
![Sonarqube Analysis Error](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5fbd0384-5b73-435e-b9ac-8b937bdbfdfb)

  
Notice that with the current state of the code, it cannot be deployed to Integration environments due to its quality. In the real world, DevOps engineers will push this back to developers to work on the code further, based on SonarQube quality report. Once everything is good with code quality, the pipeline will pass and proceed with sipping the codes further to a higher environment.

Complete the following tasks to finish Project 14

Introduce Jenkins agents – Add 2 more servers to be used as Jenkins agent. Configure Jenkins to run its pipeline jobs randomly on any available agent nodes.

Configure webhook between Jenkins and GitHub to automatically run the pipeline when there is a code push.


![Node Agents](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/88d6a37c-da4d-43a4-9795-60956ff62543)


Deploy the application to all the environments

Optional – Experience pentesting in pentest environment by configuring Wireshark there and just explore for information sake only. Watch Wireshark Tutorial here

Ansible Role for Wireshark:

- [https://github.com/ymajik/ansible-role-wireshark](https://github.com/ymajik/ansible-role-wireshark) (Ubuntu)
- [https://github.com/wtanaka/ansible-role-wireshark](https://github.com/wtanaka/ansible-role-wireshark) (RedHat)

  

### **Congratulations! You have just experienced one of the most interesting and complex projects in you Project Based Learning journey so far.**
