---
title : "Lambda"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

#### AWS Lamda

Source: 

[Serverless Computing with AWS Lambda](https://www.linkedin.com/learning/serverless-computing-with-aws-lambda/serverless-computing-with-lambdas?u=103729754)

[Learning Amazon Web Services Lambda](https://www.linkedin.com/learning/learning-amazon-web-services-lambda-22774748/create-your-first-aws-lambda?resume=false&u=103729754) (Is Processing)

1. AWS Account

Group: 
- Group name: new-admin
- Permission: AdministratorAccess

User: 
- User name: lambdauser
- Group:    new-admin
- Download :    
  - Access key ID
  - Secret access key

2. AWS Lambda

{{%expand "Severless & Lambda" %}}

|  AWS Lambda| 2 | 
|---|---|
| ![72](/aws-ws/images/7/72/1.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/2.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![72](/aws-ws/images/7/72/3.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/4.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![72](/aws-ws/images/7/72/5.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/6.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![72](/aws-ws/images/7/72/7.png?featherlight=false&width=40pc?featherlight=false&width=90pc) | 

{{% /expand%}}
{{%expand "Amazon API Gateway & Use Case" %}}

|  AWS API Gateway| 2 | 
|---|---|
| ![72](/aws-ws/images/7/72/12.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/13.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![72](/aws-ws/images/7/72/19.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/20.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| Use Case |  ![72](/aws-ws/images/7/72/21.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
{{% /expand%}}
{{%expand "Amazon S3" %}}
|  AWS S3| 2 | 
|---|---|
| ![72](/aws-ws/images/7/72/14.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/15.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |

{{% /expand%}}
{{%expand "Amazon DynamoDB & SQS & Kinesis" %}}
|DynamoDB| SQS | Kinesis |
|---|---|---|
| ![72](/aws-ws/images/7/72/16.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/17.png?featherlight=false&width=40pc?featherlight=false&width=90pc) | ![72](/aws-ws/images/7/72/18.png?featherlight=false&width=40pc?featherlight=false&width=90pc)|

{{% /expand%}}
{{%expand "Amazon Cloudwatch" %}}
|CloudWatch| - | 
|---|---|
| ![72](/aws-ws/images/7/72/50.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![72](/aws-ws/images/7/72/51.png?featherlight=false&width=40pc?featherlight=false&width=90pc) | 

{{% /expand%}}

2.1 Lambda Function

- Funtion name: my-lambda-function
- Runtime: Node.js 18.x
- Architecture: x86_64

![72][8]

Lambda: **my-lambda-function**
- Edit Test configuration and Deploy ![72][9]
- Configure Test Event
  - Event name: test-event
  - Template: hello-world ![72][10]
- Run Test - test-event ![72][11]

2.2 Add API Gateway as a trigger to Lambda

Lambda : **my-lambda-function**

- Add trigger:  
  - Select a source:  API Gateway
  - API type: HTTP API
  - Security: Open

- Configuration check:  

| - |- | - |
|---|---|---|
| ![72][22] | ![72][23] | ![72][24] |
| ![72][25] | ![72][26] |

- Edit Code:

| - |- | - | - |
|---|---|---| ---|
| ![72][30] | ![72][31] | ![72][32] | ![72][33] |

2.3 Test your function with Postman

API Development: https://www.postman.com/

| - |- | 
|---|---|
| ![72][40] | ![72][41] |

2.4 Monitor your funtion with CloudWatch metrics
2.5 Adding CloudWatch Logs

- Add log to code
- Test API with Postman
- Check: /aws/lambda/my-lambda-function



[1]: /aws-ws/images/7/72/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/7/72/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/7/72/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/7/72/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/7/72/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/7/72/6.png?featherlight=false&width=50pc
[7]: /aws-ws/images/7/72/7.png?featherlight=false&width=40pc
[8]: /aws-ws/images/7/72/8.png?featherlight=false&width=50pc
[9]: /aws-ws/images/7/72/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/7/72/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/7/72/11.png?featherlight=false&width=50pc
[12]: /aws-ws/images/7/72/12.png?featherlight=false&width=50pc
[13]: /aws-ws/images/7/72/13.png?featherlight=false&width=50pc
[14]: /aws-ws/images/7/72/14.png?featherlight=false&width=50pc

[21]: /aws-ws/images/7/72/21.png?featherlight=false&width=50pc
[22]: /aws-ws/images/7/72/22.png?featherlight=false&width=50pc
[23]: /aws-ws/images/7/72/23.png?featherlight=false&width=50pc
[24]: /aws-ws/images/7/72/24.png?featherlight=false&width=40pc
[25]: /aws-ws/images/7/72/25.png?featherlight=false&width=40pc
[26]: /aws-ws/images/7/72/26.png?featherlight=false&width=50pc

[30]: /aws-ws/images/7/72/30.png?featherlight=false&width=50pc
[31]: /aws-ws/images/7/72/31.png?featherlight=false&width=50pc
[32]: /aws-ws/images/7/72/32.png?featherlight=false&width=50pc
[33]: /aws-ws/images/7/72/33.png?featherlight=false&width=50pc


[40]: /aws-ws/images/7/72/40.png?featherlight=false&width=50pc
[41]: /aws-ws/images/7/72/41.png?featherlight=false&width=50pc