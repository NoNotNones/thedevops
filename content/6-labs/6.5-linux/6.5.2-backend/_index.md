---
title : "Backend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.5.2 </b> "
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

![652](/thedevops/images/6-labs/6.5-linux/6.5.2-backend/1.png?featherlight=false&width=90pc)

Java: 

-   Checking require java version in pom.xml -> java.version=1.8


![652](/thedevops/images/6-labs/6.5-linux/6.5.2-backend/2.png?featherlight=false&width=90pc)

Install: 

````
apt install openjdk-17-jdk openjdk-17-jre
apt install maven
java -version
mvn -v
````

Configuring Database Spring boot: https://www.baeldung.com/spring-boot-configure-data-source-programmatic

- DataSource: **application.properties**


![652](/thedevops/images/6-labs/6.5-linux/6.5.2-backend/3.png?featherlight=false&width=90pc)

- Install Database: **MariaDB**

````
apt install mariadb-server
netstat -tlpun
````
![652](/thedevops/images/6-labs/6.5-linux/6.5.2-backend/4.png?featherlight=false&width=90pc)

{{% notice note %}}
Database should install in difference Server
{{% /notice %}}

- Configure DB:

````
systemctl stop mariadb
ls /etc/mysql/
````
nano /etc/mysql/mariadb.conf.d/50-server.cnf
````
bin-address = 0.0.0.0
````

````
systemctl restart mariadb
netstat -tlpun
````
![652](/thedevops/images/6-labs/6.5-linux/6.5.2-backend/6.png?featherlight=false&width=90pc)

- Create Database: import shoe_shopdb.sql to MariaDB

mysql -u root

- show databases: **show databases;**

![652](/thedevops/images/6-labs/6.5-linux/6.5.2-backend/7.png?featherlight=false&width=90pc)

- Create datatabse: **Database** and **User**
  - **user**: shoeshop
  - **@%**: can access all server
  - **identified**: password
  - **grant**: permission for **shoeshop** apply to all resource
  - **flush privileges**: save changes

````
show databases;
create database shoeshop;
create user 'shoeshop'@%'identified by 'shoeshop';
grant all privileges on shoeshop.* to  'shoeshop'@'%';
flush privileges;
exit
````

- Login user **shoeshop**: mysql -h 192.168.33.110 -P 3306 -u shoeshop
    - **h**: host
    - **p**: port
    - **u**: user

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