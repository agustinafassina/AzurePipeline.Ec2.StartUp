# Automation pipeline: Turn on the Ec2 instances ⚙️
This repository features an Azure Pipeline designed to automate the process of starting and managing an EC2 instance in AWS. It includes commands to launch an Ec2 instance, monitor its status, and manage its lifecycle efficiently. This setup helps streamline infrastructure management, reduce manual effort, and ensure that instances are started and checked reliably within your deployment workflows.

### 🛠️For the use the schema
* Aws account and a EC2
* Azure devOps account
* * Install the [AWS Toolkit for Azure DevOps](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-vsts-tools) extension
* * Create an AWS service connection
* * Create new pipeline
* * Set the variables in azure-pipelines.yml

#### AWS service connection
The pipeline uses the `AWSShellScript@1` task, which authenticates through an Azure DevOps service connection instead of access keys stored in variables.

- Go to **Project Settings → Service connections → New service connection → AWS**
- Enter the Access Key ID and Secret Access Key (or a Role ARN to assume)
- Name it `aws-dev-connection` (or update `awsCredentials` in azure-pipelines.yml to match)

#### Pipeline variables
Defined directly in azure-pipelines.yml:

```yaml
variables:
  AWS_REGION: 'us-east-1'
  AWS_INSTANCE_ID: 'i-xxxxxxxxxxxxxxxxx,i-yyyyyyyyyyyyyyyyy'
```

Replace them with your actual region and instance IDs. `AWS_INSTANCE_ID` accepts one ID or several IDs separated by commas.

After starting the instances, the pipeline runs `aws ec2 wait instance-status-ok`, so the run only succeeds once every instance has passed its status checks.

### ⚡Triggers in Azure Devops
We can configure pipelines to run on the days we need:
- Create pipeline in Azure Devops
- Setting pipeline with this repository and azure-pipelines.yml
- Select Triggers and Scheduled
- Scheduled detail:

<div align="center">
  <img height="300" width="95%" src="triggers.png"  />
</div>

### 🔄 Workflow
Pipeline workflow overview:

<div align="center">
  <img height="300" width="90%" src="workflow.png"  />
</div>
