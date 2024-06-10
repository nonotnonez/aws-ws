---
title : "CloudFormation"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 7.4 </b> "
---

#### LinkedIn
Source: [AWS: Deployment, Provisioning, and Automation](https://www.linkedin.com/learning/aws-deployment-provisioning-and-automation/running-a-cloudformation-template?autoSkip=true&resume=false&u=103729754)

1. Write a CF template: **single_instace.yaml**
{{%expand "Expand code here" %}}
![74](/aws-ws/images/7/74/10.png?featherlight=false&width=50pc) 
{{% /expand%}}

2. Running a CF template
- CloudFormation > Stacks > Create Stack
  - Template is ready > Upload a template file : **single-instance.yaml**
  - Stack name: **WebServer**
  - IamInstallRole: **SSMInstanceRole**
  - InstanceType: **t3.micro**
  - SecurityGroup: *(AllowHTTPFromWorld)*
  - Subnet: *(172.31.32.0/20)*
  - Stack options:
    - Tags: Name  - Nginx
    - Stack failure options:  Roll back all stack resources
  - NEXT > CREATE STACK
  
- Check : 
  - Events
  
![74](/aws-ws/images/7/74/11.png?featherlight=false&width=50pc) 

  - Instances:
  
![74](/aws-ws/images/7/74/12.png?featherlight=false&width=50pc) 
![74](/aws-ws/images/7/74/13.png?featherlight=false&width=50pc) 

3. Updating a CF Stack



----
#### FCJ
Source: https://000037.awsstudygroup.com/

1. Prepairation

- IAM - Users: **CloudFormation-user**
  - Permissions:  AdministratorAccess
  - Key: Name   - Value: Admin User
  - Download Access Key ID & Secret key
- IAM - Roles: **CloudFormation-Role**
  - AWS Service
  - EC2
  - Permission: AdministratorAccess

2. Basic CloudFormation

2.1 Workspace

- Cloud9: **ASG-Cloud9-Workshop**
  - New EC2 Instances
  - Instance type:  t2.micro
  - Platform: Amazon Linux 2
  - Timeout 30 Minutes
  - Network settings:
    - Connection: SSM
    - VPC: cloudformation-vpc
    - Subnet: public-subnet-1
    - CREATE
- Instances: **aws-cloud9-...**
  - Security > Modify IAM role > IAM Role: CloudFormation-Role -> Update IAM Role

- Cloud9 Interface
  - AWS settings - Credentials - Uncheck AWS managed temporary credentials

- Workspace 
  - Install support tools
    - sudo yum -y install jq gettext bash-completion moreutils *(support text command)*
    - pip install cfn-lint *(check CF yaml/json template)*
    - cfn-lint --version
    - pip install taskcat
  - Configure AWS CLI
  
```sh
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
export AZS=($(aws ec2 describe-availability-zones --query 'AvailabilityZones[].ZoneName' --output text --region $AWS_REGION))
```
  - Save to bash_profile
  
 ```sh
echo "export ACCOUNT_ID=$ACCOUNT_ID" | tee -a ~/.bash_profile
echo "export AWS_REGION=$AWS_REGION" | tee -a ~/.bash_profile
echo "export AZS=${AZS[@]}" | tee -a ~/.bash_profile
aws configure set default.region $AWS_REGION
aws configure get default.region
 ``` 
  - Check IAM Role

```sh
aws sts get-caller-identity --query Arn | grep CloudFormation-Role -q && echo "IAM role valid" || echo "IAM role NOT valid"
```

2.2 CloudFormation Template

- AWS CLoud 9 > File > New File : **singleec2instance.yaml**
  - Parameters
```sh
AWSTemplateFormatVersion: "2010-09-09"
Description: "Deploy Single EC2 Linux Instance as part of MGT312 Workshop"
Parameters:
  EC2InstanceType:
    AllowedValues:
      - t3.nano
      - t3.micro
      - t3.small
      - t3.medium
      - t3.large
      - t3.xlarge
      - t3.2xlarge
      - m5.large
      - m5.xlarge
      - m5.2xlarge
    Default: t3.small
    Description: Amazon EC2 instance type
    Type: String
  LatestAmiId:
    Type: 'AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>'
    Default: "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
  SubnetID:
    Description: ID of a Subnet.
    Type: AWS::EC2::Subnet::Id
  SourceLocation:
    Description : The CIDR IP address range that can be used to RDP to the EC2 instances
    Type: String
    MinLength: 9
    MaxLength: 18
    Default: 0.0.0.0/0
    AllowedPattern: "(\\d{1,3})\\.(\\d{1,3})\\.(\\d{1,3})\\.(\\d{1,3})/(\\d{1,2})"
    ConstraintDescription: must be a valid IP CIDR range of the form x.x.x.x/x.
  VPCID:
    Description: ID of the target VPC (e.g., vpc-0343606e).
    Type: AWS::EC2::VPC::Id

```
  - Resource: Security Group

```sh

Resources:
  EC2InstanceSG:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: EC2 Instance Security Group
      VpcId: !Ref 'VPCID'
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: !Ref SourceLocation
```

  - Resource: Instance Role

```sh
  SSMInstanceRole:
    Type : AWS::IAM::Role
    Properties:
      Policies:
        - PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Action:
                  - s3:GetObject
                Resource:
                  - !Sub 'arn:aws:s3:::aws-ssm-${AWS::Region}/*'
                  - !Sub 'arn:aws:s3:::aws-windows-downloads-${AWS::Region}/*'
                  - !Sub 'arn:aws:s3:::amazon-ssm-${AWS::Region}/*'
                  - !Sub 'arn:aws:s3:::amazon-ssm-packages-${AWS::Region}/*'
                  - !Sub 'arn:aws:s3:::${AWS::Region}-birdwatcher-prod/*'
                  - !Sub 'arn:aws:s3:::patch-baseline-snapshot-${AWS::Region}/*'
                Effect: Allow
          PolicyName: ssm-custom-s3-policy
      Path: /
      ManagedPolicyArns:
        - !Sub 'arn:${AWS::Partition}:iam::aws:policy/AmazonSSMManagedInstanceCore'
        - !Sub 'arn:${AWS::Partition}:iam::aws:policy/CloudWatchAgentServerPolicy'
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
        - Effect: "Allow"
          Principal:
            Service:
            - "ec2.amazonaws.com"
            - "ssm.amazonaws.com"
          Action: "sts:AssumeRole"
  SSMInstanceProfile:
    Type: "AWS::IAM::InstanceProfile"
    Properties:
      Roles:
      - !Ref SSMInstanceRole

```
  - Resource: EC2 instance

```sh
Resources:
  EC2InstanceSG:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: EC2 Instance Security Group
      VpcId: !Ref 'VPCID'
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: !Ref SourceLocation

```

  - Outputs

```sh
Outputs:
  EC2InstancePrivateIP:
    Value: !GetAtt 'EC2Instance.PrivateIp'
    Description: Private IP for EC2 Instances

```

- Check location: /home/ec2-user-environment
- Check validate with cfn-lini: **cfn-lint singleec2instance.yaml**
- Create CloudFormation Stack:
  - Update role : CloudFormation-Role
    - Attach policies:  **AmazonEC2FullAccess** vs **IAMFullAccess**
- Repair VPC & Public Subnet
  - VPCs: cloudformation-vpc  10.0.0.0/16  > VPCID
  - Subnet: cloudformation-subnet-public  10.0.0.0/20 > SubnetID
- Cloud9 run command:
```sh
aws cloudformation create-stack --stack-name asg-cloudformation-stack --template-body file://singleec2instance.yaml --parameters ParameterKey=SubnetID,ParameterValue=subnet-04c111ad3987c5350 ParameterKey=VPCID,ParameterValue=vpc-0e3f81c05d3920198 --capabilities CAPABILITY_IAM --region us-east-1

```
- Check CF template
  - CloudFormation - Stacks - Stack details  
    - Events
    - Resources
    - Parameters
    - Outputs
- Check Instances
  - EC2 - Instances - Details

|  CF | |
|---|---|
|![74](/aws-ws/images/7/74/1.png?featherlight=false&width=50pc)  | ![84](/aws-ws/images/7/74/2.png?featherlight=false&width=50pc) |

----

#### Advanced CF

3.1 Create Lambda Function
- IAM Role: 
  - AWS Service - Use case: **Lambda**
  - Create Policy > Name: **ssh-key-policy** > Create Policy

```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateKeyPair",
                "ec2:DescribeKeyPairs",
                "ssm:PutParameter"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DeleteKeyPair",
                "ssm:DeleteParameter"
            ],
            "Resource": "*"
        }
    ]
}

```
  - Role name: **ssh-key-gen-role**  > Create role
- AWS Lambda > Create function
  - Author from scratch
  - Funtion name: ssh-key-gen
  - Runtime: Python 3.9
  - Architeture: x86_64
  - Excution role > Use an existing role: **ssh-key-gen-role**
  - CREATE FUNTION
{{%expand "Input code below" %}}

```sh
"""
This lambda implements the custom resource handler for creating an SSH key
and storing in in SSM parameter store.

e.g.

SSHKeyCR:
    Type: Custom::CreateSSHKey
    Version: "1.0"
    Properties:
      ServiceToken: !Ref FunctionArn
      KeyName: MyKey

An SSH key called MyKey will be created.

"""

from json import dumps
import sys
import traceback
import urllib.request

import boto3


def log_exception():
    "Log a stack trace"
    exc_type, exc_value, exc_traceback = sys.exc_info()
    print(repr(traceback.format_exception(
        exc_type,
        exc_value,
        exc_traceback)))


def send_response(event, context, response):
    "Send a response to CloudFormation to handle the custom resource lifecycle"

    responseBody = { 
        'Status': response,
        'Reason': 'See details in CloudWatch Log Stream: ' + \
            context.log_stream_name,
        'PhysicalResourceId': context.log_stream_name,
        'StackId': event['StackId'],
        'RequestId': event['RequestId'],
        'LogicalResourceId': event['LogicalResourceId'],
    }

    print('RESPONSE BODY: \n' + dumps(responseBody))

    data = dumps(responseBody).encode('utf-8')
    
    req = urllib.request.Request(
        event['ResponseURL'], 
        data,
        headers={'Content-Length': len(data), 'Content-Type': ''})
    req.get_method = lambda: 'PUT'

    try:
        with urllib.request.urlopen(req) as response:
            print(f'response.status: {response.status}, ' + 
                  f'response.reason: {response.reason}')
            print('response from cfn: ' + response.read().decode('utf-8'))
    except urllib.error.URLError:
        log_exception()
        raise Exception('Received non-200 response while sending ' +\
            'response to AWS CloudFormation')

    return True


def custom_resource_handler(event, context):
    '''
    This function creates a PEM key, commits it as a key pair in EC2, 
    and stores it, encrypted, in SSM.

    To retrieve the key with currect RSA format, you must use the command line: 

    aws ssm get-parameter \
        --name <KEYNAME> \
        --with-decryption \
        --region <REGION> \
        --output text

    Copy the values from (and including) -----BEGIN RSA PRIVATE KEY----- to 
    -----END RSA PRIVATE KEY----- into a file.

    To use it, change the permissions to 600
    Ensure to bundle the necessary packages into the zip stored in S3

    '''
    print("Event JSON: \n" + dumps(event))

    # session = boto3.session.Session()
    # region = session.region_name

    # Original
    # pem_key_name = os.environ['KEY_NAME']
    
    pem_key_name = event['ResourceProperties']['KeyName']

    response = 'FAILED'
    
    ec2 = boto3.client('ec2')

    if event['RequestType'] == 'Create':
        try:
            print("Creating key name %s" % str(pem_key_name))

            key = ec2.create_key_pair(KeyName=pem_key_name)
            key_material = key['KeyMaterial']
            ssm_client = boto3.client('ssm')
            param = ssm_client.put_parameter(
                Name=pem_key_name, 
                Value=key_material, 
                Type='SecureString')

            print(param)
            print(f'The parameter {pem_key_name} has been created.')

            response = 'SUCCESS'

        except Exception as e:
            print(f'There was an error {e} creating and committing ' +\
                f'key {pem_key_name} to the parameter store')
            log_exception()
            response = 'FAILED'

        send_response(event, context, response)

        return

    if event['RequestType'] == 'Update':
        # Do nothing and send a success immediately
        send_response(event, context, response)
        return

    if event['RequestType'] == 'Delete':
        #Delete the entry in SSM Parameter store and EC2
        try:
            print(f"Deleting key name {pem_key_name}")

            ssm_client = boto3.client('ssm')
            rm_param = ssm_client.delete_parameter(Name=pem_key_name)

            print(rm_param)

            _ = ec2.delete_key_pair(KeyName=pem_key_name)

            response = 'SUCCESS'
        except Exception as e:
            print(f"There was an error {e} deleting the key {pem_key_name} ' +\
            from SSM Parameter store or EC2")
            log_exception()
            response = 'FAILED'
         
        send_response(event, context, response)


def lambda_handler(event, context):
    "Lambda handler for the custom resource"

    try:
        return custom_resource_handler(event, context)
    except Exception:
        log_exception()
        raise

```
{{% /expand%}}

  - Code > Deploy
  - Copy Funtion ARN
  
3.2 Create Stack

- CloudFormation > Create stack > With new resource (standard)
  - New file: **custom_resource_cfn_mr.yml**
{{%expand "expand code : " %}}
```sh
Parameters:
    SourceAccessCIDR:
      Type: String
      Description: The CIDR IP range that is permitted to access the instance. We recommend that you set this value to a trusted IP range.
      Default: 0.0.0.0/0
    SSHKeyName:
      Type: String
      Description: The name of the key that will be created
      Default: MyKey01
    AMIID:
      Type: String
      Description: The AMI ID that will be used to create EC2 instance
      Default: AMIID
    VPCPublicSubnet:
      Type: AWS::EC2::Subnet::Id
      Description: Choose a public subnet in the selected VPC
    VPC:
      Type: AWS::EC2::VPC::Id
      Description: The VPC in which to launch the EC2 instance. We recommend you choose your default VPC.
    FunctionArn:
      Type: String
      Description: The ARN of the lambda function that implements the custom resource
Resources:
  SSHKeyCR:
      Type: Custom::CreateSSHKey
      Version: "1.0"
      Properties:
        ServiceToken: !Ref FunctionArn
        KeyName: !Ref SSHKeyName
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      KeyName: !Ref SSHKeyName
      InstanceType: t2.micro
      ImageId: !Ref AMIID
      IamInstanceProfile: !Ref EC2InstanceProfile
      SecurityGroupIds:
        - !Ref EC2InstanceSG
      SubnetId: !Ref VPCPublicSubnet
      Tags:
        -
          Key: Name
          Value: Cfn-Workshop-Reinvent-2018-Lab1
    DependsOn: SSHKeyCR
  EC2InstanceProfile:
      Type: AWS::IAM::InstanceProfile
      Properties:
        Roles:
        - Ref: EC2InstanceRole
        Path: "/"
      DependsOn: EC2InstanceRole
  EC2InstanceRole:
    Type: AWS::IAM::Role
    Properties:
      Policies:
      - PolicyDocument:
          Version: '2012-10-17'
          Statement:
          - Action:
            - s3:GetObject
            Resource: "*"
            Effect: Allow
        PolicyName: s3-policy
      - PolicyDocument:
          Version: '2012-10-17'
          Statement:
          - Action:
            - logs:CreateLogStream
            - logs:GetLogEvents
            - logs:PutLogEvents
            - logs:DescribeLogGroups
            - logs:DescribeLogStreams
            - logs:PutRetentionPolicy
            - logs:PutMetricFilter
            - logs:CreateLogGroup
            Resource:
            - arn:aws:logs:*:*:*
            - arn:aws:s3:::*
            Effect: Allow
        PolicyName: cloudwatch-logs-policy
      - PolicyDocument:
          Version: '2012-10-17'
          Statement:
          - Action:
            - ec2:AssociateAddress
            - ec2:DescribeAddresses
            Resource:
            - "*"
            Effect: Allow
        PolicyName: eip-policy
      Path: "/"
      AssumeRolePolicyDocument:
        Statement:
        - Action:
          - sts:AssumeRole
          Principal:
            Service:
            - ec2.amazonaws.com
          Effect: Allow
        Version: '2012-10-17'
  EC2InstanceSG:
    Type: AWS::EC2::SecurityGroup
    Properties:
        GroupDescription: This should allow you to SSH to the instance from your location
        VpcId: !Ref VPC
        Tags:
        - Key: Name
          Value: !Sub ${AWS::StackName}
        SecurityGroupIngress:
          -
            Description: This should allow you to SSH from your location into an EC2 instance
            CidrIp: !Ref SourceAccessCIDR
            FromPort: 22
            IpProtocol: tcp
            ToPort: 22
Outputs:
  MyEC2InstanceDNSName:
    Description: The DNSName of the new EC2 instance
    Value: !GetAtt MyEC2Instance.PublicDnsName
  MyEC2InstancePublicIP:
    Description: The Public IP address of the newly created EC2 instance 
    Value: !GetAtt MyEC2Instance.PublicIp

```
{{% /expand%}}

  - Preresuisite - Prepare template
    - Template is ready
    - Upload a template file: custom_resource_cfn_mr.yml
    - Stack name: **ssh-key-gen-cr**
    - Create stack
      - **AMI ID**:
      - FuntionArn:
      - SSHKeyName: MyKey01
      - SourceAccessCIDR: 0.0.0.0/0
      - VPC:
      - VPCPublicSubnet:
      - I acknowledge ...> Submit
      - CREATE STACK
- EC2 - Launch Instances : Copy **AMI ID**

- CloudFormation  > Stacks > **ssh-key-gen-cr**
  - Events
- Check EC2 instance created

3.3 Connect EC2 instances

- AWS Systems Manager - Parameter Store
  - Name: **MyKey01**
    - Overview > Value > Copy code and Save to : **cloudformation.pem**
- Use PuTTY Generator load key and Save Private key
  - Conect to EC2 Instance
    - ifconfig -a
    - ping amazon.com -c5

3.4  Mapping and StackSets



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