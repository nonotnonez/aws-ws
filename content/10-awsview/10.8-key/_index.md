---
title : "Security keys and credentials"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 10.8 </b> "
---

#### Best practices

- Security, Identity & Compliance > IAM Identity Center > Enable
  - Settings > Authentication (By default, it requires users to register an MFA device at sign in  --> Change to lab env)
    - Multi-facor auhentication > Configure
      - MFA Settings > **Allow them to sign in**
        - SAVE CHANGES
  - Users : 
    - Add user
      - Username: **tuanta**
      - Password: Generate a one-time password that you can share with this user
        - Input user info
          - NEXT
    - Add user to group
      - NEXT

![102](/aws-ws/images/10/104/1.png?featherlight=false&width=50pc)

- (A permission set is a collection of one or more IAM policies)
  - IAM Identity Center > Multi-account permissions > Permission sets > Create permission set
    - Permission set type: Pridefined permission set
    - Policy for predefined permission set:
      - **ReadOnlyAccess**
        - NEXT
        - Session duration: 1 hour
          - NEXT - CREATE
        
- Assign to User:
  - IAM Identity Center > Multi-account permissions > AWS Account 
    - Select User: XXX > **Assign users to groups**
      - User : 
        -  Select User: XXX > NEXT
        -  Permission set: ReadOnlyAccess > NEXT > SUBNMIT

- Grant access to AWS or Customer-managed :      
  -  IAM Identity Center > Dashboard 
      -  AWS Access portal URL: 
          Login username / password (change at first time login)

![102](/aws-ws/images/10/104/2.png?featherlight=false&width=50pc)

#### IAM Identity Center

- Without a centralized identity solution, users must remember credentials for each account.
- IAM Identity Center centralizes access management for multiple AWS accounts, AWS applications, and SAML-enabled cloud applications.
  - Ex: Salesforce, Microsoft 365 ...
- IAM Identity Center was previously known as AWS Single Sign-On

Identity Sources for IAM Identity Center

- Identity Center directory
- Active Directory
- External identity provider
  - Ex: Okta or Microsoft Entra ID

IAM Identity Center can be used with or without AWS Organizations



      