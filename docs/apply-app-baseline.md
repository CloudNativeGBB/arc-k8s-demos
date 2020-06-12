# Application Baseline Setup

Once you've established your cluster baseline and applied that configuration to your cluster you'll likely look to begin deploying application manifests to your cluster. You'll do this by creating one or many git repositories for those application manifests which can then be applied to the cluster in the same way as you applied the cluster baseline.

**Note:** *The following assumes you've already followed the steps in the [cluster baseline](./apply-cluster-baseline.md) doc to connect your cluster to Azure Arc for Kubernetes.

```bash
# Resource Group used for the cluster Arc instance
RG=<Resource Group Name>

# Name you want to use to refer to the target cluster once joined to Arc
CLUSTER_NAME=<Cluster Name>

```

You apply an application configuration via the same ```az k8sconfiguration create``` command we used to apply the cluster baseline configuration. Note the use of ```--operator-params``` to pass flags to be used by flux to access the right branch and path.

```bash
az k8sconfiguration create --n service-tracker-config -g $RG -c $CLUSTER_NAME \
-u https://github.com/CloudNativeGBB/app-baseline.git \
--operator-instance-name service-tracker-config --operator-namespace service-tracker-config \
--operator-params="--git-branch master --git-readonly --git-path=manifests --sync-garbage-collection" \
--cluster-type connectedClusters
--scope cluster
```

Watch for completed status

```bash
watch az k8sconfiguration show -g $RG -c $CLUSTER_NAME -n service-tracker-config -o json --cluster-type connectedClusters
```
