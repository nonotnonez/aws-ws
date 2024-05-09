---
title : "S3"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.1.2 </b> "
---

In this Workshop we will automate create an S3 bucket

#### Overview S3 Bucket

- AWS User: **tf-s3-cli**
- AWS Policy: **AmazoneS3FullAccess**
- Bucket name: **090524-tfs3bucket**

#### Review Configuration

- AWS : {{%expand "User Access , Secret Key" %}}
```sh
# check aws version
  docker-compose run --rm aws --version

# create aws user
  docker-compose run --rm aws iam create-user --user-name tf-s3-cli

# create access key for user
  docker-compose run --rm aws iam create-access-key --user-name tf-s3-cli > tf-s3-cli-access-key.json

```
{{% /expand%}}  {{%expand "Attached Amazon S3 Full Access" %}}
```sh
# AmazoneS3FullAccess
  docker-compose run --rm aws iam attach-user-policy --user-name tf-s3-cli --policy-arn arn:aws:iam::637423373411:policy/AmazoneS3FullAccess
```
{{% /expand%}} 

- Container: {{%expand "Docker-compose.yml " %}}

```sh
version: '3'
services:

# Terraform
  terraform:
    image: hashicorp/terraform:latest
    volumes:
      - .:/terraform
    working_dir: /terraform

# AWS CLI'
  aws:
    image: anigeo/awscli
    environment:
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
      AWS_REGION: "${AWS_REGION}"
      AWS_DEFAULT_REGION: ap-southeast-1
    volumes:
      - $PWD:/app
    working_dir: /app
```
{{% /expand%}} {{%expand ".env - aws access & secret key" %}}

```sh
AWS_ACCESS_KEY_ID=xxx
AWS_SECRET_ACCESS_KEY=xxx
AWS_REGION=ap-southeast-1
```
{{% /expand%}}

- Terraform 

  - **main.tf**

```sh
# Variables
variable "access_key" {
  type        = string
  sensitive   = true
}

variable "secret_key" {
  type        = string
  sensitive   = true
}

# Tf provider
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

# IAM access
provider "aws" {
  region  = "ap-southeast-1"
  access_key = var.access_key
  secret_key = var.secret_key
}

# S3 Bucket
resource "aws_s3_bucket" "example" {
  bucket = "090524-tfs3bucket"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}
```



#### Installation

AWS Version
```js
docker-compose run --entrypoint aws aws --version
```

Amazon S3 Permission
```js
docker-compose run --entrypoint aws s3 ls
```

![311][1]

Create S3 Bucket

```js
docker-compose run --rm terraform init
docker-compose run --rm terraform plan
docker-compose run --rm terraform apply --auto-approve
```
![311][4]

AWS Console review

![311][2]

Destroy S3 Bucket

```js
docker-compose run --rm terraform destroy --auto-approve
```

![311][5] ![311][6]
![311][3]


[1]: /aws-ws/images/3-config/3.1-iac/312/1.png?featherlight=false&width=50pc
[2]: /aws-ws/images/3-config/3.1-iac/312/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/3-config/3.1-iac/312/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/3-config/3.1-iac/312/4.png?featherlight=false&width=50pc
[5]: /aws-ws/images/3-config/3.1-iac/312/5.png?featherlight=false&width=50pc