# Create an Azure AKS cluster

## Pre-requisites

This assumes that you already have a Microsoft Azure Subscription and that you are logged into the [Azure CLI](https://aka.ms/azure-cli) and have set the correct Azure Subscription to provision resources into.

* [Download and Install Hashicorp Terraform CLI](https://www.terraform.io/downloads.html)
* [Configure or Login with your Azure Credentials](https://www.terraform.io/docs/providers/azurerm/index.html) (If you're already logged into Azure-CLI you're gtg).

## Create Microsoft Azure AKS Cluster

Clone the [cluster-baseline](https://github.com/CloudNativeGBB/cluster-baseline) repository:
```console
git clone git@github.com:CloudNativeGBB/cluster-baseline.git
```

### Public Cluster
```bash
# For a Public Cluster
cd cluster-baseline/infrastructure/azure/advanced_public
```

Run Terraform:
```console
terraform init
terraform plan -out tfplan
terraform apply tfplan
```

### Private Cluster
```bash
# For a Private Cluster
cd cluster-baseline/infrastructure/azure/advanced_private
```

Run provisioning script:
```console
bash provision.sh
```

To see any changes in advance that are required for your infrastructure, use `terraform plan -out tfplan`.

Once complete you can get your kube-config with admin privileges:
```bash
export CLUSTER_NAME=$(terraform output cluster_name)
export CLUSTER_RESOURCE_GROUP=$(terraform output cluster_resource_group_name)

az aks get-credentials -n $CLUSTER_NAME -g $CLUSTER_RESOURCE_GROUP --admin
```

Answer `yes` to approve the creation actions when prompted.

Rinse, repeat as needed, style as usual.
