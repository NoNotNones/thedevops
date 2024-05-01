---
title : "Linux"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 3.3.5 </b> "
---

Project:
-   Frontend: Vuejs
-   Backend: Shoeshop

Source: 
````
shoeshop-ecommerce.zip
todolist.zip
vision.zip
````
vi /etc/hosts
````
machine1	lab-server	192.168.33.100   lab.local.test
machine2	gitlab-server	192.168.33.110   gitlab.local.test
machine3	monitor-server	192.168.33.120   monitor.local.test
````
User & Working folder:
````
/projects
````

````
adduser todolist
chown -R todolist:todolist /projects/todolist
chmod -R 750 /projects/todolist
````
{{% notice note %}}
Project process :
-   1- Tools
-   2- Configuration files
-   3- Steps: build & run
{{% /notice %}}

.

{{% notice tip %}}
Security:  separate folder and user for each project
{{% /notice %}}
    
### Content

1. [Frontend](3.3.5.1-frontend/)
1. [Backend](3.3.5.1-backend/)