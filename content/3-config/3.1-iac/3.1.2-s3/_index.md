---
title : "S3"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.1.2 </b> "
---

In this Workshop we will create an S3 bucket

#### Settings

AWS Account :

- User name: tf-cli

Configure files :

{{%expand "Docker-compose.yml" %}}
```sh
version: '3'
services:
  terraform:
    image: hashicorp/terraform:latest
    volumes:
      - .:/terraform
    working_dir: /terraform
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
{{% /expand%}}

{{%expand ".env" %}}
```sh
AWS_ACCESS_KEY_ID=<ID>
AWS_SECRET_ACCESS_KEY=<key>
AWS_REGION=ap-southeast-1
```
{{% /expand%}}

#### Configuring

AWS Version
```js
docker-compose run --entrypoint aws aws --version
```
