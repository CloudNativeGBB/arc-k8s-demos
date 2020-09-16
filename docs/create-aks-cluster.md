# Create an Azure AKS cluster

## Pre-requisites

This assumes that you already have a Microsoft Azure Subscription and that you are logged into the [Azure CLI](https://aka.ms/azure-cli) and have set the correct Azure Subscription to provision resources into.

* [Download and Install Hashicorp Terraform CLI](https://www.terraform.io/downloads.html)
* [Configure or Login with your Azure Credentials](https://www.terraform.io/docs/providers/azurerm/index.html) (If you're already logged into Azure-CLI you're gtg).

## Create Microsoft Azure AKS Cluster

Clone the `cluster-baseline` repository:
```console
git clone git@github.com:CloudNativeGBB/cluster-baseline.git
cd cluster-baseline/infrastructure/azure/advanced_public
```

Initialize Terraform:
```console
terraform init
```

To see any changes in advance that are required for your infrastructure, use `terraform plan -out tfplan`.

Create the cluster using Terraform:
```console
terraform apply tfplan
```

Answer `yes` to approve the creation actions when prompted.

Rinse, repeat as needed, style as usual.
