---
title : "CodeDeploy"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 8.3 </b> "
---

Updating ....

|  CodeDeploy | Overview |
|---|---|
|![83](/aws-ws/images/8-devopsaws/83/1.png?featherlight=false&width=50pc)  | ![83](/aws-ws/images/8-devopsaws/83/2.png?featherlight=false&width=50pc) |
|![83](/aws-ws/images/8-devopsaws/83/3.png?featherlight=false&width=50pc)  | . |

1. Instance setup

- IAM Role:  
  - Role name:  **EC2RoleforCodeDeploy**
  - Permission: AmazonS3ReadOnlyAccess
  
- EC2 - Launch Instances
  - Amazon Linux 2 AMI
  - Type: t2.micro
  - VPC:
  - IAM Role: EC2RoleforCodeDeploy
  - Key pair: virginiakeypair
  
- SSH to EC2 instance (Use PuTTy & Keypair)

- Install CodeDeploy agent

{{%expand "CodeDeployConfig.md" %}}
```sh
# Installing the CodeDeploy agent on EC2
sudo yum update -y
sudo yum install -y ruby wget
wget https://aws-codedeploy-eu-west-1.s3.eu-west-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent status

# create a bucket and enable versioning
aws s3 mb s3://aws-devops-cicddemo --region us-east-1 --profile aws-devops
aws s3api put-bucket-versioning --bucket aws-devops-cicddemo --versioning-configuration Status=Enabled --region us-east-1 --profile aws-devops

# deploy the files into S3
aws deploy push --application-name CodeDeployDemo --s3-location s3://aws-devops-cicddemo/codedeploy-demo/app.zip --ignore-hidden-files --region us-east-1 --profile aws-devops

```
{{% /expand%}}

![83](/aws-ws/images/8-devopsaws/83/4.png?featherlight=false&width=50pc)

- Add Tag to EC2 instance
  - Key:    Name   - Value:  webserver
  - Key:    Environment   - Value:  Development

2. First deployment

- IAM Role - AWS Services - CodeDeploy
  - Role name: **CodeDeployRole**   
  
- IAM User: (for AWS CLI)
  - Create Access key ID & Secret access key

- AWS CLI configure:
  - aws configure --profile aws-devops
    - AWS Access Key ID:
    - AWS Secret Access Key: 
    - Default region name: json

- Create S3 buccket and enable versioning : **aws-devops-cicddemo**
  - configure in CodeDeployConfig.md

- Up load files into S3: **app.zip**

![83](/aws-ws/images/8-devopsaws/83/6.png?featherlight=false&width=50pc)


Developer Tools > CodeDeploy > Applications > Create application

- Application name: **CodeDeployDemo**
- Code platform: EC2/On-premises
  - Deployment groups: Create deployment group
    - Deployment group name:    **MyDevInstances**
    - Service role: /CodeDeployDemo
    - Deployment type: In-place
    - Environment configuration
      - Amazon EC2 instances
        - Key: Environment  - Value: Development
        
      ![83](/aws-ws/images/8-devopsaws/83/5.png?featherlight=false&width=50pc)
        - Create deployment group
    - Create deployment
      - Revision type:  My application is stored in Amazon S3
      - Revision location: ..../app.zip
        - Create Deployment

      ![83](/aws-ws/images/8-devopsaws/83/7.png?featherlight=false&width=50pc)

- SSH to EC2 instance
  - cd /var/www/htmll
  - ls
    - **index.html**

- EC2 instance check Security Group
  - Inbound rules:
    - HTTP / SSH       80/22        0.0.0.0/0

- Check via DNS
       ![83](/aws-ws/images/8-devopsaws/83/8.png?featherlight=false&width=50pc)


3. Configurations

- EC2 - Launch Instances:
  - Number of instances: 4
  - Amazon Linux 2 AMI
  - Type: t2.micro
  - VPC:
  - IAM Role: EC2RoleforCodeDeploy
  - User data:
  
  ```sh
    #!bin/bash
    sudo yum update -y
    sudo yum install -y ruby wget
    wget https://aws-codedeploy-eu-west-1.s3.eu-west-1.amazonaws.com/latest/install
    chmod +x ./install
    sudo ./install auto
    sudo service codedeploy-agent status
  ```

  - Key pair: virginiakeypair
  - Manage Tags for 4 instances
    - Key:  Name    - Value: ProdServer
    - Key:  Environment   - Value: Production
    
   ![83](/aws-ws/images/8-devopsaws/83/9.png?featherlight=false&width=50pc)

- Create New deployment group name: **MyProdInstances**
  - Service role: /**CodeDeployRole**
  - Deployment type: In-place
  - Environment configuration
      - Amazon EC2 instances
        - Key: Environment  - Value: Production
  - Deployment settings:
    - Deployment configuration: **CodeDeployDefault.OneAtATime** (use in this lab) or manual create
    
      - *Create deployment configuration*
        - *Deployment configuration name:  80percentinstance*
        - *Minimum healthy hosts:  Percentage*
        - *Value: 80*
        
  - Create deployment group
  - Create deployment
    - Deployment group: MyProdInstances
    - Revision type: S3: app.zip
    -> Create 

Check Website DNS

4. **appspec.yml**

```sh
version: 0.0
os: linux
files:
  - source: /index.html
    destination: /var/www/html/
hooks:
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
      
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root      

  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root

  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root

  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
  
```

5. Rollbacks

Developer Tools > CodeDeploy > Applications > CodeDeployDemo > MyProdInstances

- Edit:
  - Rollbacks:  Uncheck rollbacks
    - *Roll back when a deployment fails*
    - *Roll back when alarm thresolds are met*
    
   - Create deployment alarm: go to CloudWatch and create an alarm
    - Add Alarms
    - Select Metrics
      - CPUUtilization
      - Creater than ..: 70
      - Select an existing SNS topic
        - Default_CloudWatch_Alarms_Topic
    - Alarm name:  **EC2Utilization**
    - Create alarm
  
  - Add alarm : EC2Utilization
    - Roll back when alarm thresolds are met
    - Roll back when a deployment fails

