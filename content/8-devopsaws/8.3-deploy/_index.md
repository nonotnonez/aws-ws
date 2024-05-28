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

