# Create an Google Cloud Platform Cluster

## Pre-requisites

This assumes that you already have a Google Cloud Platform account and have created and selected a project with the necessary services enabled

* [Install Google Cloud SDK](https://cloud.google.com/sdk/docs/quickstarts)
* [Install `kubectl` command line tool](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Google Cloud Platform SDK config

``` cli
gcloud init
gcloud auth application-default login
```

* Set the [project ID](https://cloud.google.com/resource-manager/docs/creating-managing-projects#identifying_projects) and [compute engine zone](https://cloud.google.com/compute/docs/zones#available)

``` cli
gcloud config set project <projectid>
gcloud config set compute/zone <computezone>
```
 
## Create GCP GKE Cluster

### Option 1: Create using command line tools (locally)



* Create the Kubernetes cluster (this will take a few minutes)

``` cli
gcloud container clusters create <cluster-name>
```

* After the cluster has completed, verify that there are 3 nodes running:

``` cli
gcloud compute instances list
```

* Generate a kubeconfig entry that uses gcloud for authentication

``` cli 
gcloud container clusters get-credentials cluster-name
```

---

#### Option 2: Create GKE cluster using Terraform

* [Download and Install the Terraform CLI](https://www.terraform.io/downloads.html)

Clone the `cluster-baseline` repository:

```cli
git clone git@github.com:CloudNativeGBB/cluster-baseline.git
cd cluster-baseline
```

* Verify that Terraform is >= 0.13.2

``` cli
terraform version
```

* Initialize Terraform

```cli
terraform init
```

* To see any changes in advance that are required for your infrastructure, use `terraform plan`.

* Create the cluster using Terraform templates

```cli
cd infrastructure/gcp
terraform apply
```

* Generate a kubeconfig entry that uses gcloud for authentication

``` cli 
gcloud container clusters get-credentials <cluster-name>
```
