# Challenge 05 - Scaling and High Availability

Scale the node pool down to 1 node.

```powershell
# get the name of the nodepool
az aks nodepool list --cluster-name wthcluster -g rg-wth -o table

# scale down to 1 node
az aks nodepool scale --cluster-name wthcluster -g rg-wth -n nodepool1 -c 1
```

Once scaled down, users can verify that only 1 node remains.

```powershell
# show the nodes
kubectl get nodes
```

Scale-up the web and API apps.
```powershell
# OPTION 1: increase spec.replicas in the deployment and re-apply
kubectl apply -f .\api-deployment.yaml -f .\web-deployment.yaml

# OPTION 2: scale using CLI
kubectl scale deploy web --replicas=2
kubectl scale deploy api --replicas=4
```

There should now be pods that are Pending, as they cannot be scheduled due to lack of resources on the node. **Users would either need to scale the cluster back up to 3 nodes or lower the values of `spec.template.spec.containers.resources.requests` (and `limits`) to lower values to "fit" onto the node.**