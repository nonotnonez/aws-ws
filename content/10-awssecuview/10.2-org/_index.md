---
title : "AWS Organizations"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 10.2 </b> "
---

#### Best practices

**Overview**

 SCPs are simple powerful tools to limit actions of member accounts. And also, using AWS Organizations can be a strategic architectural decision, allowing you to easily organize and gain control over multiple AWS accounts.

**Process**
- Managements & Govermance > AWS Organizations > Create an organization
![102](/aws-ws/images/10/102/1.png?featherlight=false&width=40pc)

- Add an AWS account > Invite an existing AWS account
  - Email address or account ID of the AWS accounts to invite: **AWS ID**
    - *SEND INVITATION*
- AWS Organizations > View 1 invitation > *Accept invitation*

- AWS Organizations > AWS accounts > Select Root > Actions > Create new
  - Create organizational unit in Root
    - Organizational unit name: **Production** > *CREATE ORGANIZATIONAL UNIT*
- AWS Organizations > AWS accounts > Select account in Other OU and move to new OU: Production

![102](/aws-ws/images/10/102/2.png?featherlight=false&width=40pc)

- AWS Organizations > Services > Integrated services : Disable (default)
- AWS Organizations > Policies > Service control policies : **Enable service control policies**
  - Available policies > Create policy > Policy name: **EC2PolicyForMemberAccounts**
    - Edit statement > Add actions > Choose a service
      - **EC2**
        - Access level - list
          - **RunInstances**
      - Add a resource > ADD
        - Service: EC2
        - Resource type: All Resources
          - **ADD RESOURCE**
    - CREATE POLICY

![102](/aws-ws/images/10/102/3.png?featherlight=false&width=40pc)

- AWS Organizations > AWS accounts > **ACCOUNT**
  - Polices > Attach:  EC2PolicyForMemberAccounts
    - *ATTACH POLICY*         

![102](/aws-ws/images/10/102/4.png?featherlight=false&width=50pc)

- Verify : EC2 - Instances - Launch instances

![102](/aws-ws/images/10/102/5.png?featherlight=false&width=50pc)


#### AWS Organizations
- Uses for Multiple AWS Accounts
  - Organizations use multiple AWS accounts to:
    - Segregate environments
    - Comply with regulations
    - Create security boundaries
    - Work around services limits
    
    Multiple AWS accounts create administrative and architectural challenges

    AWS Organizations and AWS Control Tower are commonly used to manage multiple AWS accounts

    AWS Organiztions allows you to consolidate multiple AWS accounts into an organization

- AWS Organizations
  - Use AWS Organizations to:
    - Invite other AWS accounts and manage them centrally
    - Consolidate billing in the management account
    - Group accounts into organizational units (OUs)
    - Implement service control policies (SCPs) to define maximum permissions for member accounts
    