---
title : "Workflow"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.3.2.1 </b> "
---

#### Gitlab-Server

- Repo: **shoeshop**
- New branch:
    - Branch name: develop -> Create from: main
    - Branch name: feature/frontend/login -> Create from: develop
    - Branch name: feature/backend/login -> Create from: develop

- Set permission approve : http://gitlab.local.test/shoeshop/shoeshop/-/settings/repository
    - Protected branches :
      - Branch: 	Allowed to merge
        - develop	-  Maintainers  -   Maintainers
        - main	    -  Maintainers	-   **No one**

    - Default branch : develop (for Developer Teams)

- New branch:
    - Branch name: staging -> Create from: develop
    - Protected branches :
      - Branch: 	Allowed to merge
        - staging	-  Maintainers  -   Maintainers

#### Permission testing
- Edit repo: http://gitlab.local.test/shoeshop/shoeshop/-/new/develop/
  - User: tuanta
  - New file: config.txt
  - Commit: config(database): modify connection
  - Target branch: develop
    - -> OK
- Edit repo: http://gitlab.local.test/shoeshop/shoeshop/-/new/main/
  - User: tuanta
  - New file: config.txt
  - Commit: config(database): modify connection
     - ISSUE: -> You are not allowed to push into this branch