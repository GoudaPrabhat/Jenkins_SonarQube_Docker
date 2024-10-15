STEPS:
Jenkins
Create a Jenkins server using AWS EC2 instance

run the following commands
  sudo apt update
  sudo apt install openjdk-11-jre
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]"   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
  systemctl status jenkins

Copy the admin password to setup the jenkins wizard 

Update the webhook by mentioning the jenkins server url in the payload link with a name and check the webhook
http://public_ip:port/github-webhook/

Open port 8080 in your security group inbound rule to access Jenkins via browser
change the hostname: sudo hostnamectl set-hostname jenkins
then enter /bin/bash to check if the hostname is updated

SonarQube

  sudo apt update
  sudo apt install openjdk-17-jre
   wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.7.0.96327.zip?_gl=1*apb4l9*_gcl_au*NDgwMzM3MzQ2LjE3Mjg0MzU0NzA.*_ga*MTcyMjI3MzY1OS4xNzI4NDM1NDcw*_ga_9JZ0GZ5TC6*MTcyODQzNTQ3MC4xLjEuMTcyODQzNTU1Mi42MC4wLjA.
  ls
  sudo apt install unzip
  unzip sonarqube-10.7.0.96327.zip?_gl=1*apb4l9*_gcl_au*NDgwMzM3MzQ2LjE3Mjg0MzU0NzA.*_ga*MTcyMjI3MzY1OS4xNzI4NDM1NDcw*_ga_9JZ0GZ5TC6*MTcyODQzNTQ3MC4xLjEuMTcyODQzNTU1Mi42MC4wLjA.
