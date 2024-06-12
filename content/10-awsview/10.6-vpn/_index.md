---
title : "VPN and Direct Connect"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 10.6 </b> "
---

#### Best practices

Use site-to-site VPN if you need a cost-effective solution and your applications aren't demanding.

Prefer Direct Connect if you need a dedicated connection and your applications are data/performance-intensive

#### VPN and Direct Connect

- **AWS Site-to-Site VPN**
  - Secure connect between on-premises equiment and your VPCs
  - Created within your AWS account and attached to a VPC
  - Virtual private gateway is the termination point on the AWS side
  - Created within your AWS account and attached to a VPC
  - The customer gateway is a physical device or software application on your side
  - Each site-to-site VPN has two tunnels, each terminating in a different Availability Zone
  - When one tunnel becomes unavailable, network traffic is automatically routed to the other tunnel
  
![106](/aws-ws/images/10/106/1.png?featherlight=false&width=50pc)

- **Architectural Considerations**
  - Performance
  - Cost
  - Security

- **AWS Direct Connect**
  - Dedicated connection between your on-premises network and AWS
  - Bypass the public internet
  - Consider when you have applications that need high performance and low latency

- AWS Direct Connect Pricing
  - Capacity - maximum rate that data can be transferred   
  - Port hours - time that a port is provisioned 
  - https://aws.amazon.com/directconnect/pricing/  

Use site-to-site VPN if you need a cost-effective solution and your applications aren't demanding.

Prefer Direct Connect if you need a dedicated connection and your applications are data/performance-intensive