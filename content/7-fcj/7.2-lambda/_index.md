---
title : "Lambda"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

#### FCJ: Optimizing EC2 Cost with Lambda

Source: https://000022.awsstudygroup.com/vi/

![72](/aws-ws/images/7/72/100.png?featherlight=false&width=50pc) 

1. Prepairation:

- VPC: **lambda-lab**
  - Auto-generation
  - CIDR: 10.0.0.0/16
- Subnet: 
  - lambda-lab-subnet-public...
  - Enable auto-assign IPv4 Address
- Security Group: **lambda-lab**
  - Inbound rules:  SSH - ICMP - HTTP - HTTPS
- EC2 Instance: **lambda-lab-instance**
  - Key pair: **lambda-lab-key**
  - VPC: lambda-lab-vpc
  - Subnet: lambda-lab-subnet-public
  - SG: lambda-lab
  - Tags:
    - Key: environment-auto - Value: true
- IAM Role: **dc-common-lambda-role**
  - Permision:
    - AmazonEC2FullAccess
    - CloudWatchFullAccess
- Slack:
  - Workspace:  **aws-lambda-labs**
  - New channel: **notification**
  - Webhook URL: COPY Link

2. Create Lambda Function

2.1 Function stop instance:
 
- Lambda 
  - Function name: **dc-common-lambda-auto-stop**
  - Runtime: Python 3.8
  - Architecture: x86_64
  - Existing role:  **dc-common-lambda-role**
    - Configure > Environment variables:
      - Key: **environment_auto**     - Value: **true**
    - Code : **lambda_function** 
      - You need to change webhook_url to receive notifications to Slack.
import boto3
      - Deploy

{{%expand "expand: lambda_funtion" %}}
```sh
import boto3
import os
import json
import urllib3

ec2_resource = boto3.resource('ec2')
http = urllib3.PoolManager()
webhook_url = "https://hooks.slack.com/services/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"

def lambda_handler(event, context):
    environment_auto= os.environ.get('environment_auto')
    if not environment_auto:
        print('Target is empty')
    else:
        instances = ec2_resource.instances.filter(
            Filters=[{'Name': 'tag:environment_auto', 'Values': [environment_auto]}]
        )     
        if not list(instances):
            response = {
                "statusCode":500,
                "body": "Target Instance is None"
            }
        else:
            action_stop = instances.stop()
            sent_slack(action_stop)
            response = {
                "statusCode":200,
                "body": "EC2 Stopping"
            }
        return response


def sent_slack(action_stop):
    list_instance_id = []
    if (len(action_stop)>0) and ("StoppingInstances" in action_stop[0]) and (len(action_stop[0]["StoppingInstances"])>0):
        for i in action_stop[0]["StoppingInstances"] :
            list_instance_id.append(i["InstanceId"])
            msg = "Stopping Instances ID:\n %s" %(list_instance_id)
            data = {"text":msg}
            r = http.request("POST",
                webhook_url,
                body = json.dumps(data),
                headers = {"Content-Type":"application/json"})
    else:
        print ('Not found Instances Stop')

```
{{% /expand%}}

- Cloud watch - Amazone Event Brigde
  - Rules - Create rule
    - Name: **dc-common-lambda-auto-stop**
    - Description: dc-common-lambda-auto-stop
    - Select : **Schedule**
    - Schedule pattern:
      - A schedule that runs at a regular rate, such as every 10 minutes.
      - rate: 9 - Hours
    - Select target:
      - Types:  AWS service
      - Target: Lambda Function
      - Function: dc-common-lambda-auto-stop
    - Create rule

3. Check result

- Lamba Function: dc-common-lambda-auto-start
  - Test - Test event action: Create new event
  - Event name: **instance-stop**
  - Save -> Test
- Slack: Notification
  - check incomming-webhook message
- Instances: lambda-lab-instance
  - Status: Stopped
  
----

#### Learn
Source: 

[Serverless Computing with AWS Lambda](https://www.linkedin.com/learning/serverless-computing-with-aws-lambda/serverless-computing-with-lambdas?u=103729754)

[Learning Amazon Web Services Lambda](https://www.linkedin.com/learning/learning-amazon-web-services-lambda-22774748/create-your-first-aws-lambda?resume=false&u=103729754) (Is Processing)

Challenge:

|  Lambda Function | API Gateway | 
|---|---|
| ![72](/aws-ws/images/7/72/60.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/69.png?featherlight=false&width=40pc) |

Solution:

- Create funtion: Lambda > Funtions
  - Funtion name: **challenge1**  
    - Add Trigger:  API Gateway
      - Create a new API
      - Security: Open
      - Code - Edit index.mjs > Deploy
      - Configuration: Check API Endpoint

- Use Postman to POST value
  - Input valuse 1, value 2

- Clean up your AWS environment
  - Delete all AWS resources
  - Delete all the endpoints
  - **sam delete**

Workflow:

|  Lambda Function | API Gateway | 
|---|---|
| ![72](/aws-ws/images/7/72/61.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/62.png?featherlight=false&width=40pc) |
| ![72](/aws-ws/images/7/72/63.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/64.png?featherlight=false&width=40pc) |
| ![72](/aws-ws/images/7/72/65.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/66.png?featherlight=false&width=40pc) |
| ![72](/aws-ws/images/7/72/67.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/68.png?featherlight=false&width=40pc) |


----
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
| ![72](/aws-ws/images/7/72/1.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/2.png?featherlight=false&width=40pc) |
| ![72](/aws-ws/images/7/72/3.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/4.png?featherlight=false&width=40pc) |
| ![72](/aws-ws/images/7/72/5.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/6.png?featherlight=false&width=40pc) |
| ![72](/aws-ws/images/7/72/7.png?featherlight=false&width=40pc) | 

{{% /expand%}}
{{%expand "Amazon API Gateway & Use Case" %}}

|  AWS API Gateway| 2 | 
|---|---|
| ![72](/aws-ws/images/7/72/12.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/13.png?featherlight=false&width=40pc) |
| ![72](/aws-ws/images/7/72/19.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/20.png?featherlight=false&width=40pc) |
| Use Case |  ![72](/aws-ws/images/7/72/21.png?featherlight=false&width=40pc) |
{{% /expand%}}
{{%expand "Amazon S3" %}}
|  AWS S3| 2 | 
|---|---|
| ![72](/aws-ws/images/7/72/14.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/15.png?featherlight=false&width=40pc) |

{{% /expand%}}
{{%expand "Amazon DynamoDB & SQS & Kinesis" %}}
|DynamoDB| SQS | Kinesis |
|---|---|---|
| ![72](/aws-ws/images/7/72/16.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/17.png?featherlight=false&width=40pc) | ![72](/aws-ws/images/7/72/18.png?featherlight=false&width=40pc)|

{{% /expand%}}
{{%expand "Amazon Cloudwatch" %}}
|CloudWatch| - | 
|---|---|
| ![72](/aws-ws/images/7/72/50.png?featherlight=false&width=40pc) |  ![72](/aws-ws/images/7/72/51.png?featherlight=false&width=40pc) | 

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

- the metrics that provides the Lambda function.

2.5 Adding CloudWatch Logs

- Add log to code : index.mjs
- Test API with Postman
- Monitor - View CloudWatch logs
- Check: CloudWatch > Log Groups > /aws/lambda/**my-lambda-function**
- Login Lambda console > Monitor > Logs

| - |- | 
|---|---|
| ![72][52] | ![72][53] |
| ![72][54] | ![72][55] |



  



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

[52]: /aws-ws/images/7/72/52.png?featherlight=false&width=50pc
[53]: /aws-ws/images/7/72/53.png?featherlight=false&width=50pc
[54]: /aws-ws/images/7/72/54.png?featherlight=false&width=50pc
[55]: /aws-ws/images/7/72/55.png?featherlight=false&width=50pc

[60]: /aws-ws/images/7/72/60.png?featherlight=false&width=50pc
[61]: /aws-ws/images/7/72/61.png?featherlight=false&width=50pc
[62]: /aws-ws/images/7/72/62.png?featherlight=false&width=50pc
[63]: /aws-ws/images/7/72/63.png?featherlight=false&width=50pc