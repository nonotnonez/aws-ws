---
title : "Cloud 9"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---

#### Use the Cloud IDE in the browser with AWS Cloud9
https://000049.awsstudygroup.com/

1. Create Cloud9 instance
![7][1]

Create environment
-   Name: **clou9instance**
-   Environment type: Select **Create a new EC2 instance for environment (direct access)**.
-   Instance type : Select **t2.micro**.
-   Platform : Select **Amazon Linux2**.
-   Cost-saving setting : select **After 30 minutes**. Allows to automatically stop Cloud9 instances to save costs.
-   Create environment

2. Dashboard interface
3. Using AWS CLI

```sh
aws ec2 describe-instances
```
4. Clean up resources

In AWS Cloud9 > Your environments : Select **clou9instance** and click **Delete**

[1]: /aws-ws/images/7/71/1.png?featherlight=false&width=50pc

