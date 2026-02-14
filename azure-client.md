[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# Config Azure Client

## Index

- [Config Azure Client](#config-azure-client)
  - [Installtion in Windows 11](#installtion-in-windows-11)
  - [Installtion in Ubuntu](#installtion-in-ubuntu)
  - [AzureCli Commands](#azurecli-commands)


## Installtion in Windows 11 

### Option A - PowerShell + Az module

Step 1 - Install PowerShell (if not already installed)
````powershell
winget search --id Microsoft.PowerShell
winget install --id Microsoft.PowerShell --source winget
pwsh --version
````

Step 2 - Install the Az module
````powershell
Install-Module -Name Az -AllowClobber -Repository PSGallery -Force
Get-InstalledModule -Name Az --AllVersions
````

Step 3 — Authenticate to Azure 
````powershell
Connect-AzAccount
#Check context
Get-AzContext
#Change subscription
Set-AzContext -Subscription "<SUBSCRIPTION_NAME_OR_ID>"
````

### Option B - Azure CLI (az)

Step 1 — Install Azure CLI
````powershell
winget install --exact --id Microsoft.AzureCLI
az --version
````

Step 2 — Authenticate to Azure (CLI)
````bash
az login
#Check current context
az account show
#List available tenants
az account tenant list
#Change subscription
az account set --subscription "<SUBSCRIPTION_NAME_OR_ID>"
````

## Installtion in Ubuntu


````bash
#Prerequisites
sudo apt update
sudo apt install -y ca-certificates curl apt-transport-https lsb-release gnupg
````

Add the Microsoft signing key
````bash
curl -sL https://packages.microsoft.com/keys/microsoft.asc \
  | gpg --dearmor \
  | sudo tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null
````

Add the Azure CLI repository
````bash
AZ_REPO=$(lsb_release -cs)
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" \
  | sudo tee /etc/apt/sources.list.d/azure-cli.list
````

Install Azure CLI
````bash
sudo apt update
sudo apt install -y azure-cli
az --version
````

Login to Azure
````bash
az login
````

## AzureCli Commands

- Login: ````az login````
- Login without browser: ````az login --use-device-code````

- Close Session and delte local tokens:
  ````bash
  az logout
  #remains folder ~/.azure without valid credentilas
  #Optional: to clean all
  rm -rf ~/.azure
  ````
- Check current context: ````az account show````
- List all tenants you have access to:  ````az account tenant list````
- List subscriptions: ````az account list --output table````
- Select a subscription explicitly ````az account set --subscription "<SUBSCRIPTION_NAME_OR_ID>"````
- Before running any script or discovery command, always do: ````az account show````
- For automation:
  ````bash
  az login --service-principal \
  -u <APP_ID> \
  -p <SECRET or CERT> \
  --tenant <TENANT_ID>
  ````
## AWS vs Azure

| AWS          | Azure           |
| ------------ | --------------- |
| Root user    | ❌ No existe     |
| IAM User     | Entra ID User   |
| IAM Role     | Azure RBAC Role |
| Account      | Subscription    |
| Organization | Tenant          |
| SCP          | Azure Policy    |

| Uso                       | Tipo de identidad       |
| ------------------------- | ----------------------- |
| Administración del tenant | Cuenta admin (poco uso) |
| Trabajo diario            | Usuario normal          |
| Automatización            | Service Principal       |
| Azure-native automation   | Managed Identity        |


