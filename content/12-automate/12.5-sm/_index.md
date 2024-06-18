---
title : "AWS System Manager"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 12.5 </b> "
---

#### AWS Systems Manager

- AWS Systems Manager allows you to centeally view and manage resource in AWS and multicloud hybrid environments. 

|  |  |  |
|---|---| ---|
|![12][1] |![12][2] | ![12][3] | 
|![12][4] |![12][5] | ![12][6] | 
|![12][7] |![12][8] | ![12][9] | 
|![12][10] | 

- **Demo** : Let's execute a runbook that will enable versioning on S3 buckets
- 
- System Manager - Automation - **Excute automation**
  - Choose runbook : AWS-ConfigureS3BucketVersioning
    - Execute automation runbook : **Rate control**
      - Targets: 
        - Parameter: BucketName
        - Targets: Tags
          - Specify instance tag key: Environment
          - Specify tag value (optional): Production
            - ADD
        - AutomationAssumeRole (no need with admin)
        - EXCUTE 

![12][11]
    

[1]: /aws-ws/images/12/5/1.png?featherlight=false&width=50pc
[2]: /aws-ws/images/12/5/2.png?featherlight=false&width=50pc
[3]: /aws-ws/images/12/5/3.png?featherlight=false&width=50pc
[4]: /aws-ws/images/12/5/4.png?featherlight=false&width=50pc
[5]: /aws-ws/images/12/5/5.png?featherlight=false&width=50pc
[6]: /aws-ws/images/12/5/6.png?featherlight=false&width=50pc
[7]: /aws-ws/images/12/5/7.png?featherlight=false&width=50pc
[8]: /aws-ws/images/12/5/8.png?featherlight=false&width=50pc
[9]: /aws-ws/images/12/5/9.png?featherlight=false&width=50pc
[10]: /aws-ws/images/12/5/10.png?featherlight=false&width=50pc
[11]: /aws-ws/images/12/5/11.png?featherlight=false&width=90pc