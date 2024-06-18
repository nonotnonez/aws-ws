---
title : "Auto Scalling"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 12.6 </b> "
---

#### Auto Scaling

- AWS Auto Scaling automatically scales resource based on your scaling strategy

- Amazon EC2 Auto Scaling can be used to scale EC2 instances.

|  |  |  |
|---|---| ---|
|![12][1] |![12][2] | ![12][3] | 
|![12][4] |![12][5] | ![12][6] | 
|![12][7] |![12][8] | ![12][9] | 
|![12][10] | ![12][11] | 

- **Demo**
- AWS Auto Scaling - Create a scaling plan
  - Find scalable resources: 
    - Choose a method: **Search by tag**
    - Key: **Environment**
    - Value: **Production**
    - NEXT
  - Specify scaling strategy
    - Name: **Scale-DynamoDB**
    - NEXT
  - Configure advanced settings
    - Check: Replace external scaling policies
    - Uncheck: Disable scale-in
    - NEXT
    - CREATE SCALING PLAN

|  |  |  |
|---|---| ---|
|![12][12] |![12][13] | ![12][14] | 



| Amazon EC2 Auto Scaling |  |  |
|---|---| ---|
|![12][15] |![12][16] |

[1]: /aws-ws/images/12/6/1.png?featherlight=false&width=50pc
[2]: /aws-ws/images/12/6/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/12/6/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/12/6/4.png?featherlight=false&width=50pc
[5]: /aws-ws/images/12/6/5.png?featherlight=false&width=50pc
[6]: /aws-ws/images/12/6/6.png?featherlight=false&width=50pc
[7]: /aws-ws/images/12/6/7.png?featherlight=false&width=50pc
[8]: /aws-ws/images/12/6/8.png?featherlight=false&width=50pc
[9]: /aws-ws/images/12/6/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/12/6/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/12/6/11.png?featherlight=false&width=50pc
[12]: /aws-ws/images/12/6/12.png?featherlight=false&width=50pc
[13]: /aws-ws/images/12/6/13.png?featherlight=false&width=50pc
[14]: /aws-ws/images/12/6/14.png?featherlight=false&width=50pc
[15]: /aws-ws/images/12/6/15.png?featherlight=false&width=50pc
[16]: /aws-ws/images/12/6/16.png?featherlight=false&width=50pc
[17]: /aws-ws/images/12/6/17.png?featherlight=false&width=50pc
[18]: /aws-ws/images/12/6/18.png?featherlight=false&width=50pc
[19]: /aws-ws/images/12/6/19.png?featherlight=false&width=50pc