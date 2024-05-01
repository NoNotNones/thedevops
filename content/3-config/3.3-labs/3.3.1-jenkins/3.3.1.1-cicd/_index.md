---
title : "CICD"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.3.1.1 </b> "
---

In a Jenkins CI/CD setup, an agent is a worker node that performs the tasks defined in the Jenkins pipeline. 

#### Agent
Hostname: **Lab-Server**

Required: Installed JAVA
Configuration:
- Install JAVA: **apt install openjdk-11-jdk -y**
- Create User: **adduser jenkins**
- Create working folder: mkdir /var/lib/jenkins
- Permission: 
  - chown jenkins. /var/lib/jenkins
  - cd /var/lib/jenkins/
  - su jenkins

#### Jenkins-Server
**Add Node**: Dashboard > Manage Jenkins > Nodes > New node
- Node name: **lab-server**
  - Number of excute: 1
  - Lables: lab-server
  - Custom workdir path: /var/lib/jenkins
    - Add inbound port: 8999

Manage Jenkins - Security
  - Agent - Fixed: 8999

#### Connect Jenkins to Gitlab-Server

