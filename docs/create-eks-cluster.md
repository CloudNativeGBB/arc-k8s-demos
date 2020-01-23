# Create an Amazon EKS cluster

## Pre-requisites

This assumes that you already have an Amazon Web Services account and that you created an Amazon EKS service role in the AWS IAM console.

* [Download and Install the Terraform by Hashicorp CLI](https://www.terraform.io/downloads.html)
* [Configure AWS Credentials](https://www.terraform.io/docs/providers/aws/index.html)

## Create AWS EKS Cluster

Clone the `cluster-baseline` repository:
```console
git clone git@github.com:CloudNativeGBB/cluster-baseline.git
cd cluster-baseline
```

Initialize Terraform:
```console
terraform init
```

To see any changes in advance that are required for your infrastructure, use `terraform plan`.

Create the cluster using Terraform:
```console
cd infrastructure/aws
terraform apply
```

Answer `yes` to approve the creation actions when prompted.

Rinse, repeat as needed, style as usual.
