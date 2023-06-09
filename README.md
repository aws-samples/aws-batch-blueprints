# AWS Batch CloudFormation Examples.

Welcome to AWS Batch CloudFormation examples!

This project contains a collection of [AWS Batch](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html) patterns implemented in [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html).
It aims to provide common AWS Batch architecture examples across vairous workloads such Genomics processing, Seismic Imaging, Machine Learning, Data Analytics and many more.

## Examples list

| **Name** | **Description** | **Template** |
| -------- | :---- | ------------ |
| EC2 Compute Environment | Single job queue and compute environment based on EC2 with a BEST_FIT_PROGRESSIVE strategy.| [batch-single-jq-ec2.yaml](templates/batch-single-jq-ec2.yaml)|
| Launch Template based Compute Environment| EC2 compute environment based on a launch template configuration with EBS gp3 boot drive. We **recommend** customers to use **gp3** type EBS that provides 3000 IOPS and 125 MB/s by default regardles of disk size to prevent [DockerTimeoutError](https://repost.aws/knowledge-center/batch-docker-timeout-error)| [batch-jq-ce-launch-template.yaml](templates/batch-jq-ce-launch-template.yaml)|
| Local scratch | Automatically mount unformatted drive as a RAID0 volume on EC2 instance of the Compute Environment|[batch-jq-ce-lt-local-ssd.yaml](templates/batch-jq-ce-lt-local-ssd.yaml)|
| No Hyperthreads | EC2 compute environment configured to place jobs only on physical cores. AWS Batch jobs will need to use vCPUs on multiple of 2 (2vCPUs = 1 core)| [batch-jq-ce-no-hyperthread.yaml](templates/batch-jq-ce-no-hyperthread.yaml)|

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

