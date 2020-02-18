# Connect Kubernetes Clusters

## Pre-requisites

This assumes that you have already created Kubernetes clusters that need to now be connected to Azure Arc.

For each cluster, follow these steps:

## Connect Cluster

```console
az connectedk8s connect --name cloudnativegbb-eks-cluster --resource-group <resource-group>
```

## Gitops Config

```console
az k8sconfiguration create --name cluster-config --cluster-name <cluster-name> --resource-group <resource-group> --operator-instance-name cluster-config --operator-namespace cluster-config --repository-url git@github.com:CloudNativeGBB/cluster-baseline.git --operator-params="--git-readonly --git-path=manifests"
```

```console
az k8sconfiguration show --resource-group <resource-group> \
    --name cluster-config \
    --cluster-name <cluster-name>
```

Add the private key to your Github account 
