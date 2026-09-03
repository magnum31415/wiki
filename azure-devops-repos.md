# azure-devops-repos


````
az extension add --name azure-devops

az devops configure --defaults organization=https://dev.azure.com/groupit-company project=external-libs

az devops configure --defaults organization=https://dev.azure.com/groupit-company project=external-libs 

La primera vez que ejecutes cualquier comando az devops o az repos, te pedirá un PAT de Azure DevOps 
(no usa tu sesión de az login automáticamente). 
Genera uno en https://dev.azure.com/groupit-company/_usersSettings/tokens con scope 'Code (Read, write & manage)' y 'Project and Team (Read, write & manage)', y pégalo cuando te lo pida.

az repos list --organization https://dev.azure.com/groupit-company --project external-libs -o table


MODULES=(
  "terraform-azurerm-avm-ptn-alz"
  "terraform-azurerm-avm-ptn-alz-connectivity-hub-and-spoke-vnet"
  "terraform-azurerm-avm-ptn-alz-connectivity-virtual-wan"
  "terraform-azurerm-avm-ptn-alz-management"
  "terraform-azurerm-avm-ptn-monitoring-amba-alz"
  "terraform-azurerm-avm-ptn-network-private-link-private-dns-zones"
  "terraform-azurerm-avm-res-network-bastionhost"
  "terraform-azurerm-avm-res-network-ddosprotectionplan"
  "terraform-azurerm-avm-res-network-dnsresolver"
  "terraform-azurerm-avm-res-network-firewallpolicy"
  "terraform-azurerm-avm-res-network-privatednszone"
  "terraform-azurerm-avm-res-network-publicipaddress"
  "terraform-azurerm-avm-res-network-virtualnetwork"
  "terraform-azurerm-avm-res-resources-resourcegroup"
  "terraform-azurerm-avm-utl-network-ip-addresses"
  "terraform-azurerm-avm-utl-regions"
)

for repo in "${MODULES[@]}"; do
  echo "Creating repo: $repo"
  az repos create --organization https://dev.azure.com/groupit-company --project external-libs --name "$repo"
done



ORG="groupit-company"
PROJECT="external-libs"

MODULES=(
  "terraform-azurerm-avm-ptn-alz"
  "terraform-azurerm-avm-ptn-alz-connectivity-hub-and-spoke-vnet"
  "terraform-azurerm-avm-ptn-alz-connectivity-virtual-wan"
  "terraform-azurerm-avm-ptn-alz-management"
  "terraform-azurerm-avm-ptn-monitoring-amba-alz"
  "terraform-azurerm-avm-ptn-network-private-link-private-dns-zones"
  "terraform-azurerm-avm-res-network-bastionhost"
  "terraform-azurerm-avm-res-network-ddosprotectionplan"
  "terraform-azurerm-avm-res-network-dnsresolver"
  "terraform-azurerm-avm-res-network-firewallpolicy"
  "terraform-azurerm-avm-res-network-privatednszone"
  "terraform-azurerm-avm-res-network-publicipaddress"
  "terraform-azurerm-avm-res-network-virtualnetwork"
  "terraform-azurerm-avm-res-resources-resourcegroup"
  "terraform-azurerm-avm-utl-network-ip-addresses"
  "terraform-azurerm-avm-utl-regions"
)

for repo in "${MODULES[@]}"; do
  echo "=== Importing $repo ==="
  az repos import create \
    --organization "https://dev.azure.com/${ORG}" \
    --project "${PROJECT}" \
    --repository "${repo}" \
    --git-source-url "https://github.com/Azure/${repo}.git"
done


````
