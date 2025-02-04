# Challenge 03 - Introduction To Kubernetes

```powershell
# create the resource group
az group create -n rg-wth -l eastus2

# create the cluster
az aks create -n wthcluster -g rg-wth --zones 1 2 3
```

Verify the zones and network plugin.

```powershell
# verify the zones for the nodes
az aks show -n wthcluster -g rg-wth --query "agentPoolProfiles[].availabilityZones"

# verify CNI is the network plugin
az aks show -n wthcluster -g rg-wth --query "networkProfile.networkPlugin"

# verify that the plugin mode is "overlay"
az aks show -n wthcluster -g rg-wth --query "networkProfile.networkPluginMode"
```

## Tips
- **"The cluster should use a managed identity"** - This is already happening, as a cluster uses a system-assigned managed identity by default for the control plane. There are no additional options to enable this.
- **"The cluster should use Azure CNI (with overlay enabled)."** - This is also already happening, as the default networking is CNI w/ overlay. No additional flags are needed.