---
title : "Runner"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.3.2.2 </b> "
---

GitLab Runner is a lightweight agent that runs jobs defined in your GitLab CI/CD pipelines. It works by listening for jobs that need to be executed and then runs them in a specified environment.

#### Lab-Server
- Install
````
sudo apt-get update
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
sudo apt install gitlab-runner
gitlab-runner -v
````

- Edit hostname: vi /etc/hosts
````
gitlab.local.test <ip>
````

- Configuration:

```
gitlab-runner register
```
```
Enter the GitLab instance: http://gitlab.local.test/
Enter the registration token:
Enter a description for the runner: lab-server
Enter tags for the runner: lab-server
Enter optional maintenance note for the runner: <enter>

executor: shell
```
vi /etc/gitlab-runner/config.toml

````
concurrent = 4 -> use 4 robot runner
````

- Start Runner
````
nohup gitlab-runner run --working-directory /home/gitlab-runner/ --config /etc/gitlab-runner/config.toml --service gitlab-runner --user gitlab-runner 2>&1 &

ps -ef | grep gitlab-runner
````

#### Gitlab-Server
-  CICD Settings: http://gitlab.local.test/shoeshop/shoeshop/-/settings/ci_cd
   -  Edit: http://gitlab.local.test/shoeshop/shoeshop/-/runners/2/edit
      -  `Uncheck` : When a runner is locked ....
      -  Branch: **develop** - Allowed to push: **Maintainers**

- Create CICD file: **.gitlab-ci.yml**  & Commit: Configure(pipeline): add runner
````
stages:
    - build
    - deploy
    - checking

build:
    stage: build
    script:
        - whoami
        - pwd
        - ls
    tags:
        - jenkins-server
````

#### Checking
- Gitlab-Server:
  - Jobs

- Lab-Server:
  - Folder: -> /home/gitlab-runner/builds/1fijYgCS/0/shoeshop/shoeshop
  