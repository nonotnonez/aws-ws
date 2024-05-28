---
title : "CodeBuild"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 8.2 </b> "
---

|  CodeBuild | Overview |
|---|---|
|![82](/aws-ws/images/8-devopsaws/82/0.png?featherlight=false&width=50pc) | ![82](/aws-ws/images/8-devopsaws/82/01.png?featherlight=false&width=50pc) |

1. Create and test your first build

- Developer Tools > CodeBuild > Create project
  - Project name:   **DevOpsAppBuild**
  - Source:
    - AWS CodeCommit
    - Repository: **cicd-repo**
    - Reference type: 
      - Branch: master
    - Environment:  Managed image
    - Service role: 
      - New service role - Role name:  coldebuild-DevOpsAppBuild-service-role
    - Operating system: Amazon Linux 2
    - Runtime(s): Standard
      - Image: aws/codebuild/amazonlinux2-x86_64-standard:3.0  
    - VPC
    - Compute
  - Buildspec
    - Use a buildspec file: **buildspec.yaml**
  - CloudWatch: enable

Build project - Start build

Check: Build (Codebuild) - Build history

 ![82](/aws-ws/images/8-devopsaws/82/1.png?featherlight=false&width=50pc) 

 **buildspec.yaml**
```sh
version: 0.2

phases: 
    install:
        runtime-versions:
            nodejs: 10
        commands:
            - echo "installing something"
    pre_build:
        commands: 
            - echo "This is the pre build phase"
    build:
        commands:
            - echo "This is the build phase"
            - echo "Here we can run some tests"
            - grep -Fq "Welcome" index.html
    post_build:
        commands:
            - echo "This is the post build phase"
            
artifacts:
  files:
    - '**/*'
  name: DevOpsAppArtifacts             
            
```

2. Environment variables and Parameter

- Ex: add command

```sh
version: 0.2

phases: 
    install:
        runtime-versions:
            nodejs: 10
        commands:
            - printenv
            - echo "installing something"

            ...
```

- Can add in Console 
 ![82](/aws-ws/images/8-devopsaws/82/3.png?featherlight=false&width=50pc) 

- Parameter
  - AWS Systems Manager > Parameters

 ![82](/aws-ws/images/8-devopsaws/82/2.png?featherlight=false&width=50pc) 
  - Add permission access
    - IAM - Role : coldebuild-DevOpsAppBuild-service-role
      - Attach policy: **AmazonSSMReadOnlyAccess**
- Rebuild to test (secure database password with SSM)
 ![82](/aws-ws/images/8-devopsaws/82/4.png?featherlight=false&width=50pc) 

 3. Artifact and S3

- S3 > Create bucket 
  - Name: **cicddevopsartifacts**

- Developer Tools > CodeBuild > Build projects > DevOpsAppBuild > Edit Artifacts
  - Type: Amazon S3
  - Bucket name: cicddevopsartifacts
  - Namespace type - *optional* : Build ID
  - Artifacts packing:  Zip
  - Update Artifacts
- Start build : Upload_Artifacts : Successded
- S3 check:
  ![82](/aws-ws/images/8-devopsaws/82/5.png?featherlight=false&width=50pc) 
- Download to local
  ![82](/aws-ws/images/8-devopsaws/82/6.png?featherlight=false&width=50pc) 