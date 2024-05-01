---
title : "Container"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

A **container** is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries, and settings

#### Overview

##### Docker
- Docker is a platform that enables developers to build, package, ship, and run applications in containers.
- It provides tools and a platform to manage containerized applications across different environments, from development to production. 

##### Docker Compose
- Docker Compose is a tool provided by Docker that allows you to define and manage multi-container Docker applications.
-  It uses a YAML file to configure the services, networks, and volumes required for your application

##### Configuration
Check the installed software
```docker
 docker --version 
```
```dockercompose
 docker-compose --version 
```
![22](/cicd-ws/images/2-prepair/2.1-docker/4.png)

Create a docker compose file to run the software on the container environment
-  **docker-compose.yml**
```Docker-compose
version: '3'
services:

# Terraform
  terraform:
    image: hashicorp/terraform:latest
    volumes:
      - .:/terraform
    working_dir: /terraform

# AWS CLI'
  aws:
    image: anigeo/awscli
    environment:
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
      AWS_REGION: "${AWS_REGION}"
      AWS_DEFAULT_REGION: ap-southeast-1
    volumes:
      - $PWD:/app
    working_dir: /app

```
