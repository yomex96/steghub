## Tooling Website Deployment Automation with Continuous Integration and Jenkins


## Introduction
Jenkins is an open-source Continuous Integration server written in Java for orchestrating a chain of actions to achieve the Continuous Integration process in an automated fashion. Jenkins supports the complete development life cycle of software from building, testing, documenting the software, deploying, and other stages of the software development life cycle.

![jenkins_architecture](Images/jenkins.png)
#

Here I introduce Jenkins to automate code delivery to the NFS server


## Jenkins Web Architecture For CI Builds
#
 
![my_jenkins_architecture](./Img/2022-10-05_050932.png)

## Installing Jenkins Server

Spun up a web server on AWS cloud and SSH into it.

Installing JDK which is an important Java based package required for Jenkins to run.
```
sudo apt update
sudo apt install default-jdk-headless
```
![java_installation](./Img/1.java_installation.jpg)

```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins

sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
![jenkins_installation](./Img/2.jenkins_installation_status.jpg)

Since Jenkins runs on default port 8080, open this port on the Security Group inbound rule of the jenkins server on AWS 
![jenkins_rule](./Img/3.jenkins_sg_rule.jpg)

Jenkins is up and running, copy and paste jenkins server public ip address appended with port 8080 on a web server to gain access to the interactive console. `<jenkins_server_public_ip_address>:8080`
![jenkins_install_success](./Img/4.jenkins_install_success.jpg)

The admin password can be found in the `'/var/lib/jenkins/secrets/initialAdminPassword'` path on the server.

![suggested_installation](./Img/5.suggested_install.jpg)
![plugin_installation](./Img/6.plugin_installation.jpg)
![login_success](./Img/7.login_success.jpg)

## Attaching WebHook to Jenkins Server

On the github repository that contains application code, create a webhook to connect to the jenkins job. To create webhook, go to the settings tab on the github repo and click on webhooks.
Webhook should look like this `<public_ip_of_jenkins_server>:8080/github-webhook/`
![webhook_creation](./Img/8.webhook_creation.jpg)

## Creating Job and Configuring GIT Based Push Trigger

On the jenkins server, create a new freestyle job
![creating_job](./Img/9.creating_job.jpg)

In configuration of the Jenkins freestyle job choose Git repository, provide there the link to the GitHub repository and credentials (user/password) so Jenkins could access files in the repository. Also specify the branch containing code

![git_url_input](./Img/10.git_url_input.jpg)
![specify_branch](./Img/11.specify_branch.jpg)
![specify_credentials](./Img/12.specify-credentials.jpg)

## Configuring Build Triggers

Specify the particular trigger to use for triggering the job. Click "Configure" on the jenkins job and add these two configurations

### 1. Configure triggering the job from GitHub webhook:

![adding_build_triggers](./Img/13.adding_build_triggers.jpg)

### 2. Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".

![post_build_step](./Img/14.post_build_step.jpg)

At this point, our architecture has pretty much been built, lets taste it by making a change on any file on the Github repository and then push it to see the triggered job
![git_push](./Img/15.github_push.jpg)

The console output shows the created job and the successful build.
In this case the code on Github was built into an artifact on our Jenkins server workspace. Find the artificat by checking the status tab of the completed job .
![new_build](./Img/16.new_build.jpg)
![persisted_archive](./Img/17.persisted_archive.jpg)

Our created artifact can be found on our local terminal too at this path 
`/var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/`
![artifact_terminal](./Img/18.artifact_terminal.jpg)


## Configuring Jenkins To Copy Files(Artifact) to NFS Server

To achieve this, we install the `Publish Via SSH` pluging on Jenkins.
The plugin allows one to send newly created packages to a remote server and install them, start and stop services that the build may depend on and many other use cases.

On main dashboard select "Manage Jenkins" and choose "Manage Plugins" menu item.

On "Available" tab search for "Publish Over SSH" plugin and install it
![plugin](./Img/19.plugin.jpg)

Configure the job to copy artifacts over to NFS server.
On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to the NFS server:

Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)

Hostname – can be private IP address of NFS server <br/>
Username – ec2-user (since NFS server is based on EC2 with RHEL 8) <br/>
Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server

Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

![setting_publish](./Img/20.setting_pos.jpg)
![server_add](./Img/21.server_add.jpg)

We specify `**` on the `send build artifacts` tab meaning it sends all artifact to specified destination path(NFS Server). 
![](./Img/22.archive_path.jpg)
![](./Img/23.archive_path2.jpg)

Now make a new change on the source code and push to github, Jenkins builds an artifact by downloading the code into its workspace based on the latest commit and via SSH it publishes the artifact into the NFS Server to update the source code. 

This is seen by the change of name on the web application
![](./Img/24.new_changes.jpg)
![](./Img/25.new_change_updated.jpg)

## Challenges Encountered and Solutions
**1. Setting Up Jenkins on AWS**
Challenge: Configuring Jenkins on an AWS instance involved multiple steps, including handling permissions, dependencies, and security configurations. AWS Security Groups required careful setup to allow traffic on the Jenkins default port (8080).
Solution: Followed a step-by-step approach, starting with updating the system and installing the required Java Development Kit (JDK), which Jenkins depends on. Opened port 8080 on the Security Group to allow external access to the Jenkins console. These actions enabled access and remote interaction with Jenkins.

**2. Configuring GitHub Webhook for Automated Builds**
Challenge: Setting up a webhook between GitHub and Jenkins to automatically trigger builds on code updates required careful configuration. It was crucial to ensure that Jenkins could listen to GitHub events.
Solution: Created a webhook in GitHub and linked it to the Jenkins server by providing the Jenkins URL with /github-webhook/. This connection allowed Jenkins to receive notifications of Git pushes, automatically initiating builds.

**3. Managing Build Artifacts with Jenkins**
Challenge: Ensuring that build artifacts were stored and accessible for deployment was vital, yet configuring Jenkins to archive these artifacts consistently posed some difficulties.
Solution: Set up Jenkins' “Post-build Actions” to archive the build artifacts, ensuring that each build generated an archive of compiled code that could be deployed. This allowed easy access to past build artifacts for troubleshooting and review.

**4. Deploying Artifacts to NFS Server**
Challenge: Transferring build artifacts from Jenkins to an NFS server for deployment required reliable and secure SSH access. The NFS server needed to accept connections and store the artifacts for downstream processes.
Solution: Installed the Publish Over SSH plugin in Jenkins, configured SSH access with a secure private key, and specified the target directory on the NFS server. Testing the SSH connection ensured that Jenkins could securely publish the artifacts to the NFS server.


## Benefits of the Project

**Increased Deployment Speed:** Automation with Jenkins allowed code changes to reach production faster and more reliably, reducing the need for manual intervention.

**Reliability and Consistency:** The CI/CD pipeline ensures consistent builds and deployments, reducing errors and facilitating easy rollbacks using archived artifacts.

**Efficient Collaboration:** Team members can push code changes to GitHub without manually coordinating deployment, as Jenkins handles builds and deployments autonomously.

**Enhanced Security and Control:** Secure configurations with AWS Security Groups, SSH, and GitHub webhooks provide a controlled environment for deployment.

**Scalability:** This setup is scalable across multiple environments, supporting additional servers or services as the project grows.
This automated deployment pipeline provides a structured, secure, and efficient way to manage code from development to deployment, greatly enhancing productivity and stability.
