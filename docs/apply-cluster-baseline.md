# Cluster Baseline Setup

When setting up Azure Arc for Kubernetes, you will typically have multiple configurations you wish to apply. For example, you will likely have one set of configuration for cluster operational requirements, like logging and monitoring. In this example we refer to this as the 'Cluster Baseline'.

To apply the cluster baseline you first need to make sure your cluster is joined to Azure Arc for Kubernetes. To join a cluster you will need to create a resource group and provide a cluster name. These are purely for reference within the Azure Resource Manager and the Azure Portal (i.e. This is metadata used by ARM to interact with your clusters).

If you haven't already, you can join your cluster as follows. Note that you will need to have an active kube/config which is set as your current context for the target cluster.

```bash
# Resource Group for Arc will use for the cluster
RG=<Resource Group Name>
# Location for the Resource Group
LOC=<Location>
# Name you want to use to refer to the target cluster once joined to Arc
CLUSTER_NAME=<Cluster Name>

# Create the Resource Group
az group create -n $RG -l $LOC

# Make sure you have an active .kube/config for the target cluster
# and that it's set as your current context

# Join the cluster to Azure Arc
az connectedk8s connect --name $CLUSTER_NAME  --resource-group $RG

```

Now that we have the cluster connected, we should see it in the Azure portal along with our other Kubernetes clusters, however Arc joined clusters will have a green flag on them. The next step is to apply a configuration. A configuration references a git repo which contains the manifests which need to be applied. This is done via the flux operator managed for you by Arc.

Note: You may need to adjust the operator params to match your git branch and path. The below assumes master branch with the manifests under the 'manifest' path.

```bash
az k8sconfiguration create -g $RG \
-c $CLUSTER_NAME \
-n baseline-config \
--enable-helm-operator \
--helm-operator-chart-version='0.6.0' \
--helm-operator-chart-values='--set helm.versions=v3' \
--repository-url https://github.com/CloudNativeGBB/cluster-baseline.git \
--operator-params="--git-readonly --git-path=manifests --sync-garbage-collection" \
--cluster-scope \
--operator-instance-name baseline-config \
--operator-namespace baseline-config
```

Watch for Completed status.

```bash
watch az k8sconfiguration show -g $RG --cluster-name $CLUSTER_NAME --name cluster-baseline-config --cluster-type connectedClusters -o json
```
