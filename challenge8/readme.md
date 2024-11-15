# Challenge 08 - Storage

```powershell
# deploy the PVCs for data and config
kubectl apply -f .\pvc-config.yaml -f .\pvc-data.yaml
```

**Users will need to re-initialize the mongo database using the [`init.yaml`](../challenge7/init.yaml) found in Challenge 7**, running the following to clear-out the previous instance of the earlier init job:

```powershell
# delete the job manually
kubectl delete -f .\init.yaml

# re-run the job
kubectl apply -f .\init.yaml
```

Finally, run the new deployment with `volumeMounts` and `volumes` configured.

```powershell
# deploy the updated MongoDB deployment
kubectl apply -f .\mongo-deployment.yaml
```