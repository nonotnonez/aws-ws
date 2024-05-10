---
title : "Git"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### GitHub 

GitHub is a web-based platform built on top of Git, the distributed version control system. It offers a variety of features to help developers collaborate on software projects

GitHub provides a platform for hosting Git repositories. Developers can create new repositories to store their code, either publicly (visible to everyone) or privately (accessible only to authorized collaborators)

GitHub Actions is a continuous integration tool that allows developers to automate tasks for their web projects. In this course, learn how to use this powerful tool to build workflows triggered by events, develop a continuous integration and continuous delivery (CI/CD) pipeline, and create custom actions.

- Create a repo :

![24](/aws-ws/images/2-prepair/2.4-git/1.png?featherlight=false&width=90pc)

- Create accesskey :
![24](/aws-ws/images/2-prepair/2.4-git/2.png?featherlight=false&width=90pc)
![24](/aws-ws/images/2-prepair/2.4-git/3.png?featherlight=false&width=90pc)

- Clone repo to local :

````sh
git clone https://<access token>U@github.com/nonotnonez/thegithub.git
````
![24](/aws-ws/images/2-prepair/2.4-git/4.png?featherlight=false&width=50pc)

- Create a README.md file :
![24](/aws-ws/images/2-prepair/2.4-git/5.png?featherlight=false&width=50pc)

- Copy Source and Push to git repo:
````
        git add .
        git commit -m 'first check in'
        git push
````
![24](/aws-ws/images/2-prepair/2.4-git/6.png?featherlight=false&width=50pc)
![24](/aws-ws/images/2-prepair/2.4-git/7.png?featherlight=false&width=90pc)

#### First action

Setup Github actions: https://github.com/nonotnonez/thegithub/actions/new
  - Choose: **Simple workflow** -> Configure
![24](/aws-ws/images/2-prepair/2.4-git/8.png?featherlight=false&width=50pc)

![24](/aws-ws/images/2-prepair/2.4-git/9.png?featherlight=false&width=50pc)

  - Change name **blank.yml** to **hello.yml** and Commit changes
  - Click **Actions** to see the results
![24](/aws-ws/images/2-prepair/2.4-git/10.png?featherlight=false&width=50pc)
![24](/aws-ws/images/2-prepair/2.4-git/11.png?featherlight=false&width=50pc)

Workflow and actions attributes

![24](/aws-ws/images/2-prepair/2.4-git/12.png?featherlight=false&width=50pc)
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

