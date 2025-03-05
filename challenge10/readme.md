# Challenge 10 - Ingress

## Part 1

This simply adds a FQDN to the public Azure DNS. The following should be added to the web service resource they created in challenge 4. Here is an [example](./web-service-part1.yaml).

```yaml
metadata:
  annotations:
    service.beta.kubernetes.io/azure-dns-label-name: mycustomfqdn
```

## Part 2a and 2b

This part requires the use of `Ingress` APIs and requires an ingress controller. This can be added manually via Helm or by the managed add-on. 

### Using the Managed Add-On

```powershell
az aks approuting enable -n wthcluster -g rg-wth
```

Update the default `NginxIngressController` and add the following annotation:

```yaml
spec:
  loadBalancerAnnotations: 
    service.beta.kubernetes.io/azure-dns-label-name: "mycustomfqdn"
```

### Using Helm

```powershell
# add the local chart repository named ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# update the local repository list
helm repo update

# install the chart from the repository
helm install ingress-nginx ingress-nginx/ingress-nginx `
  -n ingress `
  --create-namespace `
  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz `
  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="mycustomfqdn"
```

## Cleanup

```powershell
# uninstall the app-routing add-on
az aks approuting disable -n wthcluster -g rg-wth
```