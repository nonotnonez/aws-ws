---
title : "Elastic Beanstalk"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 13.3 </b> "
---


#### Best Practices

- IAM : 
  - Policies: **ElasticBeanstalk-Instance-Policy**
  - Roles: **ElasticBeanstalk-Instance-Role**
    - AWS Service > Use case: EC2
    - Policies: ElasticBeanstalk-Instance-Policy

![131](/aws-ws/images/13/3/8.png?featherlight=false&width=40pc)

- Compute > Elastic Beanstalk > **Create Application**
  - 1. Configure environment
    - Environment tier > Web server environment
    - Application name: demoapp
    - Environment name: Demoapp-env
    - Domain: demoapp > Check availability
    - Platform: Python
    - Application code: Sample application
    - Configuration presets: Single instance (free tier eligble)
      - *NEXT*
  - 2. Configure service access
    - Service access:
      - Existing service roles:   aws-elasticbeanstalk-service-Role
      - EC2 instance profile: aws-elasticbeanstalk-Instance-Role
  - 3. Configure instance traffic and scaling
    - VPC: 172.31.0.0/16 (default)
      - Instance subnets: 
        - 172.31.0.0/20  - us-east-1a
        - 172.31.80.0/20  - us-east-1b
        - *NEXT*
    - Instances
    - Volumes: SSD
    - Size: 8 GB
    - CloudWatch monitor: 5 minutes
    - IMDS: Check Deactivated
    - Security Groups: default
    - Auto scaling group: Single instance
    - Feet composition: On-Demand instance
      - *NEXT*
  - 4. Configure updates, monitoring, and logging
    - System: Basic
    - Managed updates: Uncheck Adtivated
      - *NEXT*
      - *SUBMIT* 

|  Review |   | 
|---|---| 
|![131][9]| ![131][10]| 

----
#### Host Apps in the Cloud
- Manage both infrastructure and application
- Use a managed service to handle the infrastructure so developers can focus on the application

|  |  |  |
|---|---| ---|
|![131][2]| ![131][3]| ![131][4]|
|![131][5]| ![131][6]| ![131][7]|


[1]: /aws-ws/images/13/3/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/13/3/2.png?featherlight=false&width=40pc
[3]: /aws-ws/images/13/3/3.png?featherlight=false&width=40pc
[4]: /aws-ws/images/13/3/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/13/3/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/13/3/6.png?featherlight=false&width=40pc
[7]: /aws-ws/images/13/3/7.png?featherlight=false&width=40pc
[9]: /aws-ws/images/13/3/9.png?featherlight=false&width=40pc
[10]: /aws-ws/images/13/3/10.png?featherlight=false&width=40pc