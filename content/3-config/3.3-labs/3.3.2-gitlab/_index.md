---
title : "Gitlabs"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.3.2 </b> "
---

#### Overview

GitLab is a web-based DevOps lifecycle tool that provides a Git repository manager providing wiki, issue-tracking, and CI/CD pipeline features, using an open-source license. It offers functionalities similar to GitHub but with additional features focused on the entire DevOps lifecycle.

#### Configuration
1. Installation
````linux
curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
sudo apt-get install gitlab-ee=14.4.1-ee.0
````
Hostname: 
- vi /etc/hosts
````
192.168.33.110 gitlab.local.test
````

- vi /etc/gitlab/gitlab.rb
````
external_url 'http://gitlab.local.test'
gitlab-ctl reconfigure
````
2.Login :  http://gitlab.local.test

User: **root** - Password: **cat /etc/gitlab/initial_root_password**

View settings: 
- `Uncheck`: Siginup enabled & Required admin ...
- Setting - CI/CD - Continuos Integration and Deployment - Expand : `Uncheck`: Default to Auto DevOps

Change password root user:
- User: **root** - Edit Profiles - Password

Create User:
- Menu - Admin 
    - New User: **tuanta**
      - Access level: **admin**
    - New User: **dev1**
      - Access level: regular

3. Project process:
##### Gitlab-Server:

- Menu - Group - Create Group - Create Group 
  - Group name: **shoeshop**
  - Private
  - Role: Devops Engineer

- New project:
  -  Project name: **shoeshop**
  - `Uncheck`: Initialize repository with a README
  - Invited members:
    - **tuanta**: maintainer
    - **dev1**: develop

- Git configure: **lab-server: 192.168.33.110**

##### Lab-Server
- Folder:
    - mkdir /data
    - cd /data
    - Git config:
        - git config --global user.name "tuanta"
        - git config --global user.email "tuanta@gitlocal.test"
    
    - Clone source: git clone http://gitlab.local.test/shoeshop/shoeshop.git
        - Enter user/passsword: tuanta
    
    - Copy Source Code and Add permission to user
        - cp -rf shoeshop/* /data/shoeshop/
        - cd /data/shoeshop

    - Upload to repo:
        - git status
        - git checkout -b main
        - git add .
        - git commit -m "feat(project): create base projects"
        - git push -f origin
        - git push --set-upstream origin main
          - Enter user/password: tuanta

4. CICD Apply
- Gitlab-server
  - Edit README.md
  - Commit: doc(sys desc): change system title

- Lab-server
  - Git pull
    d- Enter user/password: tuanta


### Content

1. [Workflow](3.3.2.1-workflow/)
2. [Runner](3.3.2.2-runner/)
3. [CICD](3.3.2.3-cicd/)