---
title : "Challenge"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Chalenge: *Develop a CI/CD pipeline*
- Create a mew repo and add the exercise files
- Edit the workflow to place the jobs in order
- Configure three environments:
  - Development
  - Staging 
  - Production
- Use continuos deployment for the devlopment and staging environments
- Protect deployments to the production environment with a review
- Update the summary to indicate all jobs

**Solution**

[Timelines & Get files](https://github.com/LinkedInLearning/github-actions-for-ci-cd-4375061/tree/main/Ch04/04_08_solution_develop_a_deployment_pipeline)

#### Chalenge: *Develop a container image*
Repo 1:
- Reuseable Python intefraton workflow

Repo 2:
- Add the code for the API
- Use the integration workflow from Repo 1
- Build and publish a container image

**Solution**

[Timelines & Get files](https://github.com/LinkedInLearning/github-actions-for-ci-cd-4375061/tree/main/Ch03/03_06_solution_develop_a_container_image_pipeline)





#### Challenge: [*Develop a CI/CD workflow*](https://www.linkedin.com/learning/github-actions-for-ci-cd/solution-develop-a-ci-workflow?autoSkip=true&resume=false&u=103729754)

User a Stater Workflow
  - Create a mew repo and add the exercise files
  - Create a starter workflow
  - Run the workflow and review the output
  - Fix any errors in the code base
  - Add a test report summary

**Solution**

[Timelines & Get files](https://github.com/LinkedInLearning/github-actions-for-ci-cd-4375061/tree/main/Ch02/02_07_solution_develop_a_ci_pipeline)

test_pandas_version.py

requirements.txt

- Actions - Choose a workflow - Python application: **python-app.yml** -> Commit changes

- Check Acctions -> ISSUE

![531](/thedevops/images/5-learn/5.3-github/5.3.2-challenge/4.png?featherlight=false&width=50pc)

- Fix: **requirements.txt**: =pandas==1.5.3
- Update the workflow to add a summary of the tests being run.

....-> follow timelines :)

- Result:
![531](/thedevops/images/5-learn/5.3-github/5.3.2-challenge/5.png?featherlight=false&width=50pc)





#### Challenge: *Configure a self-hosted runner with a lable* 
- Private repository
- Add a new runner to the repository
- Configure the runner with the label **project-alpha**
- Run a job with the **project-alpha** label
- Use a cloud-based virtual machine if possible
- Consider using a free tier for access to resources

#### Challenge: *Public and Use a Container image in a workflow*
- Create a new repo
- Add the exercise files
- Use the workflow suggested by Github Actions
- Add a workflow dishpath trigger
- Add a new job to run the test
- Run the image and check ouput
  - **docker run ghcr.io/repo/image:main**
- Test for the word "container" in the output
  - Use a CLI tool like **grep**

#### Challenge: *Use a matrix strategy to test an application*
- Create a new repo
- Add the application files
- Add the workflow file
- Update the workflow to use a matrix strategy
- Add matrix keys and values:
  - Platform: Ubuntu, macOS, Windows
  - Version: "3.10"."3.9","3.8","3.7"
- Update the runs-on and setup-python configurations
- Add an include block:
  - Python version "3.6"
  - ubuntu-latest
- Add an exclude block:
  - Python version "3.10"
  - windows-latest


#### Challenge: *Create a custom action*
Custom Action:
- Dockerfile
- Script

Workflow:
- Trigger on push events
- Use the custom action

**Solution**:
- Create *Dockerfile*

````
FROM ubuntu
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
````

- Create **entrypoint.sh**

````
#!/bin/bash
echo "This is my custom action! :D"
````

- Create a workflow: **.github/workflows/custom.yml**

````
name: Custom
on: push
jobs:
  main:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: nonotnonez/thegithub@main
````

- Check Actions:
![531](/thedevops/images/5-learn/5.3-github/5.3.2-challenge/3.png?featherlight=false&width=90pc)


#### Challenge: *Develop a CI/CD pipeline for a Python Script*

Job 1: Test
- Check out the repo
- Run the script:python hello.py

Job 2: Build
- Depends on Job 1
- Check out the repo
- Create an artifact

**Solution**:
- Create repo: **pipeline**
  - add a README file

- Create python file: **hello.py**

````sh
print("hello, world")
````

- Add a workflow file: .github/workflows/pipeline.yml

````sh
name: Python Pipeline

on: push

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - run: python hello.py
  build:
    needs: [test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/upload-artifact@v2
        with:
          name: hello
          path: .

````

![531](/thedevops/images/5-learn/5.3-github/5.3.2-challenge/2.png?featherlight=false&width=50pc)

#### Challenge: *Develop a workflow that creates an artifact*

- Workflow file in a new repo
- Push trigger
- Environment variable for the artifact name
- One job with two steps
  - actions/checkout
  - actions/upload-artifact
  
**Solution**:
- Create repo: **artifact**
  - add a README file
- Add a workflow file: .github/workflows/artifact.yml

````
name: Artifact

on: [push]

env:
  ARTIFACT_NAME:  myartifact

jobs:
  main:
    runs-on: ubuntu-latest
    steps:
      - name: Check out the code
        uses: actions/checkout@v2
      - name: Upload the artifact
        uses: actions/upload-artifact@v2
        with:
          name: ${{ env.ARTIFACT_NAME }}
          path: .
````

- Commit a new file
- Checking Actions - Artifact file

![531](/thedevops/images/5-learn/5.3-github/5.3.2-challenge/1.png?featherlight=false&width=40pc)

### Challenge: *Develop a complex workflow*
- Push trigger
- Multiple runners
- Job dependencies
- Workflow file in a repo
- Four jobs
- Print the date

  - Job 1- ubuntu-latest
  - Job 2- windows-latest
  - Job 3- macos-latest
  - Job 4- depends on jobs 1, 2, and 3

**Solution**

1.Create new repo: **complex-workflow**

2.Create a new fie: .github/workflows/**complex.yml**
````
name: Complex

on: push

jobs:
  ubuntu:
    runs-on: ubuntu-latest
    steps:
      - run: date
  windows:
    runs-on: windows-latest
    steps:
      - run: date
  macos:
    runs-on: macos-latest
    steps:
      - run: date
  depends:
    needs: [ubuntu, windows, macos]
    runs-on: macos-latest
    steps:
      - run: date
````
3.Commit file

4.Actions check: - All workflows - complex

![531](/thedevops/images/5-learn/5.3-github/5.3.1-workflow/7.png?featherlight=false&width=50pc)


