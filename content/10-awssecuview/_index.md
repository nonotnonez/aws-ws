---
title : "AWS & Security Overview"
date : "`r Sys.Date()`"
weight : 10
chapter : false
pre : " <b> 10. </b> "
---

#### Security best practices

|  -| - |- |
|---|---|---|
|Avoid using the root account for everyday activities| Enable multifactor authentication for all users | Implement least privilege access |
|Regularly review user permissions| Use a centralized identity provider to manage user identities | Use account-level separation|
|Use AWS Organization to manage multiple accounts and enforce policy-based management| Use AWS Control Tower to setup accounts bases on best practices| Congigure a strong password policy|
|Use IAM Roles instead of access keys| Audit credentials to identify unused users, roles, permissions, policies, or credentials | Identify resources shared with external entities|
|Identify unused access, such as roles, access keys, or passwords| Validate IAM policies against AWS best practices | IAM Access Analyzer |

- A credential report lists all users and the status of their credentials
- Security Best Practices:
  - Store secrests securely
  - Create network-level isolation
  - Classify data
  - Protect data at rest and in motion
  - Configure logging
  - Perform vulnerability management
  - Keep up-to-date with security recommendations
- AWS Services:
  - Identity and Access Management (IAM)
  - Organizing multiple accounts with AWS Organizations
  - Governing multiple accounts using AWS Control Tower
  - IAM Identity Center
  - VPC and Subnets
  - VPN and Direct Connect
  - Controlling inbound traffic with security groups and network ACLS
  - Securing keys and Credentials (AWS Key Management Service - KMS)
  - Security data in transit (AWS Certificate Manager - ACM )
----
- Question
  - 1. How does IAM Identity Center help in simplifying access management for users across multiple AWS accounts and SAML-enabled cloud applications?
    - It centralizes access management through a single sign-on experience, reducing the need for multiple sets of credentials.
  - 2. Which connectivity option in AWS allows customers to establish a dedicated private connection from their on-premises network to their VPC?
    -  AWS Direct Connect
  - 3. How does AWS Direct Connect differ from AWS Site-to-Site VPN in terms of connectivity?
    - AWS Direct Connect provides a dedicated connection bypassing the public internet, while AWS Site-to-Site VPN establishes a secure connection over the public internet.
  - 4. How do network ACLs differ from security groups?
    - Network ACLs operate at the subnet level and support both allow and deny rules, while security groups operate at the resource level and only support allow rules.
  - 5. Which of these is true about AWS Key Management Service (KMS)?
    - AWS KMS provides AWS-managed keys, created automatically by the service, and customer-managed keys, created by the user.
    - AWS KMS supports both symmetric and asymmetric keys for cryptographic operations.
  - 6. Which of these is true about AWS Secrets Manager?
    - Secrets stored in AWS Secrets Manager are encrypted at rest using AWS KMS keys.
    - Secrets Manager allows you to manage and retrieve database and application credentials, API keys, OAuth tokens, SSH keys and other secrets.
  - 7. What type of domain names can ACM certificates secure?
    - single, multiple, and wildcard domain names
  - 8. Why should the root AWS account not be used for everyday activities?
    - It increases the risk of unauthorized access and misuse.
  - 9. How does the best practice recommendation of the principle of least privilege help when granting access permissions in AWS?
    - It grants only the minimum permissions required for a task or job role to enhance security.
  - 10. How do IAM policies affect access when multiple policies are associated with a single AWS user?
    - The user's effective permissions are the sum total (cumulative effect) of all the attached policies.
  - 11. Why might an organization choose to use AWS Organizations for managing multiple AWS accounts?
    - to enable centralized billing, management of permissions, and policy enforcement across all accounts
  - 12. How do AWS Control Tower's log archive account and audit account enhance security and compliance?
    - They centralize the storage of logs and facilitate the auditing process.

----
#### Design a Strategy for Secure Access
   - **Implement strong identity and access management**
      - Manage identities within AWS or use an external identity provider
      - Consider granting secure access to external users
      - Implement principle of least privilege
      - Use identity-base policies to define permissions for users and groups
      - Use resource-based policies to control access to resources
      - Use services control policies (SCPs) to define maximum permissions for member accounts in an AWS organization
    - **Secure every layer**
![9](/aws-ws/images/10/100/1.png?featherlight=false&width=40pc)      
      - Protect data
  - Considerations for Identifying and Classifying data
    - Is it personally identifiable information (PII) ?
    - Is it intellectual property ?
    - Is it protected health information (PHI)?
    - Is it financial information?
    - Where is the data stored?
    - Who owns the data ?
    - Who can access and modify the data?
    - What is the bussiness impact ?
  - **Protect data**
    - Use encryption to protect data at rest
    - Use secure protocols to protect data in transit
  - **Implement Traceability**
    - Use AWS CloudTrail to maintain an audit trail
    - Use AWS Config to record configurations
    - Use AWX X-Ray to get an end-to-end view of request
    - Use VPC Flow Logs to capture IP traffic
    - Tag resources to assign ownership
    - Store logs in an S3 bucket or CloudWatch log group for analysis
  - **Prepair for Security Events**
    - Create an incident response plan
    - Documentation on AWS incident respose

#### AWS Organizations or AWS Control Tower?

- Use AWS Organizations if you need a simple solution to consolidate multiple accounts into a single organization
- Use AWS Control Tower if you want to set up accounts according to sercuriy and compliance best practices
