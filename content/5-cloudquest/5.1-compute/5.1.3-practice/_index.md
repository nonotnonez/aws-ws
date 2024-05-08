---
title : "Practice"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.1.3 </b> "
---

#### Concept

In this practice lab, you will:
- Review a bucket policy to secure a bucket in Amazone S3
- Enable static website hosting.

#### Practice Lab

{{% notice tip %}}
The **AWS Management Console** is a web interface to access and manage the broad collection of services provided by Amazon Web Services (AWS).
{{% /notice %}}

Step 5:

1. In the top navigation bar search box, type: **s3**
2. In the search results, under Services, click S3.
3. Go to the next step. ![513][5]

{{% notice tip %}}
Amazon Simple Storage Service (**Amazon S3**) is an object storage service that offers industry-leading scalability, data availability, security, and performance. Customers of all sizes and industries can use Amazon S3 to store and protect any amount of data for range of use cases, such as data lakes, websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics.
{{% /notice %}}

Step 6: 

1. On the General purpose buckets tab, click the bucket name that starts with website-bucket-.
   - The bucket name that starts with website-bucket- contains code required for this lab.
   
2. Go to the next step. ![513][6] 

{{% notice tip %}}
A **bucket** is a container for objects stored in Amazon S3. Every objects is contained in a bucket. Amazon S3 offers a range of storage classes for the objects that you store. You chose a class depending on your use case scenario and performance access requirements. Amazon S3 provides storage classes for frequently accessed infrequently accessed, and archive access objects.
{{% /notice %}}

Step 7

1. At the top of the page, select (highlight) and copy the bucket name, and then paste it in the text editor of your choice on your device.

   - You must use this bucket name in the later DIY section of this solution.

2. On the Objects tab, review the objects in the bucket.

   - Five files should be displayed.
   - These files contain the contents of the static webpage.
   - Local files can be loaded into this S3 bucket by using the Upload button.

3. Choose the check box to select text.html.
4. Click Actions to expand the dropdown menu.
5. Choose Rename object.
6. Go to the next step. ![513][7]

{{% notice tip %}}
You can choose the georaphical **AWS Region** where Amazon S3 stores the buckets that you create. You might choose a Region to optimize latency, minimize costs, or address regulatory requirements.
Objects stored in an AWS Region never leave the Region unless you explicity transfer or replicate them to another Region. 
For excample, objects stored in the Euro (Ireland) Region never leave it. Howerver, Amazon S3 redundantly stores objects on multiple devices across a minimun of three Availability Zones in an AWS Region. An Availability Zone is one or more discreate data centers with redundant power, networking, and connectivity in an AWS Region
{{% /notice %}}

**Step 8**

1. For New object name, type: **error.html**

   - This file contains the code for the error page, which opens whenever something goes wrong.

2. Click Save changes.
3. Go to the next step.

![513][8]

{{% notice tip %}}
Using Amazone S3, you can upload objects up to 5 GB in size with a single PUT operation. For larger objects, upteo 5 TB in size, use the multipart upload API.
{{% /notice %}}

Step 9

1. In the success alert, review the message.
2. Click the Permissions tab.
3. Go to the next step.

![513][9]

{{% notice tip %}}
By default, all Amazone S3 resources (buckets, objects, and related subresources) are private. Only the resource owner can access them. The resource owner can optionally grant access permissons to others by writing an access policy.
{{% /notice %}}

Step 10
1. In the Block public access (bucket settings) section, review to ensure that Block all public access is set to Off.
   - Turning off "Block all public access" is necessary for static web hosting through your S3 bucket.
2. Scroll down to Bucket policy.
3. Go to the next step.

![513][10]

{{% notice tip %}}
You can grant permissions to your Amazon S3 resources through bucket policies and user policies. Both options use JSON-based access policy language. An **Amazon Resource Name (ARN)** uniquely identifies AWS resources.
{{% /notice %}}   

Step 11

1. In the Bucket policy editor window, review the policy.

   - This policy allows public access to the S3 bucket.
   - Effect says this policy will Allow access.
   - Principal defines who has access. In this case, * represents anyone.
   - Action defines what users can do to objects in the bucket. In this case, users can only retrieve data with GetObject.
   - Resource specifies that this policy applies to only this bucket.
   - Generally, to safeguard against unintentional data exposure, we recommend strict S3 bucket permissions in production. 

2. Scroll up to the top of the page.
3. Go to the next step.

![513][11] 

{{% notice note %}}
To host a static website on Amazon S3, configure your bucket for static website hosting, set permissions, and add an index document. Available options include redirects, loging, and error documents.
{{% /notice %}}       

Step 12

1. Click the Properties tab.
2. Go to the next step.

![513][12]


Step 13

1. Scroll down to Static website hosting.
2. Click Edit.
3. Go to the next step.

![513][13]

{{% notice note %}}
Amazon S3 supports virtual-hosted-style URLs and path-style URLs. 
A virual-hosted-style URL locks like: https://bucket-name.s3.Region.amazonaws.com/key.
A path-style URL looks like: https://s3.Region.amazonaws.com/bucket-nam/keyname.
{{% /notice %}}       

Step 14

1. For Static website hosting, choose Enable.
2. For Hosting type, choose Host a static website.
3. For Index document, type: **index.html**
4. For Error document, type:  **error.html**
5. Go to the next step.

![513][14]

Step 15

1. Scroll down to the bottom of the page.
2. Click Save changes.
3. Go to the next step.

![513][15]

Step 16

1. Scroll down to Static website hosting.
2. Review to ensure that Hosting type is set to Bucket hosting.
3. Under Bucket website endpoint, click the copy icon to copy the provided endpoint.
4. Go to the next step.

![513][16]

Step 17

1. To load the Beach Wave Conditions webpage, in a new browser tab (or window) address bar, paste the bucket website endpoint that you just copied, and then press Enter.
2. Go to the next step.

![513][17]

Finish

![513][18]

[5]:  /aws-ws/images/5-cloudquest/51/513/5.png?featherlight=false&width=90pc
[6]:  /aws-ws/images/5-cloudquest/51/513/6.png?featherlight=false&width=90pc
[7]:  /aws-ws/images/5-cloudquest/51/513/7.png?featherlight=false&width=90pc
[8]:  /aws-ws/images/5-cloudquest/51/513/8.png?featherlight=false&width=90pc
[9]:  /aws-ws/images/5-cloudquest/51/513/9.png?featherlight=false&width=90pc
[10]:  /aws-ws/images/5-cloudquest/51/513/10.png?featherlight=false&width=90pc
[11]:  /aws-ws/images/5-cloudquest/51/513/11.png?featherlight=false&width=90pc
[12]:  /aws-ws/images/5-cloudquest/51/513/12.png?featherlight=false&width=90pc
[13]:  /aws-ws/images/5-cloudquest/51/513/13.png?featherlight=false&width=90pc
[14]:  /aws-ws/images/5-cloudquest/51/513/14.png?featherlight=false&width=90pc
[15]:  /aws-ws/images/5-cloudquest/51/513/15.png?featherlight=false&width=90pc
[16]:  /aws-ws/images/5-cloudquest/51/513/16.png?featherlight=false&width=90pc
[17]:  /aws-ws/images/5-cloudquest/51/513/17.png?featherlight=false&width=90pc
[18]:  /aws-ws/images/5-cloudquest/51/513/18.png?featherlight=false&width=90pc