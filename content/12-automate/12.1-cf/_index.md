---
title : "CloudFormation"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 12.1 </b> "
---


#### AWS CloudFormation

- A template is a JSON- or YAML-formatted text file

![9](/aws-ws/images/12/1/10.png?featherlight=false&width=40pc)

  - Resource:
    - The resources section of the template describes the resource type, in this case the EC2 instance. 
  - Properties:
    - The property section describes properties such as the instance type, image ID, and the security group to use. 

- **CloudFormation** can only perform actions that you have permission to do

|  |  |  |
|---|---| ---|
|![121][1]| ![121][2]| ![121][3]|

- **Demo**:

- Cloudformation > Stacks 
  - Create stack
    - Prepare temple: **Use a sample template**
    - Select: **WorkPress blog**
  - Specify stack details
    - Stack name: **workpress-blog**
    - Parameters:
      - DBName: workpressdb
      - DBPassword: ***
      - DBRootPassword: ***
      - DBUser: ***
      - InstanceType: **t2.micro**
      - KeyName:
      - SSHLocation: 
        - NEXT
  - Configure stack options
    - Perrmissions
      - IAM role: (limit CF)
        - NEXT
        - SUBMIT

|  |  |  |
|---|---| ---|
|![121][11]| ![121][12]| 
|![121][13]|


[1]: /aws-ws/images/12/1/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/12/1/2.png?featherlight=false&width=40pc
[3]: /aws-ws/images/12/1/3.png?featherlight=false&width=40pc
[4]: /aws-ws/images/12/1/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/12/1/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/12/1/6.png?featherlight=false&width=40pc
[7]: /aws-ws/images/12/1/7.png?featherlight=false&width=40pc
[8]: /aws-ws/images/12/1/8.png?featherlight=false&width=40pc
[9]: /aws-ws/images/12/1/9.png?featherlight=false&width=40pc
[10]: /aws-ws/images/12/1/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/12/1/11.png?featherlight=false&width=50pc
[12]: /aws-ws/images/12/1/12.png?featherlight=false&width=50pc
[13]: /aws-ws/images/12/1/13.png?featherlight=false&width=50pc
[14]: /aws-ws/images/12/1/14.png?featherlight=false&width=50pc
[15]: /aws-ws/images/12/1/15.png?featherlight=false&width=50pc