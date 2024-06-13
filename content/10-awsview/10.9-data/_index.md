---
title : "Securing data in transit"
date : "`r Sys.Date()`"
weight : 9
chapter : false
pre : " <b> 10.9 </b> "
---

#### Best practices

- AWS Certificates Manager (ACM) > Request certificate
    - Domain name: example.com
    - Validation method: DNS validation 
    - Key algorithm:    RSA 2048
      - REQUEST
    - Input certificate detail

![102](/aws-ws/images/10/104/1.png?featherlight=false&width=50pc)


#### Securing data in transit

- Data in transit is secured using SSL/TLS certificates
- AWS Certificat Manager (ACM) allows you to provision and manage SSL/TLS certificates

SSL/TLS

- Secure Sockets Layer (SSL) and Transport Layer Security (TLS) are cryptographic protocols that secure communications
- TLS is the successor of SSL
- Both protocols use certificates
- When you visit a website, your browser requests the server's certificate 
- Your browser verifies that the server certificate is issued by a trusted certificates authority (CA)
- CAs are widely recognized and trusted to issue SSL/TLS certificates
- Your browser is preinstalled with a list of trusted CAs
- After verifying the server's identity, your browser establishes a secure channel using the public key in the certificate
      
AWS Certificate Manager (ACM)

- Allows you to request a publicly trusted certificate
- Manages private SSL/TLS certificates
- Allows you to import certificates issued by third-party CAs
- Supported by Elastic Load Balancing, CloudFront, Elastic Beanstalk, API gateway, etc..

ACM Certificates

- ACM certificates can secure:
  - Single domain names like www.example.com
  - Multiple domain names like www.example.com and www.example.net
  - Trusted by major browsers: Chrome, firefox
  