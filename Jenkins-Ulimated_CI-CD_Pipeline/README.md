# Jenkins-Ulimate-CI-CD Pipeline

## Installation on EC2 Instance


Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

<img width="927" height="555" alt="aws_ec2" src="https://github.com/user-attachments/assets/7a5bfab0-a439-4014-ba0c-f7a48a40d820" />


## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances

<img width="1913" height="315" alt="inbound_rules" src="https://github.com/user-attachments/assets/519c910c-a11e-4e4f-bae5-3b8d34843558" />


### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).


<img width="1887" height="865" alt="unlock_jenkins" src="https://github.com/user-attachments/assets/ebaee1dd-8e20-4f4d-828f-fa9c8ec6d7dd" />

### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      


### Click on Install suggested plugins

<img width="1893" height="867" alt="jenkins_install_plugin" src="https://github.com/user-attachments/assets/bfc438c5-7154-41b1-b731-104688076301" />

<img width="1896" height="870" alt="jenkins_install_plugin_2" src="https://github.com/user-attachments/assets/0ce3cf4d-ddf7-49dc-8c44-c1ec5d9f047f" />

Wait for the Jenkins to Install suggested plugins


<img width="1400" height="867" alt="jenkins_create_firstadmin" src="https://github.com/user-attachments/assets/cb669310-4472-4a28-a0b2-4df5640d708b" />

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

Jenkins Installation is Successful. You can now starting using the Jenkins

<img width="702" height="316" alt="jenkins_successfully_installed" src="https://github.com/user-attachments/assets/957d5f31-a491-466b-b762-e084eac41d5a" />

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
  

Wait for the Jenkins to be restarted.


## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.




