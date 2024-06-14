---
title : "Resilience and High Availiability"
date : "`r Sys.Date()`"
weight : 13
chapter : false
pre : " <b> 13. </b> "
---

#### Chapter Quiz

1. How does a read replica contribute to the resilience of a database in Amazon RDS?
   - by offloading read queries and serving as a backup in case of primary instance failure
2. What feature in Amazon RDS and Amazon Aurora enables seamless transition between production and staging environments for databases?
   - Blue/Green deployment
3. In which scenario is a Gateway load balancer recommended?
   - when using third-party virtual appliances for network traffic inspection
4. Which AWS services are commonly used for decoupling and creating event-based architectures?
   - imple Notification Service (SNS) and Simple Queue Service (SQS)
5. How does Simple Queue Service (SQS) help in creating a decoupled application architecture?
   - It acts as a message buffer between sender and receiver applications.
6. Which AWS service allows developers to focus only on the application code while handling deployment details like capacity provisioning and load balancing?
   - Elastic Beanstalk
7. What is the benefit of using Amazon EC2 Instances as the launch type for ECS?
   - It provides more control over the underlying infrastructure, allowing for customization.
8. Which AWS storage service allows on-premises systems to store and retrieve objects in Amazon S3?
   - Storage Gateway
9. How is Recovery Time Objective (RTO) defined?
   -  the maximum acceptable delay between service interruption and restoration
10. Why is it recommended to build loosely coupled architectures?
    -  to contain failures to only affected components and allow independent scaling
12. How does distributing workloads across multiple Availability Zones contribute to system resilience? 
    - It minimizes the impact of failures in a single location.




#### Design for Failure

| RTO and RPO |  |  |
|---|---| ---|
|![13][1]| ![13][2]| ![13][3]|
|![13][4]| ![13][5]| ![13][6]|
|![13][7]| ![13][8]| ![13][9]|
|![13][10]| | |

[1]: /aws-ws/images/13/0/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/13/0/2.png?featherlight=false&width=40pc
[3]: /aws-ws/images/13/0/3.png?featherlight=false&width=40pc
[4]: /aws-ws/images/13/0/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/13/0/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/13/0/6.png?featherlight=false&width=40pc
[7]: /aws-ws/images/13/0/7.png?featherlight=false&width=40pc
[8]: /aws-ws/images/13/0/8.png?featherlight=false&width=40pc
[9]: /aws-ws/images/13/0/9.png?featherlight=false&width=40pc
[10]: /aws-ws/images/13/0/10.png?featherlight=false&width=40pc