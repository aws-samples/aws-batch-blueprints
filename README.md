# AWS Batch Blueprints.

Welcome to AWS Batch Blueprints!

This project contains a collection of [AWS Batch](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html) patterns implemented in [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html).
It aims to provide common AWS Batch architecture examples across vairous workloads such Genomics processing, Seismic Imaging, Machine Learning, Data Analytics and many more.

## Examples list

| **Name**                                        | **Description**                                                                                                                                                                                                                                                                                                     | **Template**                                                                                 |
| ----------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| EC2 Compute Environment                         | Single job queue and compute environment based on EC2 with a BEST_FIT_PROGRESSIVE strategy.                                                                                                                                                                                                                         | [batch-single-jq-ec2.yaml](templates/batch-single-jq-ec2.yaml)                               |
| Fargate Compute Environment  | Single job queue and compute environment based on Fargate resources.  | [batch-fargate-linux.yaml](templates/batch-fargate-linux.yaml)|

| Launch Template based Compute Environment       | EC2 compute environment based on a launch template configuration with EBS gp3 boot drive. We **recommend** customers to use **gp3** type EBS that provides 3000 IOPS and 125 MB/s by default regardles of disk size to prevent [DockerTimeoutError](https://repost.aws/knowledge-center/batch-docker-timeout-error) | [batch-jq-ce-launch-template.yaml](templates/batch-jq-ce-launch-template.yaml)               |
| Local scratch                                   | Automatically mount unformatted drive as a RAID0 volume on EC2 instance of the Compute Environment                                                                                                                                                                                                                  | [batch-jq-ce-lt-local-ssd.yaml](templates/batch-jq-ce-lt-local-ssd.yaml)                     |
| No Hyperthreads                                 | EC2 compute environment configured to place jobs only on physical cores. AWS Batch jobs will need to use vCPUs on multiple of 2 (2vCPUs = 1 core)                                                                                                                                                                   | [batch-jq-ce-no-hyperthread.yaml](templates/batch-jq-ce-no-hyperthread.yaml)                 |
| Cache container image                           | Container image will be pulled if not present on Amazon EC2 instance and will be cached on the EC2 for the lifetime of the instance                                                                                                                                                                                 | [batch-jq-container-image-cached.yaml](templates/batch-jq-container-image-cached.yaml)       |
| EBS size proportional to vCPUs                  | Resize EBS depending the number of vCPUs                                                                                                                                                                                                                                                                            | [batch-jq-ce-lt-ebs-resize.yaml](templates/batch-jq-ce-lt-ebs-resize.yaml)                   |
| Detailed Monitoring of EC2 Compute environments | Configure Amazon CloudWatch Agent to collect Amazon EC2 memory and disk utilization                                                                                                                                                                                                                                 | [batch-jq-ce-lt-detailed-monitoring.yaml](templates/batch-jq-ce-lt-detailed-monitoring.yaml) |
| Amazon FSx for Lustre on Compute Environment    | Mount Amazon FSx for Lustre on a compute environment of AWS Batch                                                                                                                                                                                                                                                   | [batch-jq-ce-lt-fsx-for-lustre.yaml](templates/batch-jq-ce-lt-fsx-for-lustre.yaml)           |
| NVIDIA GPU Metrics                              | Collect NVIDIA GPU metrics for jobs running with AWS Batch                                                                                                                                                                                                                                                          | [README](ec2/gpu-monitoring-metrics/README.md)                                               |

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
