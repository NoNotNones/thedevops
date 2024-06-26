---
title : "Jenkins"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

#### Overview
Jenkins is an open-source automation server that is widely used for automating software development processes such as building, testing, and deploying applications. It allows developers to automate repetitive tasks associated with the software development lifecycle, thereby saving time and reducing errors.

- Login
  - **vagrant ssh machine2**
  
#### Install Jenkins server in Linux
- Login Jenkins-Server
- Installation : 
  - {{%expand "Step by step" %}}

```sh
# Change to the root user after logging in:

    sudo su -

# Add the aptitude key for the Jenkins application:

    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

# Add the Jenkins debian repo to the aptitude sources list:

    echo "deb https://pkg.jenkins.io/debian-stable binary/" > /etc/apt/sources.list.d/jenkins.list

# Update the source lists and upgrade any out of date packages:

    apt update
    apt -y upgrade

# Install the software for the Jenkins master:  openjdk-11-jdk, nginx, and jenkins.

# Install JDK and nginx first:
    apt -y install openjdk-11-jdk nginx

# Then install jenkins:

    apt -y install jenkins

  # Confirm that jenkins and nginx are installed:

    systemctl status nginx  | grep Active
    systemctl status jenkins  | grep Active
```
{{% /expand%}}

  - {{%expand "Install via shell scripts (checking ...)" %}}

    vi **jenkins-install.sh**

````sh
    #!/bin/bash

apt install openjdk-11-jdk -y
java --version
wget -p -O - https://pkg.jenkins.io/debian/jenkins.io.key | apt-key add -
sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
apt-get update
apt install jenkins -y
systemctl start jenkins
systemctl enable jenkins
ufw allow 8080
````
    chmod +x jenkins-install.sh
    sh jenkins-install.sh
    systemctl status jenkins

{{% /expand%}}

#### Install Jenkins server running in docker container

  - {{%expand "Step by step" %}}
     
  - Prepair
```linux
   sudo -i
   apt update
   mkdir /tools
   cd /tools
   mkdir docker
   cd docker
   vi install-docker.sh
```     
File content: install-docker.sh

```sh
#!/bin/bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce
sudo systemctl start docker
sudo systemctl enable docker
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker --version
docker-compose --version
```
  - Install
```linux
chmod +x install-docker.sh
sh install-docker.sh

```
![61](/thedevops/images/6-labs/6.1-jenkins/1.png)
![61](/thedevops/images/6-labs/6.1-jenkins/2.png)

- Create Docker compose file
```linux
   mkdir /tools/jenkins
   cd /tools/jenkins
   vi docker-compose.yml
```

```sh
version: '3'
services:
# Jenkins  
  jenkins:
    container_name: jenkins
    image: jenkins/jenkins
    ports:
      - "8080:8080"
    volumes:
      - jenkins_home:/var/jenkins_home
volumes:
  jenkins_home:
```
- Run Jenkins Server
```linux
docker-compose up jenkins -d
```
![61](/thedevops/images/6-labs/6.1-jenkins/3.png)

{{% /expand%}}

#### Configuration Jenkins Server:
- Login: `http://192.168.33.110:8080`
- Configuration
  - {{%expand "Admin Configuration: expand" %}}    
 ```sh
## Admin password: 
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

![61](/thedevops/images/6-labs/6.1-jenkins/4.png)
![61](/thedevops/images/6-labs/6.1-jenkins/5.png)
![61](/thedevops/images/6-labs/6.1-jenkins/6.png)
![61](/thedevops/images/6-labs/6.1-jenkins/7.png)
![61](/thedevops/images/6-labs/6.1-jenkins/8.png)
{{% /expand%}}

- Dashboard Overview
![61](/thedevops/images/6-labs/6.1-jenkins/9.png?featherlight=false&width=50pc)

#### Create a pipeline project


### Content

1. [Jenkins CICD](6.1.1-cicd/)
2. [Advanced](6.1.2-advanced/)
