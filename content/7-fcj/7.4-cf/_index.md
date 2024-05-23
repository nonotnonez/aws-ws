---
title : "CloudFormation"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 7.4 </b> "
---

#### AWS CloudFormation

Source: https://000037.awsstudygroup.com/

1. Prepairation

- IAM - Users: **CloudFormation-user**
  - Permissions:  AdministratorAccess
  - Key: Name   - Value: Admin User
  - Download Access Key ID & Secret key
- IAM - Roles: **CloudFormation-Role**
  - AWS Service
  - EC2
  - Permission: AdministratorAccess

2. Basic CloudFormation

2.1 Workspace

- Cloud9: **ASG-Cloud9-Workshop**
  - EC2 Instances
  - Timeout 30 Minutes
  - VPC: cloudformation-vpc
  - Subnet: public-subnet-1
  - IAM Role: CloudFormation-Role
- Cloud9 Interface
  - AWS settings - Credentials - Uncheck AWS managed temporary credentials

- Workspace:
  - sudo yum -y install jq gettext bash-completion moreutils
  - pip install cfn-lint
  - cfn-lint --version
  - pip install taskcat



[1]: /aws-ws/images/7/73/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/7/73/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/7/73/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/7/73/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/7/73/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/7/73/6.png?featherlight=false&width=50pc
[7]: /aws-ws/images/7/73/7.png?featherlight=false&width=40pc
[8]: /aws-ws/images/7/73/8.png?featherlight=false&width=50pc
[9]: /aws-ws/images/7/73/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/7/73/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/7/73/11.png?featherlight=false&width=50pc
[12]: /aws-ws/images/7/73/12.png?featherlight=false&width=50pc
[13]: /aws-ws/images/7/73/13.png?featherlight=false&width=50pc
[14]: /aws-ws/images/7/73/14.png?featherlight=false&width=50pc
[15]: /aws-ws/images/7/73/15.png?featherlight=false&width=50pc
[16]: /aws-ws/images/7/73/16.png?featherlight=false&width=50pc
[17]: /aws-ws/images/7/73/17.png?featherlight=false&width=50pc
[18]: /aws-ws/images/7/73/18.png?featherlight=false&width=50pc
[19]: /aws-ws/images/7/73/19.png?featherlight=false&width=50pc
[20]: /aws-ws/images/7/73/20.png?featherlight=false&width=50pc
[21]: /aws-ws/images/7/73/21.png?featherlight=false&width=50pc
[22]: /aws-ws/images/7/73/22.png?featherlight=false&width=50pc
[23]: /aws-ws/images/7/73/23.png?featherlight=false&width=50pc
[24]: /aws-ws/images/7/73/24.png?featherlight=false&width=40pc
[25]: /aws-ws/images/7/73/25.png?featherlight=false&width=40pc



[30]: /aws-ws/images/7/73/30.png?featherlight=false&width=50pc
[31]: /aws-ws/images/7/73/31.png?featherlight=false&width=50pc
[32]: /aws-ws/images/7/73/32.png?featherlight=false&width=50pc
[33]: /aws-ws/images/7/73/33.png?featherlight=false&width=50pc


[40]: /aws-ws/images/7/73/40.png?featherlight=false&width=50pc
[41]: /aws-ws/images/7/73/41.png?featherlight=false&width=50pc
[42]: /aws-ws/images/7/73/42.png?featherlight=false&width=50pc
[43]: /aws-ws/images/7/73/43.png?featherlight=false&width=50pc
[44]: /aws-ws/images/7/73/43.png?featherlight=false&width=50pc