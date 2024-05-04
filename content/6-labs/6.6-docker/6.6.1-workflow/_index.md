---
title : "Workflow"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.6.1 </b> "
---

#### Overview

##### Docker backend:
1. Chọn image đúng version dự án
2. Chọn image từ các nguồn offical, verified, sponsored
3. Chọn base image là alpine (nhẹ)

Project folder: /data/shoeshop

1.Create Database:
- apt install mariadb-server
- systemctl stop mariadb
- apt install net-tools
- nano /etc/mysql/mariadb.conf.d/50-server.cnf
  - bin-address = 0.0.0.0
- systemctl restart mariadb
- netstat -tlpun

2.Database configuration:
- mysql -u root
  - show databases;
  - create database shoeshop;
  - create user 'shoeshop'@'%' identified by 'shoeshop';
  - grant all privileges on shoeshop.* to  'shoeshop'@'%';
  - flush privileges;
  - exit

- mysql -h 192.168.33.100 -P 3306 -u shoeshop -p
  - Enter password:	shoeshop
  - show databases;	
  - use shoeshop;	
  - show tables;	
  - source /data/shoeshop/shoe_shopdb.sql	
  - show tables;	
  - exit	

- nano src/main/resources/application.properties
  - spring.datasource.url=jdbc:mysql://192.168.33.100:3306/shoeshop
  - spring.datasource.username=shoeshop
  - spring.datasource.password=shoeshop

- vi **Dockerfile**

````sh
## build stage ##
FROM maven:3.5.3-jdk-8-alpine as build
WORKDIR /app
COPY . .
RUN mvn install -DskipTest=true

## run stage ##
FROM amazoncorretto:8u402-alpine-jre
WORKDIR /run
COPY --from=build /app/target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar
EXPOSE 8080
ENTRYPOINT java -jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar
````
3.Run Project:
  - docker build -t shoeshop:v1 .
  - docker run --name shoeshop -dp 8899:8080 shoeshop:v1
  - docker images
  - docker ps -a

Check: http://192.168.33.100:8899

4.Advanced: nano **Dockerfile-v2**

````sh
"## build stage ##
FROM maven:3.5.3-jdk-8-alpine as build

WORKDIR /app
COPY . .
RUN mvn install -DskipTest=true


## run stage ##
FROM alpine:3.19

RUN apk add openjdk8

WORKDIR /run
COPY --from=build /app/target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar

EXPOSE 8080

ENTRYPOINT java -jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar"
````

5.Advanced: User Permissons

User: Shoeshop
  - adduser shoeshop
  - chown -R shoeshop. /data/shoeshop/
  - chmod -R 750 /data/shoeshop/

**Dockerfile-v3**

````sh
"## build stage ##
FROM maven:3.5.3-jdk-8-alpine as build

WORKDIR /app
COPY . .
RUN mvn install -DskipTest=true


## run stage ##
FROM alpine:3.19

RUN adduser -D shoeshop

RUN apk add openjdk8

WORKDIR /run
COPY --from=build /app/target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar

RUN chown -R shoeshop:shoeshop /run

USER shoeshop

EXPOSE 8080

ENTRYPOINT java -jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar"

````

Run project:
- docker build -t shoeshop:v3 -f Dockerfile-v3 .
- docker run --name shoeshop-v3 -dp 7777:8080 shoeshop:v3
- docker logs shoeshop-v3
- docker exec -it shoeshop-v3 sh
  - ls -l
  - ps -ef| grep shoe
  
##### Docker Frotend

1.Project folder:
  - mkdir /data/todolist
  - apt insall unzip
  - unzip todolist.zip
  - cdp -rf todolist/* /data/todolist
  - cd /data/todolist

2. Insall npm
  - sudo apt install npm -y

3. Create Dockerfile

````sh
## build stage ##
FROM node:18.18-alpine as build

WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

## run stage ##
FROM nginx:alpine

RUN apk add openjdk8

WORKDIR /run
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 8080

CMD ["nginx", "-g",  "daemon off;"]
````

4. Run Project
- docker build -t todolist:v1 .
- docker run --name todolist-v1 -dp 6868:80 todolist:v1
- http://localhost:6868

##### Docker Volume, Compose, Network
1. Docker Volume
   - docker run --rm -v 'pwd':/app --workdir="/app" mnv install -DskipTests=true
   - mkdir -p /db/mariadb-1
   - docker run -v /db/mariadb-1/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -p 3307:3306 --name mariadb-1 -d mariadb:10.6
   - docker ps
   - netstat -tlpn
   - mysql -h 192.168.1.110 -P 3307 -u root -p
     - MariaDB [(none)]>
     - show databases;
     - create database demo
     - show databases;
     - exit
   - ls /db/mariadb-1/
   - Delete container images and run docker again
   
2. Docker Compose
   - vi docker-compose.yml

````sh
version: '3.8'
services:
    db1:
          image: mariadb:10.6
          volumes:
               - /db/mariadb-1:/var/lib/mysql
          environment:
               MYSQL_ROOT_PASSWORD: root
          ports:
              - "3307:3306"
          container_name: mariadb-1
          restart: always
    app-backend:
          image: shoeshop:v3
          ports:
              - "8081:8080"
          container_name: shoeshop-1
          restart: always

````

  - docker-compose up -d
  - docker-compose ps

3. Docker Network

- docker inspect shoeshop-1
- docker exec -it shoeshop-1 sh
  - whoami
- docker exec -it mariadb-1 sh
  - whoami 
  - apt update
  - apt install iputils-ping -y
  - ping 172.18.0.3

##### Docker Registry

