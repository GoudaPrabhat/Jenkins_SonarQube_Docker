STEPS:
Create a Jenkins server using AWS EC2 instance
Open port 8080 in your security group inbound rule to access Jenkins via browser
run the following commands
  sudo apt update
  sudo apt install openjdk-11-jre
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]"   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
  systemctl status jenkins
