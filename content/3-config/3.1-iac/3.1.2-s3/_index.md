---
title : "S3"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.1.2 </b> "
---

In this Workshop we will automate create an S3 bucket

#### Overview S3 Bucket

- AWS User:
- AWS Policy:
- Bucket name:

#### Review Configuration

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
{{% /expand%}}

- AWS : {{%expand "User Access , Secret Key, Key Pair: " %}}
```sh
# check aws version
  docker-compose run --rm aws --version

# create aws user
  docker-compose run --rm aws iam create-user --user-name tf-cli

# create access key for user
  docker-compose run --rm aws iam create-access-key --user-name tf-cli > tf_cli-access_key.json

# create keypair for ec2 
  docker-compose run --rm aws ec2 create-key-pair --key-name tf-cli-keypair --query 'KeyMaterial' --output text > tf-cli-keypair.pem
```
{{% /expand%}}  {{%expand "EC2 and Limit Region" %}}
```sh
# Custom policy file:
  ec2-limited-access-policy.json

# Create IAM poliy: EC2FullAccessAPSouthEast1
  docker-compose run --rm aws iam attach-user-policy --user-name tf-cli --policy-arn arn:aws:iam::637423373411:policy/EC2FullAccessAPSouthEast1
```
{{% /expand%}} {{%expand "ec2-limited-access-policy.json" %}}
```sh
 {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:Region": "ap-southeast-1"
                }
            }
        }
    ]
}
```
{{% /expand%}}

- Terraform 
{{%expand "main.tf" %}}

```js
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
  bucket = "my-tf-test-bucket"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}
```
{{% /expand%}}  {{%expand ".env" %}}
```sh
AWS_ACCESS_KEY_ID=<ID>
AWS_SECRET_ACCESS_KEY=<key>
AWS_REGION=ap-southeast-1
```
{{% /expand%}}

#### Installation

AWS Version
```js
docker-compose run --entrypoint aws aws --version
```

Amazon S3 Permission
```js
docker-compose run --entrypoint aws s3 ls
```


