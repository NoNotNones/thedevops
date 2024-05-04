---
title : "Docker"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6.6 </b> "
---

#### Install

Project folder for tools:
- mkdir /tools
- cd /tools/
- mkdir docker
- cd docker
- vi **install-docker.sh**
- chmod +x install-docker.sh
- ./install-docker.sh

````sh
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
````

#### Run APP

Ubuntu
- docker pull ubuntu:22.04
- docker run --name ubuntu -it  ubuntu:22.04
- docker ps -a
- docker start 6e7
- docker exec -it ubuntu bash

Nginx
- docker run --name nginx -p 9999:80 nginx
- Check: http://192.168.33.100:9999

#### Clean
- docker rm -f $(docker ps -aq)

#### Dockerfiles
Overview:
Dockerfiles để làm gì:
  - Đưa source code vào container
  - Cài đặt công cụ để chạy dự án

Dockerfile command:
  - FROM
  - WORKDIR
  - COPY
  - RUN
  - ENV
  - EXPOSE 80
  - CMD
  - ENTRYPOINT

Tư duy viết Dockerfile tối ưu:
  - Non root user
  - Base image
  - Multipe stage

### Content

1. [Workflow](6.6.1-workflow/)
2. [Advanced](6.6.2-advanced/)
