---
title : "Frontend"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.3.5.1 </b> "
---

##### Overview
Vagrant and Oracle VM Vitualbox Manager

##### Configuration
Server name: **machine1** - IP: **192.168.33.100**

-   PS D:\LAB\Vagrant>

````
vagrant up machine1
vagrant ssh machine1
````
- Copy source to machine: 
````
scp -i d:\LAB\Vagrant\.vagrant\machines\machine1\virtualbox\private_key \LAB\Vagrant\datas\todolist.zip vagrant@192.168.33.100:/home/vagrant
````

- Setup:
````
sudo -i
apt update
apt install unzip
unzip todolist.zip
mkdir /projects
mv todolist /projects
ls -l /projects
````
- Add user for project:
````
adduser todolist
chown -R todolist:todolist /projects/todolist
chmod -R 750 /projects/todolist
````

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/1.png?featherlight=false&width=90pc)

#### Run Project: VUE
Source : 
-    https://vuejs.org/guide/quick-start
-    Prerequisites: Node.js - https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04

Install Aplication

````
apt update
apt install nodejs
apt install npm
````

Configure foler and user
````
sudo todolist
cd /projects/todolist
````
Review configure file: 
- package.json
````
{
  "name": "todolist",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",  
    "build": "vue-cli-service build"
  },
  "dependencies": {
    "bootstrap-vue": "^2.22.0",
    "core-js": "^3.8.3",
    "vue": "^2.6.14",
    "vue-router": "^3.5.1"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~5.0.0",
    "@vue/cli-plugin-router": "~5.0.0",
    "@vue/cli-service": "~5.0.0",
    "vue-template-compiler": "^2.6.14"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead"
  ]
}
````
- vue.config.js 
````
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true
})

module.exports = {
  devServer: {
    port: 3000
  },
};
````

Run project
````
npm install
npm run build
````

ISSUE: version
![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/2.png?featherlight=false&width=90pc)


Fix ISSUE: Update same version or latest
````
curl -s https://deb.nodesource.com/setup_18.x | sudo bash
sudo apt install nodejs -y
node -v
npm run build
````
{{% notice tip %}}
Project will build file or folder. In this project **VUE** will build to a folder is : **dist**
{{% /notice %}}

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/3.png?featherlight=false&width=90pc)

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/4-1.png?featherlight=false&width=90pc)

{{% notice tip %}}
We have 3 way to run a Fontend project: Webserver - Services - Pm2
{{% /notice %}}
````
npm run serve
````

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/4.png?featherlight=false&width=90pc)

Checking : http://192.168.33.100:3000/

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/5.png)

#### Run with Nginx Webserver
Install
````
apt install nginx -y
````
Check port
````
netstat -tlpun
````
![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/6.png?featherlight=false&width=50pc)

Configure: 

Change default nginx port to **8999**: cd /etc/nginx
- vi sites-avaiable/default
````
server {
        listen 8999 default_server;
        listen [::]:8999 default_server;
````
- Test and apply configure: **nginx -t** and **systemctl restart nginx**
![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/7.png?featherlight=false&width=90pc)
![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/8.png?featherlight=false&width=90pc)

Setup Project run Nginx port **8088**:

- Create configure file: vi **conf.d/todolist.conf**
````
server {
    listen 8088;
    root /projects/todolist/dist/;
    index index.html;
    try_files $uri $uri/ /index.html;
}
````
- ISSUE: User for nginx (vi /etc/nginx.conf -> user **www-data**)
![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/9.png?featherlight=false&width=90pc)

- Fix: add user (www-data) to group todolist
````
usermod -aG todolist www-data
````
- Apply configure: **systemctl restart nginx** or **nginx -s reload**

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/10.png?featherlight=false&width=90pc)

#### Projects React
Source: **vision.zip**

User: **vision**

Project folder: **/projects/vision**

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/11.png?featherlight=false&width=90pc)

Run application
-   npm install

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/12.png?featherlight=false&width=90pc)   


Run as services: vi **/lib/systemd/system/vision.service**
````
[Service]
Type=simple
User=vision
Restart=on-failure
WorkingDirectory=/projects/vision/
ExecStart=npm run start -- --port=3000
````

-   systemctl daemon-reload
-   systemctl start vision
-   systemctl status vision
![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/13.png?featherlight=false&width=90pc)

Check: http://192.168.33.100:3000

![3351](/cicd-ws/images/3-config/3.3-labs/3.3.5-linux/3.3.5.1-frontend/14.png?featherlight=false&width=90pc)