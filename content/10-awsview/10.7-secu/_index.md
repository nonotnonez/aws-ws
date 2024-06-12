---
title : "Security Group and Network ACLs"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 10.7 </b> "
---

#### Best practices


#### NACLs and Security Groups

- Network ACLs
  - Allow or deny inbound and outbound traffic at the subnet level
  - All VPCs have a default NACL that allows all traffic
  - Default NACL can be modified
  - Custom NACLs can be created
  - A NACL can be associated with multiple subnets
  - A subnet can be only be associated with one NACL
  - Each rule has a rule number
  - Rules are evaluated in order, starting with the lowest number
  - NACLs are stateless 
    - This means they do not maintain traffic state information
    - works at the subnet level
  - If inbound traffic is allowed, it can reach all instances in the subnet
  - Do not evaluate traffic routed within a subnet
    - It only applies to traffic entering or leaving the subnet
    
![107](/aws-ws/images/10/107/1.png?featherlight=false&width=90pc)

- For example: here is an EC2 instance in a subnet. A network ACL rule allows specific outbound traffic, but responses to that traffic are not automatically allowed, so traffic must be explicitly allowed in both directions.

![107](/aws-ws/images/10/107/2.png?featherlight=false&width=90pc)

- Security Groups