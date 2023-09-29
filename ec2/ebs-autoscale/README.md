# AWS Batch and Amazon EBS Autoscale

This infrastructure as code example shows how to dynamically grow a storage volume for scratch data.

This example leverages [amazon-ebs-autoscale](https://github.com/awslabs/amazon-ebs-autoscale) to mount a logical volume on `/scratch` (default: 200GiB).
The ebs-autoscale systemd service observes the used and available space.
At a threshold level of available space (default: 50%), ebs-autoscale creates an additional EBS volume and extend the size of the logival volume.
It creates more space available for `/scratch`.

For more information on the **amazon-ebs-autoscale** options, please [read](https://github.com/awslabs/amazon-ebs-autoscale/blob/master/README.md).
