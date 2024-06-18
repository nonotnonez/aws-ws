---
title : "Lambda"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 12.2 </b> "
---

#### Going serverless with Lambda

- AWS Lambda lets you run code without provisioning servers.
- Lambda abstracts the underlying infratructure.

|  |  |  |
|---|---| ---|
|![122][1] |![122][2] | ![122][3]|
|![122][4] |

- **Demo**:
- AWS Services > Compute > Lambda > **Create functions**
  - Blue/Green
  - Buleprint name: Get S3 object
  - Function name: **retrieve-s3-data**
  - Runtime: Python
  - Excution role: 
    - Create a new role: 
    - Role name: **lambda-s3-iam-role**
  - Lambda funtion code: checking

![122][5] 

- Workflow: 
    - The function has a **handler** (lambda_hander) that serves as the entry point. It extracts event **data and contacts** (context) paths to it. The function then extracts the **bucket name** (bucket) and the object **key** from the event data. It attempts to retrieve the **object** (get_object) from the specified bucket. If the retrieval is successful, the function **prints the object's content type**. 
- S3 trigger
  - Bucket: **s3/processing-bucket-1**
  - Event types: 
    - PUT
    - POST
      - CREATE FUNCTION

![122][6] 

- Check Code > Add 1 line > Deploy to Save the changes
- Check Trigger and start the code

|  |  |  |
|---|---| ---|
|![122][7] |![122][8] | 

- Upload files to Bucket and check Cloudwatch


|  |  |  |
|---|---| ---|
|![122][10] |![122][9] | 

[1]: /aws-ws/images/12/2/1.png?featherlight=false&width=50pc
[2]: /aws-ws/images/12/2/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/12/2/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/12/2/4.png?featherlight=false&width=50pc
[5]: /aws-ws/images/12/2/5.png?featherlight=false&width=50pc
[6]: /aws-ws/images/12/2/6.png?featherlight=false&width=50pc
[7]: /aws-ws/images/12/2/7.png?featherlight=false&width=50pc
[8]: /aws-ws/images/12/2/8.png?featherlight=false&width=50pc
[9]: /aws-ws/images/12/2/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/12/2/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/12/2/11.png?featherlight=false&width=50pc
[12]: /aws-ws/images/12/2/12.png?featherlight=false&width=50pc
[13]: /aws-ws/images/12/2/13.png?featherlight=false&width=50pc
[14]: /aws-ws/images/12/2/14.png?featherlight=false&width=50pc
[15]: /aws-ws/images/12/2/15.png?featherlight=false&width=50pc