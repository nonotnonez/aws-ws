---
title : "CodePipleline"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 8.4 </b> "
---

#### Overview

- CodePipeline is a continuous delivery tool, and it shows a visual workflow so we understand how to orchestrate our pipeline using it. You can find sources such as GitHub, CodeCommit, or Amazon S3, and then you can build using technologies such as CodeBuild, or Jenkins, and so on. 

|  CodeBuild |  |
|---|---|
|![84](/aws-ws/images/8-devopsaws/84/1.png?featherlight=false&width=40pc)  | ![84](/aws-ws/images/8-devopsaws/84/3.png?featherlight=false&width=50pc) |
|![84](/aws-ws/images/8-devopsaws/84/2.png?featherlight=false&width=50pc)  | ![84](/aws-ws/images/8-devopsaws/84/4.png?featherlight=false&width=50pc)  |


#### CodeCommit and CodeDeploy

- Develop Tools > CodePipeline > Pipelines > Create new pipeline
  -   Pipeline name:    **cicd-pipeline**
  -   Service role: New service role
      -   Role name: **AWSCodePipelineServiceRole-us-east-1-cicd-pipeline**
      -   Next
  -   Advanced settings
      -   Bucket:   **cicddevopsartifacts**
  -   Source: AWS CodeCommit
  -   Repository name:  cicd-repo
  -   Branch name: master
      -   Next
  -   Build provider: Skip step
      -   Next
  -   Deploy provider: AWS CodeDeploy
  -   Application name: CodeDeployDemo
  -   Deploymeny group: MyDevInstances
      -   Next - Create pipeline  

|  CodePipeline |  |
|---|---|
|![84](/aws-ws/images/8-devopsaws/84/5.png?featherlight=false&width=50pc)  | ![84](/aws-ws/images/8-devopsaws/84/6.png?featherlight=false&width=50pc) |
|![84](/aws-ws/images/8-devopsaws/84/7.png?featherlight=false&width=50pc)  |  |

- Edit and Comming index.html
- Check Pipelines is processing
![84](/aws-ws/images/8-devopsaws/84/8.png?featherlight=false&width=50pc) 

#### Add CodeBuild

- Editing: cicd-pipeline
  - Edit: Deploy - Add Stage - Stage name:  **Test**
    - Edit: Test
      - Add action group
        - Action name:  **TestforWelcome**
        - Action provider: AWS CodeBuild
        - Input artifacts:  SourceArtifact
        - Project name: DevOpsAppBuild
        - Output artifacts: TestResults
        -> DONE

![84](/aws-ws/images/8-devopsaws/84/9.png?featherlight=false&width=50pc) 
   - SAVE
   - Release change > Release

- CodeBuild check:
  - Build > Build projects > Build project

![84](/aws-ws/images/8-devopsaws/84/10.png?featherlight=false&width=50pc) 

- Check Permission
  - IAM - Roles - **codebuild-DevOpsAppBuild-service-role**
    - Edit policy
      - Resource: All resources
    
![84](/aws-ws/images/8-devopsaws/84/11.png?featherlight=false&width=40pc) 

- Run Codepile again

![84](/aws-ws/images/8-devopsaws/84/12.png?featherlight=false&width=40pc) 

|  Check Build history | Pipeline Deploy |
|---|---|
|![84](/aws-ws/images/8-devopsaws/84/13.png?featherlight=false&width=50pc)  | ![84](/aws-ws/images/8-devopsaws/84/14.png?featherlight=false&width=50pc) |

- Test Issue: Index.html
- Commit Changes
- Check Pipeline process
- Check build history 
  - Issue : "Welcome"

|  Edit index.htm | |
|---|---|
|![84](/aws-ws/images/8-devopsaws/84/15.png?featherlight=false&width=50pc)  | ![84](/aws-ws/images/8-devopsaws/84/16.png?featherlight=false&width=50pc) |

|  Pipeline | Build project|
|---|---|
|![84](/aws-ws/images/8-devopsaws/84/17.png?featherlight=false&width=50pc)  | |

#### Manual approval steps

- Pipeline: **cicd-pipeline** - Edit - Add stage > Stage name: **DeploytoProd**
  - Add action group > Action name: **DeplotToProd**
      - Add action group
        - Action name:  **manualApproval**
        - Action provider: Manual approval
        - SNS topic ARN - optional :
          - ...Default_CloudWatch_Alarms_Topic
        - URL for review - optional:    http://ec2-18-232-147-162.compute-1.amazonaws.com/
        - Comments - optional: Please review and Approve if Happy with the changes
        
        -> DONE
        -> Save -> Release change

|  Pipeline | Release |
|---|---|
|![84](/aws-ws/images/8-devopsaws/84/18.png?featherlight=false&width=50pc)  | ![84](/aws-ws/images/8-devopsaws/84/19.png?featherlight=false&width=50pc) |
|![84](/aws-ws/images/8-devopsaws/84/20.png?featherlight=false&width=50pc)  | ![84](/aws-ws/images/8-devopsaws/84/21.png?featherlight=false&width=50pc) |