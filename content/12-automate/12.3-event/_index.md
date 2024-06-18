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

- **Demo**
- Amazon EventBridge - Create rule
- Define rule detail :
  - Name: **process-s3-objects-with-lambda**
  - Event bus: default
  - Rule type:
    - Rule with an event pattern
    - NEXT
- Build event pattern
  - Event source
    - AWS events
  - Sampe event - optional
  - Event pattern
    - AWS service
      - **S3**
    - Event type:
      - Amazon S3 Event Notification
    - Event type specification 1
      - Specific event(s)
        - **Object Created**
     - Event type specification 2
      - Specific event(s)
        - **processing-bucket-1**    ![12][5]
  - Select target(s)
    - Target 1
      - Target types
        - Select a target: **Lambda function**
        - Function: **Do_nothing**
     - Target 2
      - Target types
        - Select a target: **SNS topic**
        - Topic : **Topic1**
        - NEXT
  - Configure tags - NEXT - CREATE rule 

|  |  |  |
|---|---| ---|
|![12][6] |![12][7] | 

  - The rule has been created and it will be invoked each time an object is uploaded to the specified bucket. The rule will then forward the event to the configured targets, the Lambda function and the SNS topic. This rule is based on an event pattern.

  - You can also create a schedule rule. Let's say we need to start an EC2 instance, which is a batch-processing server, at 11 p.m. every night and stop it the following morning at 6 a.m

- Schedules:
  - Specfify schedule detail
    - Schedule name: **Start_batch_server_11PM** -> NEXT ![12][8]
  - Select target
    - All APIs - **Amazone EC2** > **Startinstances**
      - Provice instance IDs ![12][9]
  - Settings
    - Action after schedule completion: **NONE**
    - DLQ - Retry policy: Turn off Retry
    - Permissions - Select an existing role: **EventBridge_EC2_Startinstance_Role** ![12][10]
    - NEXT - CREATE SCHEDULE ![12][11]
  


[1]: /aws-ws/images/12/3/1.png?featherlight=false&width=50pc
[2]: /aws-ws/images/12/3/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/12/3/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/12/3/4.png?featherlight=false&width=50pc
[5]: /aws-ws/images/12/3/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/12/3/6.png?featherlight=false&width=40pc
[7]: /aws-ws/images/12/3/7.png?featherlight=false&width=50pc
[8]: /aws-ws/images/12/3/8.png?featherlight=false&width=40pc
[9]: /aws-ws/images/12/3/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/12/3/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/12/3/11.png?featherlight=false&width=50pc
