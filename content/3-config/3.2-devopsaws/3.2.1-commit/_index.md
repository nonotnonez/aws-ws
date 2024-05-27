---
title : "CodeCommit"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.2.1 </b> "
---

![321](/aws-ws/images/3-config/3.2-devopsaws/321/1.png?featherlight=false&width=50pc)

-  Version control is the ability to understand the various changes that happened to the code overtime (and possibly roll back)

1. Create Repo

Developer Tools > CodeCommit > Repository > Create repository

- Repository name:    **cicd-repo**

- IAM User
  - User name: **cicd-user**
    - Permission:
      - **AdministratorAccess**
  - Download Access & Secret key

- Credentials
    - User: cicd-user
      - Credentials : HTTPS Git credentials for AWS CodeCommit
      - Generate credentials

- Clone Repo
    - Repo: cicd-repo
      - Clone URL - Clone HTTPS
    - Ex: git clone https://git-commit.aws-east-1.amazonaws.com/v1/repos/cicd-repo

![321](/aws-ws/images/3-config/3.2-devopsaws/321/2.png?featherlight=false&width=50pc)  

2. Clone, add, commit, push

- Local 
  - Copy source code to repo
  - git status
  - git add .
  - git status
  - git commit -m "First Commit"
  - git push

- Check on AWS repo
    - Repositories
      - Code
      - Commits - master

3. Branching

- Local website check: index.html
  - Edit index.htm (v2)
  - git status
  - git add .
  - git commit -m "revised index to v2"
  - git push

- Check on AWS repo
  - Commits : review changes
  
![321](/aws-ws/images/3-config/3.2-devopsaws/321/4.png?featherlight=false&width=50pc) 
![321](/aws-ws/images/3-config/3.2-devopsaws/321/5.png?featherlight=false&width=50pc)   
![321](/aws-ws/images/3-config/3.2-devopsaws/321/3.png?featherlight=false&width=50pc)  


4. Branch merge pull request

- Create branch
  - git checkout -b feature1
  - edit index.html (v3)
  - git add .
  - git commit -m "revised index to v3"
  - git push
    ![321](/aws-ws/images/3-config/3.2-devopsaws/321/6.png?featherlight=false&width=50pc)  

- Check AWS repo
  - Branches
    ![321](/aws-ws/images/3-config/3.2-devopsaws/321/7.png?featherlight=false&width=50pc) 
  
  - Pull request 
    - Create pull request
      - Destination:    master
      - Source: feature1
      - Compare
      - Details
        - Title:   **mergefeature1**
        - Create pull request
    - Check > Changes (review changes)
    - Merge
      - Delete after merge
      - Merge pull request
  - Branches
  - Code:  check index.html