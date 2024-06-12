---
title : "AWS Control Tower"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 10.3 </b> "
---

#### Best practices
- Managements & Govermance > AWS Control Tower setup > Set up landding zone
  - Home region: ES East (N.Virginia)
  - Region deny settings: Not Enabled
    - *NEXT*
  - OUs 
    - Foundation OU - Name: Security
    - Additional OU - Opt out of creating OU
      - *NEXT*
  - Configure shared accounts
    - Log archive account
      - Create new account: XXX email
    - Audit account
      - Create new account: XXX email
        - *NEXT*
  - Additional configurations
    - Seft-managed AWS account access with IAM Identity Center or another method
    - AWS CloudTrail configuration : Not enabled > Confirm
      - *NEXT*
  - Check : I understand the permissions ... 
    - *SET UP LANDING ZONE*

![102](/aws-ws/images/10/103/1.png?featherlight=false&width=50pc)

- AWS Control Tower > Controls libray > All controls > Check: **Amazon s3 :[AWS-GR_S3_ACCOUNT_LEVEL..]** > Enable control
  - Enable control on OU : - Check Name: **Security** > Enable control on OU


#### AWS Control Tower
- Used to manage security and compliance of multiple accounts
- Govern a multi-account AWS environment following best practices
- Controls are rules that help with governance

Control Categories
- Controls are categorized based on:
  - **Behavior**
    - **Preventive** ( Preventive controls)
      - Disallow actions that lead to policy violations
      - For example:
        - Require attached EBS volumees to be configured to encript data at rest
        - Disallow bucket policy changes for S3 buckets
    - **Detective**( Detective controls )
      - Check for noncompliance of resources
      - For example:
        - Detect EC2 instance that have a public IPv4 address
        - RDS instances should have automatic backups enabled
    - **Proactive** ( Proactive Controls )
      - Scan resources before provisioning to ensure they're compliant
      - Noncompliant resources aren't provisioned
      - For example:
        - Require S3 buckets to have " Block public access" settings configured
        - Require application load balancers to have logging activated
  - **Guidance** (Control Guidance)
    - **Mandatory** (Mandatory Controls)
      - Are always applied and can not changed
        - Enable CloudTrail in all available regions
        - Disallow changes to CloudWatch set up by Control Tower
    - Strongly recommended 
      - Enfore common best practices for well-architected, multi-account environments
        - Disallow creation of access keys for the root user
        - Detect whether public read access to S3 buckets is allowed
    - Elective
      - Allow you to track or lock down actions commonly restricted in enterprise environments
        - Disallow changes to logging configuration for S3 buckets
        - Detect whether MFA is enabled for IAM users
    
  Strongly recommended and elective controls are optional

  Control Tower can provision new AWS accounts that adhere to security and compliance requiments

  AWS Control Tower
  - Control Tower creates a landing zone-a container for your accounts
  - Landing zone contains Security OU with two accounts:
    - Log archive account
      - Log archive account contains logging information for all enrolled accounts
    - Audit account
      - Audit account can be used to perform audits and automated security operations
      