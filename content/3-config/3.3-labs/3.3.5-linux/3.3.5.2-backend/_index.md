---
title : "Backend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.3.5.2 </b> "
---

#### Project workflow
1. Tools
2. Edit & configure files
3. Install & configure Database
4. Build
5. Run
6. Check status

#### Configuration
Source: shoeshop-ecommerce.zip

User: shoeshop

Java project folder: /projects/shoeshop

1. Folder and User permisson:
````
adduser shoeshop
chown -R shoeshop. /projects/shoeshop/
chmod -R 750 /projects/shoeshop/
````

2. Requires tools: https://spring.io/guides/gs/maven

Maven: defined with an XML file named **pom.xml**

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.2-backend/1.png?featherlight=false&width=90pc)

Java: 

-   Checking require java version in pom.xml -> java.version=1.8


![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.2-backend/2.png?featherlight=false&width=90pc)

Install: 

````
apt install openjdk-17-jdk openjdk-17-jre
apt install maven
java -version
mnv -v
````

````
apt install mariadb-server
netstart -tlpun
````

Configuring Database Spring boot: Mariadb 

````
systemctl stop mariadb
````
nano /etc/mysql/mariadb.conf.d/50-server.conf
````
bin-address = 0.0.0.0
````
````
systemctl restart mariadb
````
mysq -u root
````
show databases;
create database shoeshop;
create user 'shoeshop'@%'identified by 'shoeshop';
grant all privileges on shoeshop.* to 'shoeshop'@'%';
flush privileges;
exit
````

mysq -h 192.168.33.110 -P 3306 -u shoeshop
````
show databases;
use shoeshop;
show tables;
source /projects/shoeshop/shoeshopdb.sql
show tables;
exit
````
vi /src/main/resources/application.properties
````
spring.datasource.url=jdbc:mysql://192.168.33.110:3306/shoeshop
spring.datasource.username=shoeshop
spring.datasource.password=shoeshop
````
mvn install -DskipTest=true

ls target/
````
java -jar target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar
````

````
nohup java -jar target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar 2>&1 &
````

tail -f nohup.out

1. Check: http://192.168.33.110:8080