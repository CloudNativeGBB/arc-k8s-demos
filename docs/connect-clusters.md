# Connect Kubernetes Clusters

## Pre-requisites

This assumes that you have already created at least one of the Kubernetes clusters from the previous steps.

## Connect Cluster

For each cluster, follow these steps:

```console
az connectedk8s connect --name <your-cluster-name> --resource-group <your-resource-group>
```

## Gitops Config

```console
az k8sconfiguration create --name cluster-config --cluster-name <your-cluster-name> --resource-group <your-resource-group> --operator-instance-name cluster-config --operator-namespace cluster-config --repository-url git@github.com:CloudNativeGBB/cluster-baseline.git --operator-params="--git-readonly --git-path=manifests"
```

```console
az k8sconfiguration show --resource-group <your-resource-group> \
    --name cluster-config \
    --cluster-name <your-cluster-name>
```

* [Add the resulting SSH key to your Github account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) 
