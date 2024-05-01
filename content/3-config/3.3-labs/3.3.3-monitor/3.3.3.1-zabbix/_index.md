---
title : "Zabbix"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.3.3.1 </b> "
---

#### Overview
- Zabbix is an open-source monitoring software tool designed to track the status of various network services, servers, and other network hardware.
- It provides real-time monitoring, alerting, and visualization of metrics such as CPU load, network utilization, disk space, and more.
![3331](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.1-zabbix/1.png?featherlight=false&width=90pc)

#### Configuration
1. Zabbix Server Configuration: collects and processes data
- Install Repo
````shell
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu20.04_all.deb
dpkg -i zabbix-release_6.4-1+ubuntu20.04_all.deb
apt update -y

````
- Install Zabbix server, frontend, agent
````sh
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y
````
- Create initial database
````sh
apt install software-properties-common -y
apt update -y
apt install -y mariadb-server-10.6 mariadb-client-10.6 mariadb-common
systemctl start mariadb
systemctl status mariadb
````
- Configuration Database : **mysql -u root**
````sh
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
 quit;
````
````sh
cat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
mysql -u root
set global log_bin_trust_function_creators = 0;
exit
````
-  Configure the db for Zabbix server
````sh
vi /etc/zabbix/zabbix_server.conf
DBPassword=password
````
- Start Zabbix server and agent processes
````sh
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
````

Zabbix Agent Configuration: run on the monitored devices and send data to the server
- Installation
````sh
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu20.04_all.deb
dpkg -i zabbix-release_6.4-1+ubuntu20.04_all.deb
apt update -y
apt install zabbix-agent
````
- Configuration : 
vi /etc/zabbix/zabbix_agentd.conf
````sh
Server=<zabbix dns>
ServerActive=<zabbix dns>
Hostname=<zabbix server ip>
````

````sh
systemctl restart zabbix-agent
````

Add Host to Zabbix Server
- Login
- Add Host
    - Host - Create host
    ````sh
    Host name:	<Agent + IP>
    Templates:	Linux by Zabbix agent
    Host groups: 	Linux servers
    Interfaces: Agent - IP: <Agent IP>
    -> Add
    ````
Create Item & Trigger
- Item:
````sh
Name:	App service on server <Host Agent IP>. running on port 8080 is unavaiilable		
Type:	Zabbix agent		
Key:	net.tcp.listen[8080]	-> Select	net.tcp.listen[port]
Type of information:	Numeric (unsigned)		
Host interface:	<agent IP> :10050		
Update interval	10s		

-> TEST -> OK
````
- Trigger:
````sh
Name:	App service on server 192.168.1.110. running on port 8080 is unavaiilable
Severity:	High
Expresstion:	Item: App service on server <Host Agent IP>. running on port 8080 is unavaiilable
	Result = 0
	-> Add
````

Dashboard Overview:

![3331](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.1-zabbix/4.png?featherlight=false&width=90pc)
![3331](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.1-zabbix/3.png?featherlight=false&width=90pc)
![3331](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.1-zabbix/2.png?featherlight=false&width=90pc)

2. HiKVision APIs for Monitoring:
- Overview: 
Monitoring Hikvision devices using Zabbix involves leveraging Zabbix's capabilities to communicate with Hikvision APIs.

![3331](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.1-zabbix/11.png?featherlight=false&width=90pc)
![3331](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.1-zabbix/12.png?featherlight=false&width=90pc)
![3331](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.1-zabbix/13.png?featherlight=false&width=90pc)