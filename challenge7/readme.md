# Challenge 07 - Updates and Rollbacks

This challenge requires creation of a job resource running the container `whatthehackmsft/content-init` to populate the MongoDB database.

```powershell
# run the mongo init job to create the "contentdb" database
# NOTE: be sure to set the MONGODB_CONNECTION env var to be the mongo service
kubectl apply -f .\init.yaml
```

Verify that the database is running and now includes a new "contentdb" database in the list

```powershell
# shell into pod and verify database creation
kubectl exec -it PODNAME -- bash
mongosh
show dbs;
```

## Update the app and load data 

The `:v2` variant of the web app has a slightly updated UI and is for the year 2022. The api variant relies on the MongoDB database that was created. 

**Make sure they set the `MONGODB_CONNECTION` environment variable in the API deployment.**

```powershell
# update to v2
kubectl apply -f .\api-deployment-v2.yaml -f .\web-deployment-v2.yaml

# check the rollout history of api and web deployments
kubectl rollout history deploy api, web
```
*NOTE: you can view the current revision number of the deployment by viewing the deployment.kubernetes.io/revision annotation. History is kept via ReplicaSets, each tied to a specific version.*