---
title : "Step Functions"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 12.4 </b> "
---

#### Orchestrating with Step Functions

- Orchestration involves coordinating and maintaining interconnected tasks to accomplish a specific workflow.

- Ingestion -> Validation -> Transformation -> Storage ![12][1]

- AWS Step Functions is a serverless orchestration service.
- Step Functions allows you to coordinate the components of distributed applications and microservices using visual workflows

| Use Cases |  |  |
|---|---| ---|
|![12][2] |![12][3] | ![12][4] | 
|![12][5] |![12][6] | ![12][7] | 

||  |  |
|---|---| ---|
|![12][8] |![12][9] | 

- **Demo**
- AWS Step Functions >  Get started ![12][10]
- Create your own - Drag and drop 
  - Actions:
    - Lambda Invoke
      - State name: **Validate Payment**
  - Flow:
    - Choice
  - Actions:
    - SNS Public
      - State name: **Send failed with retry link**
  - Search:
    -  Amazon DynamoDB - Updateitem
      - State name: **Update Inventory**
  - Actions:
    - SNS Public
      - State name: **Notify shipping company**
  - Actions:
    - Lambda Invoke
      - State name: **Generate invoice**
  - Actions:
    - SNS Public
      - State name: **Send success email with invoice**
  - Flow:
    - Success
  - Flow:
    - Fail ![12][11]

[1]: /aws-ws/images/12/4/1.png?featherlight=false&width=50pc
[2]: /aws-ws/images/12/4/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/12/4/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/12/4/4.png?featherlight=false&width=50pc
[5]: /aws-ws/images/12/4/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/12/4/6.png?featherlight=false&width=40pc
[7]: /aws-ws/images/12/4/7.png?featherlight=false&width=50pc
[8]: /aws-ws/images/12/4/8.png?featherlight=false&width=40pc
[9]: /aws-ws/images/12/4/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/12/4/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/12/4/11.png?featherlight=false&width=50pc
