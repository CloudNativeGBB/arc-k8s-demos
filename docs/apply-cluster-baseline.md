# Cluster Baseline Setup

When setting up Azure Arc for Kubernetes, you will typically have multiple configurations you wish to apply. For example, you will likely have one set of configuration for cluster operational requirements, like logging and monitoring. In this example we refer to this as the 'Cluster Baseline'.

## Cluster Connect

To apply the cluster baseline you first need to make sure you're cluster is joined to Azure Arc for Kubernetes. To join a cluster you will need to create a resource group and provide a cluster name. These are purely for reference within the Azure Resource Manager and the Azure Portal (i.e. This is metadata used by ARM to interact with your clusters).

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

## Apply Configuration

Now that we have the cluster connected, we should see it in the Azure portal along with our other Kubernetes clusters. Arc joined clusters will have a green flag on them. The next step is to apply a configuration. A configuration defines the operators needed (Flux & Helm) and references a git repo which contains the manifests to be applied. The manifests are applied via the flux operator, and optionally the Helm operator, managed for you by Arc.

For this example we'll be deploying a baseline configuration consiting of the following:
- [Falco](https://sysdig.com/opensource/falco/)
- [Prometheus Operator](https://github.com/coreos/prometheus-operator)
- [Azure Monitor for Containers](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/container-insights-overview)
- [Optional - Sealed Secrets Controller](https://github.com/bitnami-labs/sealed-secrets)

>Note: The following steps are a manual walk through of this setup to show all of the components in play. In a real world scenario these steps would be automated via a combination of Policy and CI/CD.

### Log Analytics Pre-Config

To deploy Azure Monitor for Containers we'll need to have a Log Analytics workspace to target which has the Container Insights solution installed. After creating this we'll need to grab the workspace ID and Key, which will be used in the HelmRelease. For details on how to setup Log Analytics you can follow the [Configure hybrid Kubernetes clusters with Azure Monitor for containers](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/container-insights-hybrid-setup) (Note: In the hybrid cluster setup for Log Analytics, you can stop before the 'Install the chart' step, as we'll install the chart via our Arc configuration.)

Once you have your workspace, you need to grab the access key.

```bash
# Get Access Key
az monitor log-analytics workspace get-shared-keys --resource-group $RG --workspace-name <INSERT WORKSPACE NAME>
```

Now that we have our workspace we need to take the values from the workspace creation and create the Azure Monitor Helm chart values file. We'll create a secret using this values file to be used by the Helm operator when deploying the Azure Monitor Helm chart.

Create a new file called 'azure-monitor-values.yaml' containing the following:

```yaml
omsagent:
  secret:
    wsid: '<INSERT WORKSPACE ID>'
    key: '<INSERT WORKSPACE KEY>'
  env:
    clusterName: '<INSERT RESOURCE ID OF ARC CLUSTER>'
```

To make these values available to the Helm operator we need to get this file into a secret. We'll do that manually here, but you could also use a [sealed secret](https://github.com/bitnami-labs/sealed-secrets/blob/master/README.md).

```bash
# Create a secret containing the azure-monitor-values.yaml file
kubectl create secret generic azure-monitor-conn --from-file=azure-monitor-values.yaml -n kube-system
```

>Note: For educational purposes the azure-monitor.yaml file sets the Helm parameter for the cluster name to be sent to Azure Monitor. This is to show you how you set values directly from the HelmRelease definition, as well as how to reference from a Kubernetes secret. If you wish you can combine these all into the azure-monitor-values.yaml file.

### Deploy the Cluster Baseline Configuration

We're now ready to deploy our configuration. If you forked the repository and played around with folder structure and/or branches you may need to modify the command below to match your repo/branch/path. Also, note that we are setting the 'enable-helm-operator' flag as well as passing in the operator version and a parameter to the Helm operator to require Helm version 3.

```bash
az k8sconfiguration create -g $RG \
-c $CLUSTER_NAME \
-n baseline-config \
--enable-helm-operator \
--helm-operator-version='0.6.0' \
--helm-operator-params='--set helm.versions=v3' \
--repository-url https://github.com/CloudNativeGBB/cluster-baseline.git \
--operator-params="--git-branch monitoring --git-readonly --git-path=manifests --sync-garbage-collection" \
--scope cluster \
--operator-instance-name baseline-config \
--operator-namespace baseline-config \
--cluster-type connectedClusters
```

Watch for Compeleted status.

```bash
watch az k8sconfiguration show -g $RG --cluster-name $CLUSTER_NAME --name baseline-config --cluster-type connectedClusters -o json
```

