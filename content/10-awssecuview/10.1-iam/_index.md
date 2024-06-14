---
title : "Identity and Access Management"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 10.1 </b> "
---

#### Best practices

**Overview**

Using IAM roles to grant access to third parties is more secure than sharing account credentials.
Having a clear strategy for managing identities is important to the security of your AWS environment.

**Workflow**

- Login AWS Console
- IAM > Roles > Create Role
  - Select trusted entiy: AWS account
  - An AWS account > Another AWS account: Account ID: **XXX**
  - Options > Require external ID ...
    - *NEXT*
  - Add permissions > Permissions policies : **AmazonEC2ReadOnlyAccess**
  - Role name: **EC2ReadOnlyAccess**
    - *CREATE ROLE*

- Roles : EC2ReadOnlyAccess
  - Trust relationships > Edit trust policy
    - line 7: "AWS": "arn:aws:iam::...:user/**tuanta**"
      - *UPDATE POLICY*

- IAM > Policy > Create policy
  - Specify permission : **STS** (Security Token Service)
  - Actions allowed > Assess level: 
    - Write: **AssumeRole**
    - Resources: **ALL** (keep simple for lab)
      - *NEXT*
    - Policy name: **AssumeRolePolicy**
      - *CREATE POLICY*
- IAM > User > User name: **tuanta**
  - Permisions > Add permissions > Attach policies directly
    - Permission policies: AssumeRolePolicy
      - *ADD PERMISSIONS*
- Login IAM User
  - Click " Switch role"
  - Input information > Switch Role

|  IAM ROLE | Switch Role |
|---|---|
|![101](/aws-ws/images/10/101/4.png?featherlight=false&width=40pc)| ![101](/aws-ws/images/10/101/5.png?featherlight=false&width=40pc)|
|![101](/aws-ws/images/10/101/6.png?featherlight=false&width=40pc)| |

#### AWS IAM
- With IAM, you can manage access for internal and external uers
- IAM entities include users, groups, policies, and roles

#### IAM Users and Groups
- A user consists of a name and credentials
- Group users to create user groups
- IAM allows you to manage identities outside of AWS
  - Ex: Microsoft Active Directory
- Use identity providers that support OpenID Connect (OIDC) or Security Assertion Markup Languages (SAML) 2.0

| Policy | - |
|---|---|
|An IAM policy is a document that contains permissions | ![101](/aws-ws/images/10/101/1.png?featherlight=false&width=40pc)|

- IAM Role
  - Similar to an IAM User - it is an identity with permissions
  - Not uniquely associated with a user
  - Intended to be assumed by anyone who needs it
  - Roles have policies associated with them
  
  
| IAM Role | - |
|---|---|
| AWS does not permit this access by default | ![101](/aws-ws/images/10/101/2.png?featherlight=false&width=40pc)|
| Attaching an IAM Role with appropriate permissions | ![101](/aws-ws/images/10/101/3.png?featherlight=false&width=40pc)|

  - Use IAM roles to grant other AWS account access to permissions in your account
    - Ex: 
      - Grant access to third party to upload objects to an S3 bucket in your account
      - Share your logs with a third party for monitoring and analysis.