---
title : "Optimize Cost"
date : "`r Sys.Date()`"
weight : 11
chapter : false
pre : " <b> 11. </b> "
---

#### Chapter Quiz

1. How does a tagging policy contribute to cost optimization?
   - It helps in tracking costs and identifying resources by owner, project, and other attributes.
2. How can **CloudWatch** help in rightsizing EC2 instances?
   - to monitor resource usage and identify opportunities for downsizing
3. How can lifecycle policies in Amazon S3 help reduce storage costs?
   - by transitioning data to cheaper storage classes and deleting expired data
4. When should you use **AWS Budgets**?
   - when you want to set a monthly cost budget with alerts

#### Best Practices

- AWS Account > Billing and Cost Management 
 
  - **Pricing Calculator** :
    -  Create Estimate
       - Configure EC2 > SAVE AND ADD SERVICE
       - Add Service

![9](/aws-ws/images/11/18.png?featherlight=false&width=40pc)

  - **Cost Explorer**
    - Report parameter
      - Tag: 
        - Cost Center: Ex: Filter ID: 10011 | 10012
    - Save to report library

![9](/aws-ws/images/11/17.png?featherlight=false&width=40pc)

| Budget |  | 
|---|---|
|![9](/aws-ws/images/11/19.png?featherlight=false&width=40pc)| ![9](/aws-ws/images/11/20.png?featherlight=false&width=40pc)|

  - **Cost Allocation Tags**

![9](/aws-ws/images/11/21.png?featherlight=false&width=40pc)

  - **Cost and Usage Reports**
  - **Data Exports**

| Bills |  | 
|---|---|
|![9](/aws-ws/images/11/22.png?featherlight=false&width=40pc)| ![9](/aws-ws/images/11/23.png?featherlight=false&width=40pc)|

#### Strategies for optimizing costs

- Understand Pricing Models
  - Most services have pay-as-you-go pricing
  - Services have configuration options to optimize cotst
  - For example:
    - EC2 instances support On-Demand Instances, Saving Plans, and more 
    - Amazon S3 has storage classes with different pricing
    
- Rightsize your resources
  - Services have configurations for different performance use cases
  - Use the configuration that meets your requiements
  - Undersizing can lead to performance issues, and oversizing can lead to unnecessary costs
  - Audit refularly for underutilized and unsused resources

- Use Auto Scaling
  - Many AWS services support auto scaling
  - Utilize auto scaling to match supply with demand and optimize costs
  
- Manage Data Transfer Costs
  - Avoid routing traffic over the internet when connecting AWS services
  - Use VPC engpoints to keep traffic on the AWS network
  - Use AWS Direct Connect to optimize data transfer costs between on-premises environment and AWS

- Implement Governance
  - Governance is critical for cost control
  - Develop policies for resource provisioning
  - Implement tagging policy to cateforize resources
  - Use IAM to restrict user and application access
  
- Measure Spending
  - Use tools to understand spending and receive recommendations
  - For example, AWS Cost Explorer and Cost allocation tags
  - Regularly audit to ensure compliance with governance policies and reduce unnecessary spending

----
#### Reduce Compute Costs

- **AWS Compute Services**
  - Amazon EC2
  - AWS Lambda
  - Amazon Elastic Container Service (ECS)
  - AWS Elastic Beanstalk

- **Rightsize EC2 Instances**
  - EC2 instance families:
    - Gerneral purpose
    - Compute optimized
    - Memory optimized
    - Storage optimized
  - Each instance family has many instance types/sizes
  - Each instane type and family is optimized for specific use cases
  - T3 is suitable for web servers and small databases
  - C5 is suitable for batch processing, scientific modeling, and high-performance computing
  - Use CloudWatch to monitor instance usage
  - Downsize underutilized instances to reduce costs

- **Utilize Spot Instances**
  - Spot Instance offer spare EC2 capacity at low rates - in some cases, up to 90% lower than On-Demand rates
  - AWS may interrupt them with a two-minute warning
  - Applications that can handle interuptions can use Spot Instances
  - Use On-demand and Sport Instances to handle a workload
  - If the Spot Instances are interrupted, On-Demand Instances can continue to operate

- **Leverage Reserved Instances and Savings Plans**
  - Reserved Instance and Savings Plans offer significant discounts
  - Require one- or three-year commitment
  - Ideal for long-term projects with predictable usage

- **Implement Audo Scaling**
  - Auto scaling adds or removes instances to handle the workload
  - Prevents under- or over-provisioning

- **Adopt Serverless**
  - With serverless (like AWS Lambda), you only pay when your code runs, reducing costs
  - Create event-driven architectures

- **Consolidate Workloads**
  - Combine multiple workloads into fewer instances
  - Identify underutilized instances and evalute workloads for compatibility
  - Consolicate and monitor performance to ensure expected outcomes

|  |  | 
|---|---|
|![9][1] | ![9][2]|

| Reduce Storage Costs |  | |
|---|---| ---|
|![9][3] | ![9][4]| ![9][5]|
|![9][6] | ![9][7]| ![9][8]|
|![9][9] | ![9][10]| ![9][11]|

| Tools to track cost and usage|  | |
|---|---| ---|
|![9][12] | ![9][13]| ![9][14]|
|![9][15] | ![9][16]| 


[1]: /aws-ws/images/11/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/11/2.png?featherlight=false&width=40pc
[3]: /aws-ws/images/11/3.png?featherlight=false&width=40pc
[4]: /aws-ws/images/11/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/11/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/11/6.png?featherlight=false&width=40pc
[7]: /aws-ws/images/11/7.png?featherlight=false&width=40pc
[8]: /aws-ws/images/11/8.png?featherlight=false&width=40pc
[9]: /aws-ws/images/11/9.png?featherlight=false&width=40pc
[10]: /aws-ws/images/11/10.png?featherlight=false&width=40pc
[11]: /aws-ws/images/11/11.png?featherlight=false&width=40pc
[12]: /aws-ws/images/11/12.png?featherlight=false&width=40pc
[13]: /aws-ws/images/11/13.png?featherlight=false&width=40pc
[14]: /aws-ws/images/11/14.png?featherlight=false&width=40pc
[15]: /aws-ws/images/11/15.png?featherlight=false&width=40pc
[16]: /aws-ws/images/11/16.png?featherlight=false&width=40pc

