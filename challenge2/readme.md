# Challenge 02 - The Azure Container Registry

```powershell
# create the resource group
az group create -n rg-wth -l eastus2

# create the ACR
az acr create -n wthacr02192015 -g rg-wth --sku Standard
```

Verify the ACR was created.

```powershell
# verify the ACR
az acr list -o table
```

Copy the Dockerfile files into their respective source code folders and build the images.

```powershell
# build content-api (in content-api folder)
az acr build -r wthacr02192015 -t content-api:latest -f .\Dockerfile .

# build content-web (in content-web folder)
az acr build -r wthacr02192015 -t content-web:latest -f .\Dockerfile .
```

## Tips
- **"The SKU is required"** - Basic or Standard is fine