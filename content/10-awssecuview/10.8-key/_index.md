---
title : "Security keys and credentials"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 10.8 </b> "
---

#### Best practices

**KMS**

- Security, Identity, & Compliance > Key Management Service (KMS)
  - Customer managed keys - Create key
    - Configure key
      - Key type:
      - Key usage:
      - Advanced options: KMS
      - Regionality: Single-Region key
        - NEXT
    - Add lables
      - Alias:  **key-1**
        - NEXT
    - Define key administrative permissions
      - NEXT
    - Define key usage permissions
      - NEXT

- KMS > Customer managed keys : key-1
  - Schedule key deletion > Waiting period (in days): 30 > SCHEDULE DELETION


|  -| - |
|---|---|
|![108](/aws-ws/images/10/108/1.png?featherlight=false&width=40pc)| ![108](/aws-ws/images/10/108/2.png?featherlight=false&width=50pc)|

- Use this key to encrypt an EBS volume
  - EBS > Volumes > Create volume

![108](/aws-ws/images/10/108/3.png?featherlight=false&width=40pc)

**Secrets Manager**

- AWS Secrets Manager > Store a new secret
  - Secret type: Other type of secret
    - Key/value pairs:   **test-key**     /   **test-value**
    - Encryption key:   aws/secretsmanager
      - NEXT
  - Configure secret
    - Secret name:  **test-secret**
      - (can you Lambda function to rotate secrets key)
      - NEXT
    - STORE

![108](/aws-ws/images/10/108/4.png?featherlight=false&width=40pc)

----
#### Store Sensitive Information in AWS

- AWS KMS allows you to create and store keys used for cryptographic operations
- AWS Secrets Manager protects secrets related to your applications and services

**AWS KMS**

- Create and edit symmetric and asymmetric keys
- Control access to keys using key policies and IAM policies
- Automatically rotate the cryptographic material of a key
- For asymmetric key pairs, the public key can be exported outside of AWS
- Keys are AWS-managed or customer-managed
- You can also create multi-region keys

**Secrets Manager**

- Secrets Manager can manage:
  - Database and application credentials
  - API keys
  - OAuth tokens
  - SSH keys
  - Other secrets     
  - Encrypts secrets using KMS keys
  - When retrieved, the secret is decrypted and transmitted over TLS
  - Control access using IAM policies
  - Use IAM cross-account access to allow users in other accounts to access secrets in your account
  - Can automatically rotate your secrets based on a schedule using a Lambda function

Hard-coding credentials in the application source code is a security risk

Store credentials in Secrets Manager and configure the application to call service