---
title : "Git"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

Git is a distributed version control system (DVCS) that helps developers track changes to source code during software development. It allows multiple developers to collaborate on projects simultaneously.

#### Overview
##### Github
- GitHub is a web-based platform built on top of Git, the distributed version control system. It offers a variety of features to help developers collaborate on software projects
- GitHub provides a platform for hosting Git repositories. Developers can create new repositories to store their code, either publicly (visible to everyone) or privately (accessible only to authorized collaborators)

#### Configuration
**Create Github Repository and Access**
- Create a public repo: https://github.com/nonotnonez/ws-0001
- Create Github Access Key : https://github.com/settings/tokens
  - Name: **github_token_ws**
  - Expiration:	90 days
  - Select scopes:
    - repo
    - workflow
    
- Clone Source form Github:
    git clone https://**token**@github.com/NoNotNonez/ws-0001.git
- Copy Source code to Github Repo:
    - cd /ws-0001/terraform
- Create **.gitignore**: 
  - to security and prevent important file upload to github
![24](/cicd-ws/images/2-prepair/2.4-git/1.png)

- Push Source code to Git Repo:
    - git status
    - git add .
    - git commit -m "Add Tf source"
    - git push 
![24](/cicd-ws/images/2-prepair/2.4-git/2.png)


