# AWS Batch with AWS Fargate CloudFormation templates resources

This directory contains the AWS CloudFormation templates for bootstrapping an AWS Batch environment with a single job queue and compute environment configured to use Fargate resources. 

Detailed instructions are in [the examples directory for Fargate](../examples/fargate/README.md).

We provide two templates: 

| Name | Description | Location | 
| ---- | ----------- | -------- | 
| Fargate with existing VPC | A template that leverages an existing VPC subnet and security group. | [cloudformation/batch-fargate-existing-vpc.yaml](cloudformation/batch-fargate-existing-vpc.yaml)
| Fargate with new VPC | A template that creates a new VPC and other resources needed for running jobs.  | [cloudformation/batch-fargate-new-vpc.yaml](cloudformation/batch-fargate-new-vpc.yaml)

> [!IMPORTANT]
> Fargate support for Linux and Windows containers is AWS Region and Availability Zone dependent. To check if your Region and Availability Zone is support, consult the [Supported Regions for Amazon ECS on AWS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate-Regions.html) documentation page.
  

