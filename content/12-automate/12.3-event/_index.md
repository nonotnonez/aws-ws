---
title : "Event-driven architecture"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 12.3 </b> "
---

#### Event-driven architecture

- In an event-driven architecture, changes within the environment trigger code execution or initiation of workflows.
- Event-driven architecture creates loosely coupled, resilient, and scalable infrastructure.

|  |  |  |
|---|---| ---|
|![12][1] |![12][2] | ![12][3] | 

- You can configure EventBridge to detect EC2 instance termination events. Upon detection, EventBridge can notify an SNS topic and trigger a Lambda function to launch a replacement instance
- You can configure EventBridge to detect S3 object upload events. Upon detection, EventBridge can trigger a Lambda function to process the uploaded object.

[1]: /aws-ws/images/12/3/1.png?featherlight=false&width=50pc
[2]: /aws-ws/images/12/3/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/12/3/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/12/3/4.png?featherlight=false&width=50pc
[5]: /aws-ws/images/12/3/5.png?featherlight=false&width=50pc