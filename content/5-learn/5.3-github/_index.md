---
title : "GitHub"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

[Home](./_index.md) | [Workflow](./5.3.1-workflow/_index.md) | [Challenge](./5.3.2-challenge/_index.md) | [Advanced](./5.3.3-advanced/_index.md) | [Pages](./5.3.4-pages/_index.md)

### Learning GitHub Actions
- [Source: Linkedin Learning](https://www.linkedin.com/learning/learning-github-actions-2/automating-with-github-actions-2?u=103729754) 
- GitHub Actions is a continuous integration tool that allows developers to automate tasks for their web projects. In this course, learn how to use this powerful tool to build workflows triggered by events, develop a continuous integration and continuous delivery (CI/CD) pipeline, and create custom actions.
- What you should know:
  - Github
    - Github account
    - Create a repository on GitHub.com
    - Create and manage a repository branch
    - Commit code changes to a repositoy
    - Work with pull requests
  - YAML
    - Working with YAML
  - Docker and Scripting
    - Use a Dockerfile to create containers
    - Shell scripting
    - Scripting a higher-level language

#### Introduction
Create a repo 

![53](/thedevops/images/5-learn/5.3-github/1.png?featherlight=false&width=90pc)

Create accesskey:
![53](/thedevops/images/5-learn/5.3-github/2.png?featherlight=false&width=90pc)
![53](/thedevops/images/5-learn/5.3-github/3.png?featherlight=false&width=90pc)

Clone repo to local:

````sh
git clone https://<access token>U@github.com/nonotnonez/thegithub.git
````
![53](/thedevops/images/5-learn/5.3-github/4.png?featherlight=false&width=50pc)

Create a README.md file 
![53](/thedevops/images/5-learn/5.3-github/5.png?featherlight=false&width=30pc)

Copy Source and Push to git repo:
````
        git add .
        git commit -m 'first check in'
        git push
````
![53](/thedevops/images/5-learn/5.3-github/6.png?featherlight=false&width=30pc)
![53](/thedevops/images/5-learn/5.3-github/7.png?featherlight=false&width=50pc)

#### Your first action
Setup Github actions: https://github.com/nonotnonez/thegithub/actions/new
  - Simple workflow -> **Configure**
![53](/thedevops/images/5-learn/5.3-github/8.png?featherlight=false&width=50pc)

![53](/thedevops/images/5-learn/5.3-github/9.png?featherlight=false&width=50pc)

  - Change name **blank.yml** to **hello.yml** and Commit changes
  - Click Actions to see the results
![53](/thedevops/images/5-learn/5.3-github/10.png?featherlight=false&width=90pc)
![53](/thedevops/images/5-learn/5.3-github/11.png?featherlight=false&width=90pc)

Workflow and actions attributes

![53](/thedevops/images/5-learn/5.3-github/12.png?featherlight=false&width=30pc)
- name:
  - The name of the workflow
  - Not required
- on:
  - The Github event that triggers the workflow
  - Required
- On Events:
  - Repository events
  - push
  - pull_request
  - release
  - **Webhooks**
    - branch creation
    - issues
    - members
  - Scheduled
    - Cron format 
- Jobs:
  - Workflows must have at least one job
  - Each job must have a identifier
  - Must start with a letter or underscore
  - Can only contain alphanumeric character,-,or_
- runs-on:
  - The type of machine needed to run the job
- Runners:
  - Windows Server 2019
  - Ubuntu 18.04
  - macOS Catalina 10.15
  - Self-hosted runners
- steps
  - List of actions or commands
  - Access to the file system 
  - Each step runs in its own process
- uses
  - Identifies an action to use
  - Defines the location of that action 
- run
  - runs commands in the vitual environment's shell
- name
  - an optional identifier for the step