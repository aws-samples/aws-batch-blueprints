# How to use Amazon FSx for Lustre with AWS Batch

In this example, you will learn how to use Amazon FSx for Lustre with AWS Batch on Amazon EC2.

You will :

1. Create an Amazon FSx for Lustre.
1. Deploy an AWS Batch environment capable to mount an Amazon FSx for Lustre.
1. Create an AWS Batch job definition that defines how to mount Amazon FSx for Lustre.
1. Submit an AWS Batch job that shows that lustre is mounted in the container.
1. Read the execution logs of the AWS Batch job.

## Prerequisites

Before starting, you will need to create an Amazon FSx for Lustre file system, [Getting started](https://docs.aws.amazon.com/fsx/latest/LustreGuide/getting-started.html).

## Deploy AWS Batch environment with Amazon FSx for Lustre

Once your Amazon FSx for Lustre is created, you will deploy an AWS Batch environment based on EC2 compute environments to mount Amazon FSx for Lustre.
To ease the setup, you will find infrastructure as code (IaC) in [/ec2/amazon-fsx-for-lustre/](/ec2/amazon-fsx-for-lustre/).

## Create job definition

The Batch job definition defines how to mount a local directory on Amazon EC2 in a Batch job.
In this example, the Amazon FSx for Lustre file system is mounted on an Amazon EC2 instance at `/fsxl`.

To expose Amazon FSx for Lustre in the Batch job, you will create a job definition with the following specifications:

- one **volume** with parameters as:
  - **fsxforlustre** for the **name**. It sets the name of the volume.
  - **/fsxl** for **sourcePath**. The path on the host container EC2 instance that's presented to the container.
- one **mountPoints** with parameteres as:
  - **fsxforlustre** for the **sourceVolume**. The name of the volume to mount.
  - **/scratch** for the **containerPath**. The path on the container where the host volume is mounted.

Below, you will find the job defition file that you will register to mount Amazon FSx for Lustre in Batch job
It displays /scratch where FSx for Lustre is mounted to confim it works as expected.

<!-- embedme batch-fsx-lustre-job-definition.json -->

```json
{
  "jobDefinitionName": "FSxLustre",
  "type": "container",
  "containerProperties": {
      "image": "public.ecr.aws/amazonlinux/amazonlinux:2",
      "command": [ "df", "-Th", "/scratch" ],
      "mountPoints": [
        {
          "containerPath": "/scratch",
          "readOnly": false,
          "sourceVolume": "fsxforlustre"
        }
      ],
      "volumes": [
        {
          "host": {
             "sourcePath": "/fsxl"
          },
          "name": "fsxforlustre"
        }
      ],
      "resourceRequirements": [
        {
          "type": "MEMORY",
          "value": "500"
        },
        {
          "type": "VCPU",
          "value": "1"
        }
      ]
  }
}

```

Register the job definition.

```bash
aws batch register-job-definition --cli-input-json file://batch-fsx-lustre-job-definition.json
```

## Submit job

You will now submit job based on the job defition created previously.
Please refer to your IaC output for the **job queue** name to use.

```bash
JOB_ID=$(aws batch submit-job --job-queue [JQ-FSXL] --job-name fsx-lustre --job-definition FSxLustre --query jobId)
echo $JOB_ID
```

## Describe job

Describe the job and retrieve the Batch job log. It might be empty till the job is **RUNNING**, please keep trying till you get a value.

```bash
JOB_LOG_STREAM=$(aws batch describe-jobs --jobs $JOB_ID --query jobs[0].container.logStreamName --output text)
echo $JOB_LOG_STREAM
```

## Read output log

It is time to check the output of the job with the following command line:

```bash
aws logs get-log-events --log-group-name /aws/batch/job  --query events[].message --log-stream-name $JOB_LOG_STREAM
```

You will find an output example that shows the lustre file system mounted on `/scratch` in the container.

```txt
[
    "Filesystem                  Type     Size  Used Avail Use% Mounted on",
    "172.31.38.184@tcp:/cm3phbev lustre   1.2T  7.5M  1.2T   1% /scratch"
]
```

## Clean up

Delete the IaC deployed in the first step as well as the Amazon FSx for Lustre.
