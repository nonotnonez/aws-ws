---
title : "EC2"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1.1 </b> "
---

In this Workshop we will automate create an EC2 instances with the information bellow by using **Terraform**, **Docker-compose** and **AWS CLI**

#### Overview AWS EC2
- AWS User : **tf-cli**
- Access & Secret key :  **tf_cli-access_key.json**
- AWS Key-pair : **tf-cli-keypair**

- EC2 Instance:
  - Instances name: **Web-Server**
  - VPC: 10.0.0.0/16
  - Subnets: 10.0.1.0/24
  - Region: Singapore (ap-southeast-1)
  - Available zone: ap-southeast-1b
  - Instance type: t2.micro
  - Amazon Machine Images: Amazon Linux 2 AMI
  - Key pair: **tf-cli-keypair**
  - Security setting: 
      -   Only allow **my ip** connect **SSH** to EC2 instance
      -   Allow all access from port **8080** to EC2 instance
   
#### Review Configuration

- Container:

{{%expand "Docker-compose.yml " %}}

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

- AWS :

{{%expand "User Access , Secret Key, Key Pair: " %}}
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
{{% /expand%}}

{{%expand "Attach User with Policy Access EC2 and Limit Region" %}}
```sh
# Custom policy file:
  ec2-limited-access-policy.json

# Create IAM poliy: EC2FullAccessAPSouthEast1
  docker-compose run --rm aws iam attach-user-policy --user-name tf-cli --policy-arn arn:aws:iam::637423373411:policy/EC2FullAccessAPSouthEast1

#
```
{{% /expand%}}

{{%expand "ec2-limited-access-policy.json" %}}
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


- Terraform :

 {{%expand "variables.tf - Security credential variables  " %}}
```sh
variable "access_key" {
  type        = string
  sensitive   = true
}

variable "secret_key" {
  type        = string
  sensitive   = true
}

variable "region" {
  type        = string
  default     = "ap-southeast-1"
}
```
{{% /expand%}}

{{%expand "main.tf - Instances configurations" %}}

```sh
variable vpc_cidr_block {}
variable subnet_1_cidr_block {}
variable avail_zone {}
variable env_prefix {}
variable instance_type {}
variable my_ip {}
variable ami_id {}

resource "aws_vpc" "myapp-vpc" {
  cidr_block = var.vpc_cidr_block
  tags = {
      Name = "${var.env_prefix}-vpc"
  }
}

resource "aws_subnet" "myapp-subnet-1" {
  vpc_id = aws_vpc.myapp-vpc.id
  cidr_block = var.subnet_1_cidr_block
  availability_zone = var.avail_zone
  tags = {
      Name = "${var.env_prefix}-subnet-1"
  }
}

resource "aws_security_group" "myapp-sg" {
  name   = "myapp-sg"
  vpc_id = aws_vpc.myapp-vpc.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port       = 0
    to_port         = 0
    protocol        = "-1"
    cidr_blocks     = ["0.0.0.0/0"]
    prefix_list_ids = []
  }

  tags = {
    Name = "${var.env_prefix}-sg"
  }
}

resource "aws_internet_gateway" "myapp-igw" {
	vpc_id = aws_vpc.myapp-vpc.id
    
    tags = {
     Name = "${var.env_prefix}-internet-gateway"
   }
}

resource "aws_route_table" "myapp-route-table" {
   vpc_id = aws_vpc.myapp-vpc.id

   route {
     cidr_block = "0.0.0.0/0"
     gateway_id = aws_internet_gateway.myapp-igw.id
   }

   # default route, mapping VPC CIDR block to "local", created implicitly and cannot be specified.

   tags = {
     Name = "${var.env_prefix}-route-table"
   }
 }

# Associate subnet with Route Table
resource "aws_route_table_association" "a-rtb-subnet" {
  subnet_id      = aws_subnet.myapp-subnet-1.id
  route_table_id = aws_route_table.myapp-route-table.id
}

output "server-ip" {
    value = aws_instance.myapp-server.public_ip
}

resource "aws_instance" "myapp-server" {
  ami                         = var.ami_id
  instance_type               = var.instance_type
  key_name                    = "tf-cli-keypair"
  associate_public_ip_address = true
  subnet_id                   = aws_subnet.myapp-subnet-1.id
  vpc_security_group_ids      = [aws_security_group.myapp-sg.id]
  availability_zone			  = var.avail_zone

  tags = {
    Name = "${var.env_prefix}-server"
  }
}

```
{{% /expand%}}

{{%expand "terraform.tfvars - Terraform provider" %}}

```sh
# Network and Instance variables
vpc_cidr_block = "10.0.0.0/16"
subnet_1_cidr_block = "10.0.1.0/24"
avail_zone = "ap-southeast-1b"
env_prefix = "web"
my_ip = "<myip>/32"
ami_id = "ami-04f73ca9a4310089f"
```
{{% /expand%}}

### Installation
Terraform plan: 
```dockercompose
 docker-compose run –rm terraform plan
```
{{%expand "process" %}}
![311](/aws-ws/images/3-config/3.1-iac/311/1.png)
{{% /expand%}}

Terraform apply: 
```dockercompose
 docker-compose run --rm terraform apply --auto-approve
```
{{%expand "process" %}}
![311](/aws-ws/images/3-config/3.1-iac/311/2.png)
{{% /expand%}}

AWS Instance checking:

![311](/aws-ws/images/3-config/3.1-iac/311/4.png?featherlight=false&width=90pc)

Add Keypair permission:
```dockercompose
chmod 400 tf-cli-keypair.pem 
```
SSH to EC2 Instances:

```dockercompose
 ssh -i tf-cli-keypair.pem ec2-user@13.250.64.49
```
-   AWS Instance checking:
![23](/aws-ws/images/3-config/3.1-iac/311/3.png?featherlight=false&width=90pc)