# Azure Arc for Kubernetes Demos

This organization and repo are focused on demos around Azure Arc for Kubernetes. There are 3 main scenarios we will be focused around.

1. Connecting existing clusters to Azure Arc for Kubernetes to gain visibility into the api endpoint.

2. Provide GitOps guidance around repo structure along with responsibilities.

3. Use Azure Policy to publish applications to connected clusters.

## Setup

This section walks through at a high-level what is needed to get the demos setup.

- [Add extensions and register providers](https://docs.microsoft.com/en-us/azure/azure-arc/kubernetes/connect-cluster)
- [Create AKS cluster in Azure](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough)
- [Create GKE cluster in GCP](./docs/create-gke-cluster.md)
- [Create EKS Cluster in AWS](./docs/create-eks-cluster.md)
- [Configure Azure Arc common cluster configuration](./docs/apply-cluster-baseline.md)
- [Configure Azure Arc for application deployment](./docs/apply-app-baseline.md)
