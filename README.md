# Azure Arc for Kubernetes Demos

This organization and repo are focused on demos around Azure Arc for Kubernetes. There are 3 main scenarios we will be focused around.

1. Connecting existing clusters to Azure Arc for Kubernetes to gain visibility into the api endpoint.

2. Provide GitOps guidance around repo structure along with responsibilities.

3. Use Azure Policy to publish applications to connected clusters.

## Setup

This section walks through at a high-level what is needed to get the demos setup.

- Enable Azure Arc for Kubernetes on your Azure Subscription (Contact Arc Engineering [todo: insert contact])
- [Install the CLI extension and register providers](https://github.com/Azure/azure-arc-kubernetes-preview/blob/master/README.md)
- Create AKS cluster in Azure
- Create GKE cluster in GCP
- [Create EKS Cluster in AWS.](./docs/create-eks-cluster.md)
- Use Azure CLI to connect AKS, GKE and EKS clusters in Azure Arc for Kubernetes
- Setup cluster-baseline repo for Infrastructure Pipeline(s) based on Terraform and Cluster Configurations based on K8s manifests
- Setup source control repo for Application
- Setup app-baseline repo for application yaml files
- [Use Azure CLI to configure Azure Arc to point to cluster-baseline repo for common cluster configuration.](./docs/apply-cluster-baseline.md)
- [Use Azure CLI to configure Azure Arc to point to app-baseline repo for application deployment](./docs/apply-app-baseline.md)
