# Challenge 09 - Helm

## Locally Deployed

1. Create a [`templates`](./templates/) folder and move the files "helm-webapp-deployment.yml" and "helm-webapp-services.yml" into it. **There is no need to include the "helm-webapp-namespace.yml" file** because Helm will handle the namespace creation for us.
2. Modify the [`helm-webapp-deployment.yml`](./templates/helm-webapp-deployment.yml) file to use placeholders for the image name and tag:

    ```yaml
    spec:
      containers:
      - name: langfacts
        image: {{ .Values.image.name }}:{{ .Values.image.tag }}
    ```
3. Create a [`Chart.yaml`](./Chart.yaml) file that contains some information about the Helm deployment. 
4. Create a [`values.yaml`](./values.yaml) file that contains the default parameters for the chart. *This can technically be optional, as the couple of parameters used for this can be specified via `--set image.tag=v1` from the CLI.*

Next, invoke the Helm `install` command to create the release.

```powershell
# invoke Helm on the current directory to install a release
helm install langfacts . -n whatthehack --create-namespace
```

To change to `v2`, etc, the `update` command can be used.

```powershell
# update the release to change the image tag/version
helm upgrade langfacts . --set image.tag=v2 -n whatthehack
```

## Remotely Deployed (ACR)

This step packages the chart/template files into a zipped file and uploads it to the ACR. 

```powershell
# set the ACR to have admin enabled
az acr update -n wthacr012345abcde --admin-enabled true

# get the credentials
az acr credential show -n wthacr012345abcde -o table

# add ACR as a repository for helm (legacy)
az acr helm repo add -n wthacr012345abcde

# add ACR as a repository for helm (Helm 3 commands)
helm repo add wth https://wthacr012345abcde.azurecr.io/helm/v1/repo --username wthacr012345abcde --password YOUR_ACR_PASSWORD

# package the current chart folder
helm package .

# log into the ACR with your password
helm registry login wthacr012345abcde.azurecr.io -u wthacr012345abcde -p YOUR_ACR_PASSWORD

# upload the chart to the repository
helm push langfacts-1.0.0.tgz oci://wthacr012345abcde.azurecr.io/langfacts

# install using OCI
helm install langfacts oci://wthacr012345abcde.azurecr.io/langfacts/langfacts -n whatthehack --create-namespace

# NOTE: it appears this method no longer works unless the ACR Premium SKU is used
az acr helm push -n wthacr012345abcde langfacts-1.0.0.tgz
```