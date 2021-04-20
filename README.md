# GitOps using GitHub Actions with Azure

This is an example of creating a GitOps-like approach for automatically executing
any [Azure Bicep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/bicep-overview)
files within the repository using a GitHub Action workflow.

In this case, there is a simple [main.bicep](./iac/main.bicep) file that will create an Azure storage account.

The [gitops.yaml](./.github/workflows/gitops.yaml) GitHub Action file that will use the 
[ARM Deploy](https://github.com/Azure/arm-deploy) action to execute the Bicep file whenever the repo is committed.

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

# Set the values into a GitHub secrets
gh secret set AZURE_CREDENTIALS -b"$AZURE_CREDS"
gh secret set AZURE_RG -b"$AZURE_RG"
gh secret set AZURE_SUBSCRIPTION -b"$AZURE_SUB_ID"

```

## Follow Up

Email chwieder@microsoft.com for more information on this repository.