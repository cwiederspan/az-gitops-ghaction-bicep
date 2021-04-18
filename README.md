# GitOps using GitHub Actions with Azure

## Setup

```bashrc

AZURE_SUB_ID=XXX
AZURE_RG=cdw-azgitops-20210418
LOCATION=westus2

# Log into Azure
az login

# Create a Resource Group
az group create -n $AZURE_RG -l $LOCATION

# Create an SP for the Resource Group
az ad sp create-for-rbac \
    --name $AZURE_RG-sp \
    --role contributor \
    --scopes /subscriptions/$AZURE_SUB_ID/resourceGroups/$AZURE_RG \
    --sdk-auth \
    > azureauth.json

# Set the secret into a GitHub secret
gh secret set AZURE_CREDENTIALS < azureauth.json
gh secret set AZURE_RG -b"$AZURE_RG"
gh secret set AZURE_SUBSCRIPTION -b"$AZURE_SUB_ID"

# Delete the secret file just to be sure
rm azureauth.json



```

