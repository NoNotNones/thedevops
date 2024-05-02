---
title : "Workflow"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### Create a workflows
- Workflows define the event that triggers actions
- Workflows define with actions to run
- Repositories can contain multiple workflows

Workflows are stored in a directory name .github/workflows
````
mkdir -p .github/workflows
````
-p option will create both directories at the same time

Create a workflow: 
````
cd .github/workflows
vim first.yml
````
**first.yml**
````
name: first

on: push
````
Add jobs and steop to a workflow

````
name: first

on: push

jobs:
    job1:
        name: First Job
        runs-on: ubuntu-latest
        steps:
            - name: Step one
            - name: Step two
    job2:
        name: Second Job
        runs-on: windows-latest
        steps:
            - name: Step one
            - name: Step two
````
Adding an Actions
- uses: Excute an action in the operating system

![53](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/14.png?featherlight=false&width=30pc)

Adding a command
- Run: Excute commands in the operating system's shell
- Bash: Default shell for Ubuntu, macOS

![53](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/2.png?featherlight=false&width=30pc)

````
name: first

on: push

jobs:
    job1:
        name: First Job
        runs-on: ubuntu-latest
        steps:
            - name: Step one
              uses: actions/checkout@v2 #name &tag
            - name: Step two
              run: env | sort
    job2:
        name: Second Job
        runs-on: windows-latest
        steps:
            - name: Step one
              uses: actions/checkout@v2 #name &tag
            - name: Step two
              run: "Get-ChildItem Env: | Sort-Object Name"
````
Run a workflow
````
git add first.yml
git commit -m "First commit"
git push
````

### Content

1. [Jenkins CICD](3.3.1.1-cicd/)
2. [Advanced](3.3.1.2-advanced/)
