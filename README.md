# GitOps using GitHub Actions with Azure

## Setup

```bashrc

# Create some variables for reuse
AZURE_SUB_ID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
AZURE_RG=cdw-azgitops-20210418
LOCATION=westus2

# Log into Azure
az login

# Create a Resource Group
az group create -n $AZURE_RG -l $LOCATION

# Create an SP for the Resource Group
AZURE_CREDS=$(az ad sp create-for-rbac \
    --name $AZURE_RG-sp \
    --role contributor \
    --scopes /subscriptions/$AZURE_SUB_ID/resourceGroups/$AZURE_RG \
    --sdk-auth)

# Set the secret into a GitHub secret
gh secret set AZURE_CREDENTIALS -b"$AZURE_CREDS"
gh secret set AZURE_RG -b"$AZURE_RG"
gh secret set AZURE_SUBSCRIPTION -b"$AZURE_SUB_ID"

```
