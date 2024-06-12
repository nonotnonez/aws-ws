---
title : "VPC and Subnets"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 10.5 </b> "
---


#### Best practices

**Overviews**

Users in the other account can deploy resources in the shared subnet.

**Process**

- VPC > Your VPCs > Create VPC
  - Name tag: **ProdVPC**
  - IPv4 CIDR: 10.0.0.0/16
  - Tags
    - Key: Name
    - Value: ProVPC
      - CREATE VPC
      
- VPC > Subnets > Create Subnet
  - VPC ID: ProVPC
  - Subnet name:   **Subnet-A** 
  - Availability Zone: us-east-1a
  - IPv4 VPC CIDR block: 10.0.0.0/16
  - IPv4 subnet CIDR block: 10.0.1.0/24
  - Tags
    - Key: Name
    - Value: Subnet-A

- VPC > Subnets > Create Subnet
  - VPC ID: ProVPC
  - Subnet name:   **Subnet-B** 
  - Availability Zone: us-east-1b
  - IPv4 VPC CIDR block: 10.0.0.0/16
  - IPv4 subnet CIDR block: 10.0.2.0/24
  - Tags
    - Key: Name
    - Value: Subnet-B

- **Resource Acess Manager** 
  - Setting
    - Enable sharing with AWS Organizations
  - Resource shares > Create resource share
    - Name: **Shared_subnet**
    - Resources: Subnets
      - Check Subnet-A
        - NEXT
    - Managed permission for ec2:Subnet
      - NEXT
    - Grant access to principals 
      - Allow sharing only within your organization
      - Display organizational structure
        - Select OU or Account
          - NEXT
          - CREATE RESOURCE SHARE
          
![105](/aws-ws/images/10/105/3.png?featherlight=false&width=50pc)

#### Vitual Private Cloud (VPC)
- Logically isolated virutal network
- Resides in a singel AWS Region
- Contains subnets

**Subnets**
- Range of IP address in the VPC
- Resides in a single Availability Zone

![105](/aws-ws/images/10/105/1.png?featherlight=false&width=50pc)

**Designing VPCs and Subnets**
- **Network isolation**
  - VPCs are isolated
  - Use VPCs to segregate resources
- **IP addressing**
  - Choose a `CIDR block` that is large enough
  - VPCs are isolated but can be peered if their IP ranges don't overlap
  - Subnets can be `public` or `private`
    - Public subnets
      - Have a route to an internet gateway allowing access to the internet
      - Allow incomming connections from the internet
      - Are typically used by web servers
    - Private subnets
      - Have a route to a network address translation (`NAT`) device allowing to the internet
      - Do not allow connections originating from the internet
      - Are typically used by database servers
- **Security**
  - Use security groups and network access control list (`NACLs`) to filter inbound and outbound traffic
  - Security groups are applied to instances
  - NACLs are applied to subnets
- **Connectivity**
  - `VPC peering` allows direct connectivity between VPCs
  - Peering VPCs may belong to the same or different AWS accounts
  - Use `AWS VPN` or `AWS Direct Connect` to connect with an on-premises environment
  - Use AWS VPN to create an IPsec site-to-site VPN with your on-premises network
  - Use AWS Direct Connect to create a dedicated private connection with your on-premises network
    - **High Availability**
      - Spread resources in multiple subnets to make them highly available and fault tolerant
      - Use services like `Elastic Load Balancing` and `Auto Scaling`
- **Compliance**
  - Consider regulatory requirements, such as data residency
    - For example, if you're a bank operating in the U.K. you may be required to host your resources and data in an `AWS region` in the U.K.

![105](/aws-ws/images/10/105/2.png?featherlight=false&width=40pc)

- Architectual Considerations
  - You can bring your IPv4 and IPv6 addresses to your AWS account
  - Subnets can be shared with other accounts in the same AWS organization   



      