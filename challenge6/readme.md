# Challenge 06 - Deploy MongoDB to AKS

```powershell
# deploy mongodb deploy and service
kubectl apply -f .\mongo-deployment.yaml -f .\mongo-service.yaml

# shell into the pod and get version
kubectl exec -it PODNAME -- mongosh "--version"
```