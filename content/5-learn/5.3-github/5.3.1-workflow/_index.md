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
![53](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/15.png?featherlight=false&width=50pc)

Adding Dependencies
- needs: Identifies one or more jobs that must complete successfully before a job will run

![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/4.png?featherlight=false&width=40pc)

Add conditionals
![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/5.png?featherlight=false&width=40pc)

Workflow and Action Limitations
- 1000 API requests per hour
- Acction cant trigger other workflows
- Logs are limited to 64KB
- Exceeding limits can cause
  - Job queueing
  - Failed jobs
- Limits are subject to change

![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/6.png?featherlight=false&width=40pc)

#### Using Actions

Use an action from the Marketplace
- Maketplace : Python Syntax Checker (copy & paste to .yml file)

- Python file: **100daysofcode.py**
````py
from datetime import date

start = date(2020, 1, 1)
today = date.today()
delta = today - start

if (delta.days < 101):
    print("Today is day {}".format(delta.days))
else:
    print('100 Days of Code sprint has ended')
````

- Github action file: **syntax-check.yml**
````sh
name: Python application

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Check out the code
      uses: actions/checkout@v2
    - name: Python Syntax Checker
      uses: cclauss/Find-Python-syntax-errors-action@v0.2.0

````
- Workflow:
![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/8.png?featherlight=false&width=50pc)

Use an action from a repository
- Actions in the workflow's repo
- Actions in any public repository
- Docker images from an image registry
- Use an Action in the same Repo
  - Specify any path relative to the repository root
````sh
steps:
      - uses: "./.github/action1"
````

- Use an Action in a Different repo
  - Specify the repo  owner's user ID, the repo, and a reference
````sh
steps:
      - uses: "user/reoi@ref"
````

  - Reference can be a branch, tag, or SHA
````sh
steps:
      - uses: "user/repo/path@ref"
````

````sh
steps:
      - name: Octocat's Cool Action
    uses: "octocat/my-cool-action@develop"
````

Use an Action from an Image Repo

  - Specify the "docker://" path to the image and tag
````
steps:
  - uses: "docker://image:tag"
````

  - Docker Hub is used by defacult: https://hub.docker.com
````
steps:
  - uses: "docker://python:3.9"
````
````
steps:
  - uses: "docker://host/image:tag"
````

Passing Arguments to an Action
- Steps use the attribute to pass arguments

- Create a new block for mapping arguments to unputs 
````
uses: {github account}/{action name}
with:
        key:value
        key:value
````
````
steps:
      - name: Checkout the code
        uses: actions/checkout@v2
        with:
            repository: apache/tomcat
            ref: master
            path: ./tomcat
````

Using environment variables
- Dynamimc key value pairs stored in memory
- Injected at runtime
- Case sensitive

![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/9.png?featherlight=false&width=50pc)

- Defining Environment Variables
  - Use the env attribute
  - Defined in:
    - Workflows
    - Jobs
    - Steps
    
![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/10.png?featherlight=false&width=40pc)

  - Accessing Environment Variables
    - Shell variable syntax
      - Bash (linux/macOS)
        - $VARIABLE_NAME
      - PowerShell (Windows) 
        - $Env:VARIABLE_NAME
         
      Variable is read from the shell

    - YAML syntax
      - ${{ env.VARIABLE_NAME }}

      Variable is read from the shell
      Variable can be used in workflow configuration

Using Secrets
- Stored as encrypted environment variables
- Cant be viewed or edited
- Workflows can have up to 100 secrets
- Secrets limited to 64kb
- Larger secrets can be encrypted and stored in the repo

Accessing Secrets
- Use the secrets workflow context
- ${{ secretts.SECRET_NAME }}
- Must be explicity passed to a step or action

**secret.yml**

````sh
name: secrets

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}
    - name: List S3 Buckets
      run: aws s3api list-buckets
````

![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/11.png?featherlight=false&width=40pc)

![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/12.png?featherlight=false&width=40pc)

Using **artifacts**
- Data preserved from a workflow
- Files or collection of files
  - Compiled binaries
  - Archives
  - Test results
  - Log files
- Pass data between workflow jobs
  - Job 1 - Create and upload artifact
  - Job 2 
    - Wait for Job 1 to complete 
    - Download and use artifact
- Can only be uploaded by a workflow
  - actions/upload-artifact
- Can only downloaded by the uploading workflow
- Manual downloads
- Free accounts get 500MB for storage
- Stored for 90 days
- For pull requests, retention period resets on pushes

  [artifact.yml](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/script/artifact.txt)

  [hello-server.go](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/script/hello-server.go)

  [test.sh](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/script/test.sh)

Manage pull requests
- Pull requests
  - Pull facilitated conversations around code
  - Merge code from one branch to another branch
  - Merge code from a cloned repo into the original repo
  - Allows for code reviews
- Automatin Pull-Request Merging
  - Automatic approve and merge PRs based on criteria
  - Run automated tests to check the code in the PR
  - Check the username that submitted the PR
  - Approve and merge the PR
  - Delete the branch associated with the PR

#### *Developing a CI/CD Workflow*
Continous Integration
- Find and resolve problems early
- Work locally and then commit to repo

Continous Delivery and Deployment
- Compiled into artifacts
- Stored
- Additional tests
- Ready for deployment
- Deploy to live environments


![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/16.png?featherlight=false&width=40pc)

[Lab](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/script/04_01)

#### Content

1. [Jenkins CICD](3.3.1.1-cicd/)
2. [Advanced](3.3.1.2-advanced/)
