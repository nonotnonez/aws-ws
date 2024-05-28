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

