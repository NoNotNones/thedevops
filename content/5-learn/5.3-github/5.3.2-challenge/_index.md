---
title : "Challenge"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

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

