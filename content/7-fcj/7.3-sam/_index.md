---
title : "SAM"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

#### AWS Serverless Application Model

Source: [Learning Amazon Web Services Lambda](https://www.linkedin.com/learning/learning-amazon-web-services-lambda-22774748/introduction-to-aws-sam?resume=false&u=103739754) (Is processing)

Github: https://github.com/LinkedInLearning/learning-amazon-web-services-lambda-4411402

Challenge:

| - |- | 
|---|---|
| ![72][30] | ![72][31] |
| ![72][32] | ![72][33] |

Solution:

- Create funtions:  **RollADiceWithSidesFunction**
- Create folder: **roll-dice-sides/app.mjs**
- Start local:
  - sam build
  - sam local start-api
  - Running on **http://127.0.0.1:3000**
  - curl http://localhost:3000/roll
  - curl http://localhost:3000/roll\?sides\=4
  - curl http://localhost:3000/roll\?sides\=20

![72][40] 
- Deploy to the Cloud
  - sam deploy --guild (or use samconfig.toml: **same deploy**)

| - |- | 
|---|---|
| ![72][41] | ![72][42] |

- CloudFormation check

| - |- | 
|---|---|
| ![72][43] | ![72][44] |


Source: https://github.com/LinkedInLearning/learning-amazon-web-services-lambda-4411402/tree/05_05/sam-first-project

{{%expand "app.mjs" %}}
```sh
export const lambdaHandler = async (event) => {
	console.log('Roll dice with sides run');

	let sides = 6;
	if (event.queryStringParameters && event.queryStringParameters.sides) {
		sides = event.queryStringParameters.sides;
	}

	const result = rollDice(sides);

	const message = `The result is ${result}, you rolled a dice of ${sides} sides.`;

	try {
		return {
			statusCode: 200,
			body: JSON.stringify({
				message: message,
			}),
		};
	} catch (err) {
		console.log(err);
		return err;
	}
};

function rollDice(sides) {
	const randomNumber = Math.floor(Math.random() * sides) + 1;

	return randomNumber;
}
```
{{% /expand%}}



{{%expand "templete.yaml" %}}
```sh
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: >
  sam-first-project

  Sample SAM Template for sam-first-project

# More info about Globals: https://github.com/awslabs/serverless-application-model/blob/master/docs/globals.rst
Globals:
  Function:
    Timeout: 3

Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function # More info about Function Resource: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#awsserverlessfunction
    Properties:
      CodeUri: hello-world/
      Handler: app.lambdaHandler
      Runtime: nodejs18.x
      Architectures:
        - x86_64
      Events:
        HelloWorld:
          Type: Api # More info about API Event Source: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#api
          Properties:
            Path: /hello
            Method: get

  RollADiceFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: roll-dice/
      Handler: app.lambdaHandler
      Runtime: nodejs18.x
      Events:
        RollDiceApi:
          Type: Api
          Properties:
            Path: /dice
            Method: get

  RollADiceWithSidesFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: roll-dice-sides/
      Handler: app.lambdaHandler
      Runtime: nodejs18.x
      Events:
        RollDiceSideApi:
          Type: Api
          Properties:
            Method: get
            Path: /roll

Outputs:
  # ServerlessRestApi is an implicit API created out of Events key under Serverless::Function
  # Find out more about other implicit resources you can reference within SAM
  # https://github.com/awslabs/serverless-application-model/blob/master/docs/internals/generated_resources.rst#api
  HelloWorldApi:
    Description: 'API Gateway endpoint URL for Prod stage for Hello World function'
    Value: !Sub 'https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/Prod/hello/'
  HelloWorldFunction:
    Description: 'Hello World Lambda Function ARN'
    Value: !GetAtt HelloWorldFunction.Arn
  HelloWorldFunctionIamRole:
    Description: 'Implicit IAM Role created for Hello World function'
    Value: !GetAtt HelloWorldFunctionRole.Arn
```
{{% /expand%}}

---
Overview 
{{%expand "IAC" %}}
|CloudWatch| - | 
|---|---|
| ![73](/aws-ws/images/7/73/1.png?featherlight=false&width=40pc?featherlight=false&width=50pc) |  ![73](/aws-ws/images/7/73/2.png?featherlight=false&width=40pc?featherlight=false&width=50pc) | 
| ![73](/aws-ws/images/7/73/3.png?featherlight=false&width=40pc?featherlight=false&width=50pc) |  ![73](/aws-ws/images/7/73/4.png?featherlight=false&width=40pc?featherlight=false&width=50pc) | 


{{% /expand%}}

1. Install and configure AWS SAM

- Install AWS CLI
  - **aws --version**
  - **sam --version**

- Create an AWS SAM Project
  - sam init
{{%expand "Configure" %}}
|MACOS| sam-first-project | 
|---|---|
| ![73](/aws-ws/images/7/73/5.png?featherlight=false&width=40pc?featherlight=false&width=50pc) |  ![73](/aws-ws/images/7/73/6.png?featherlight=false&width=40pc?featherlight=false&width=50pc) | 

{{% /expand%}}

    - 1 - AWS Quick Start Templates
      - 1 - Hello World Example
      - 11 - nodejs18.x
      - 1 - Zip
      - 2 - Hello World Example TypeScript
      - No - X-RAY
      - No - CloudWatch
      - Project name: **sam-first-project**

  - ls :  sam-first-project
  - cd sam-first-project
  - code .



2. Lambda functions with AWS SAM

- sam-first-project
  - sam build
  - -> .aws-sam
  - sam local invoke HelloWorldFunction --event events/event.json
    - change name and rebuild to check 
  - sam local start-api
  - Testing
  
{{%expand "Expand" %}}

|  AWS SAM | - | 
|---|---|
| ![73](/aws-ws/images/7/73/7.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/8.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![73](/aws-ws/images/7/73/9.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/10.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![73](/aws-ws/images/7/73/11.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/12.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |


{{% /expand%}}

3. Funtion and API gateway with SAM

- Create funtions: RollADiceFuntion
- Create folder : roll-dice
  - create file /roll-dice/app.mjs
- Build template: sam build
- Run funtions: sam local invoke RollADiceFuntion --event events/event.json

{{%expand "Expand" %}}
|  Funtions | - | 
|---|---|
| ![73](/aws-ws/images/7/73/13.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/14.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![73](/aws-ws/images/7/73/15.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/16.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |

{{% /expand%}}

4. Deploy your AWS SAM project to the cloud

- sam build

- sam deploy --guided

- CloudFormation check

- Check template.yaml output

- Check API

- Test with Postman

- See funtion metric and logs : sam logs --name:RollADiceFunction --stack-name same-first-project --region eu-west-1 --tail
- Check samconfig.toml
- Excute Postman test
- Check Sam logs again

{{%expand "Expand" %}}
|  Build | Deploy | 
|---|---|
| ![73](/aws-ws/images/7/73/17.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/18.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![73](/aws-ws/images/7/73/19.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/20.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![73](/aws-ws/images/7/73/21.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  - |
| ![73](/aws-ws/images/7/73/22.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/23.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |
| ![73](/aws-ws/images/7/73/24.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |  ![73](/aws-ws/images/7/73/25.png?featherlight=false&width=40pc?featherlight=false&width=90pc) |

{{% /expand%}}




[1]: /aws-ws/images/7/73/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/7/73/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/7/73/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/7/73/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/7/73/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/7/73/6.png?featherlight=false&width=50pc
[7]: /aws-ws/images/7/73/7.png?featherlight=false&width=40pc
[8]: /aws-ws/images/7/73/8.png?featherlight=false&width=50pc
[9]: /aws-ws/images/7/73/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/7/73/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/7/73/11.png?featherlight=false&width=50pc
[12]: /aws-ws/images/7/73/12.png?featherlight=false&width=50pc
[13]: /aws-ws/images/7/73/13.png?featherlight=false&width=50pc
[14]: /aws-ws/images/7/73/14.png?featherlight=false&width=50pc
[15]: /aws-ws/images/7/73/15.png?featherlight=false&width=50pc
[16]: /aws-ws/images/7/73/16.png?featherlight=false&width=50pc
[17]: /aws-ws/images/7/73/17.png?featherlight=false&width=50pc
[18]: /aws-ws/images/7/73/18.png?featherlight=false&width=50pc
[19]: /aws-ws/images/7/73/19.png?featherlight=false&width=50pc
[20]: /aws-ws/images/7/73/20.png?featherlight=false&width=50pc
[21]: /aws-ws/images/7/73/21.png?featherlight=false&width=50pc
[22]: /aws-ws/images/7/73/22.png?featherlight=false&width=50pc
[23]: /aws-ws/images/7/73/23.png?featherlight=false&width=50pc
[24]: /aws-ws/images/7/73/24.png?featherlight=false&width=40pc
[25]: /aws-ws/images/7/73/25.png?featherlight=false&width=40pc



[30]: /aws-ws/images/7/73/30.png?featherlight=false&width=50pc
[31]: /aws-ws/images/7/73/31.png?featherlight=false&width=50pc
[32]: /aws-ws/images/7/73/32.png?featherlight=false&width=50pc
[33]: /aws-ws/images/7/73/33.png?featherlight=false&width=50pc


[40]: /aws-ws/images/7/73/40.png?featherlight=false&width=50pc
[41]: /aws-ws/images/7/73/41.png?featherlight=false&width=50pc
[42]: /aws-ws/images/7/73/42.png?featherlight=false&width=50pc
[43]: /aws-ws/images/7/73/43.png?featherlight=false&width=50pc
[44]: /aws-ws/images/7/73/43.png?featherlight=false&width=50pc