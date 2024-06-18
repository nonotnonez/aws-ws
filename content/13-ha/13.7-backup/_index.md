---
title : "Backing Up"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 13.7 </b> "
---

#### AWS Backup

- AWS Backup 
  - Settings > Configure resources
  - Backup plans > Create backup plan
    - Build a new plan
    - Backup plane name:    Daily-EBS-Snapshot
    - Schedule
      - Backup rule name: **backup-at-11AM**
      - Total retention period: 1 Days
        - CREATE PLAN > ASSIGN RESOURCES
  - Backup plans > Daily-EBS-Snapshots > Assign resources
    - name: **daily-backups-for-all-ebs-volumes**
    - IAM role: Default role
    - Defind resource selection : Include specific resource: **EBS**
      - ASSIGNMENT RESOURCE

![131](/aws-ws/images/13/7/6.png?featherlight=false&width=50pc)

  - Protectd resources > Create on-demanded backup
    - Resource type: **EBS** Volume ID: **vol-07c...**
    - Backup windows > Create backup now > **1 Days**
      - CREATE ON-DEMAND BACKUP

![131](/aws-ws/images/13/7/7.png?featherlight=false&width=50pc)    

#### Security Databases against Failures

- AWS Backup allows you to centralize and automate backups across AWS services




|  |  |  |
|---|---| ---|
|![131][2]| ![131][3]| ![131][4]|
|![131][5]| ![131][6]| ![131][7]|


[1]: /aws-ws/images/13/7/1.png?featherlight=false&width=40pc
[2]: /aws-ws/images/13/7/2.png?featherlight=false&width=40pc
[3]: /aws-ws/images/13/7/3.png?featherlight=false&width=40pc
[4]: /aws-ws/images/13/7/4.png?featherlight=false&width=40pc
[5]: /aws-ws/images/13/7/5.png?featherlight=false&width=40pc
[6]: /aws-ws/images/13/7/6.png?featherlight=false&width=40pc
[7]: /aws-ws/images/13/7/7.png?featherlight=false&width=40pc
[8]: /aws-ws/images/13/7/8.png?featherlight=false&width=40pc
[9]: /aws-ws/images/13/7/9.png?featherlight=false&width=40pc

[10]: /aws-ws/images/13/7/10.png?featherlight=false&width=40pc
[11]: /aws-ws/images/13/7/11.png?featherlight=false&width=40pc
[12]: /aws-ws/images/13/7/12.png?featherlight=false&width=40pc
[13]: /aws-ws/images/13/7/13.png?featherlight=false&width=40pc
[14]: /aws-ws/images/13/7/14.png?featherlight=false&width=40pc
[15]: /aws-ws/images/13/7/15.png?featherlight=false&width=40pc