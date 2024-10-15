STEPS:
Set up Jenkins server

run the following commands
# to update the system
  sudo apt update
# install java 17 runtime environment
  sudo apt install openjdk-17-jre
#install jenkins
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]"   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
  systemctl status jenkins
# copy the Jenkins administrative  password

create jenkins pipeline
metntion the github repository

enable github webhook
Update the webhook by mentioning the jenkins server url in the payload link with a name and check the webhook
http://public_ip:port/github-webhook/

change the hostname: sudo hostnamectl set-hostname jenkins
then enter /bin/bash to check if the hostname is updated
Open port 8080 in your security group inbound rule to access Jenkins via browser

test jenkins pipeline
------------------------------------------------------------
Set up SonarQube server

sudo apt update 
sudo apt install openjdk-17-jre
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.7.0.96327.zip?_gl=1*apb4l9*_gcl_au*NDgwMzM3MzQ2LjE3Mjg0MzU0NzA.*_ga*MTcyMjI3MzY1OS4xNzI4NDM1NDcw*_ga_9JZ0GZ5TC6*MTcyODQzNTQ3MC4xLjEuMTcyODQzNTU1Mi42MC4wLjA.
sudo apt install unzip
unzip sonarqube-10.7.0.96327.zip?_gl=1*apb4l9*_gcl_au*NDgwMzM3MzQ2LjE3Mjg0MzU0NzA.*_ga*MTcyMjI3MzY1OS4xNzI4NDM1NDcw*_ga_9JZ0GZ5TC6*MTcyODQzNTQ3MC4xLjEuMTcyODQzNTU1Mi42MC4wLjA.

set up new sonarqube project
Give a project key(name) and select jenkins as a source and copy the project key.

create new sonarqube token
from the admin setting create a global token and copy the token

install and configure SonarQube Scanner plugin in jenkins
In Jenkins: select plugins and install SonarQube Scanner and SSH2 Easy plugins
Go to global tool confgiuration and add sonarqube scanner.
From configure system, add sonarQube and mention the url of the sonarQube url and add the sonarQube token with an id.
Now in the pipeline add sonarQube key in the build steps 

Test jenkins pipeline (which incl. the sonarqube code checking build step)
--------------------------------------------------------------
Set up docker server (Hosting server)

Update the system and install docker

sudo apt update 

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Set up ssh of docker server, create a ssh key pair in jenkins server and copy to docker server
In Docker server : edit the sshd config file
sudo su
nano /etc/ssh/sshd_config
then uncomment 
PubKeyAuthentication Yes
and change 
PasswordAuthentication Yes

Then restart the service: systemctl restart sshd
To set the password : passwd ubuntu
and change the password
 
In Jenkins server:  
create a key
ssh-keygen
ssh-copy-id ubuntu@public_id_of_hostingServer

Add docker server into jenkins pipeline 
In Manage Jenkins > Configure System
In Server group center, add the details of the docker server and then add the IP address of the server

Add a remote shell build step to test if jenkins server can ssh into docker server and run shell script
Select the pipeline and in the post build step select the remote shell and try some commands to check if the server was properly installed

create a Dockerfile

Add a execute shell build step to copy build files from jenkins server to docker server
scp -r . /* ubuntu@Ip_Dockerserver:~/directory/

Check if the user has permission to run the docker commands
if not then run the follwoing commamnd
sudo usermod -aG docker ubuntu
newgrp docker
docker ps

Add a remote shell build step to build the docker image and spin it up in docker server
cd /home/ubuntu/directory
docker build -t Myresume_website .
docker run -d -p 8085:80 --name=Mywebsite Myresume_website

Allow the port 8085 in security group of the server and check if the website is working. 
