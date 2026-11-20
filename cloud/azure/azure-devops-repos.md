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


az repos list --organization https://dev.azure.com/groupit-company --project external-libs -o table
ID                                    Name                                                              Default Branch                                       Project
------------------------------------  ----------------------------------------------------------------  ---------------------------------------------------  -------------
00000000-0000-4000-8000-000000000d01  external-libs                                                                                                          external-libs
00000000-0000-4000-8000-000000000d02  terraform-azurerm-avm-ptn-alz                                     avm-bot/managed-files-sync-20260609102131            external-libs
00000000-0000-4000-8000-000000000d03  terraform-azurerm-avm-ptn-alz-connectivity-hub-and-spoke-vnet     copilot/add-primary-region-identification            external-libs
00000000-0000-4000-8000-000000000d04  terraform-azurerm-avm-ptn-alz-connectivity-virtual-wan            copilot/add-primary-region-identification            external-libs
00000000-0000-4000-8000-000000000d05  terraform-azurerm-avm-ptn-alz-management                          avm-bot/managed-files-sync-20260609102358            external-libs
00000000-0000-4000-8000-000000000d06  terraform-azurerm-avm-ptn-monitoring-amba-alz                     arjenhuitema-fix-example-azapi-provider-alias        external-libs
00000000-0000-4000-8000-000000000d07  terraform-azurerm-avm-ptn-network-private-link-private-dns-zones  avm-bot/managed-files-sync-20260609103458            external-libs
00000000-0000-4000-8000-000000000d08  terraform-azurerm-avm-res-network-bastionhost                     avm-bot/managed-files-sync-20260609111944            external-libs
00000000-0000-4000-8000-000000000d09  terraform-azurerm-avm-res-network-ddosprotectionplan              avm-bot/managed-files-sync-20260609112053            external-libs
00000000-0000-4000-8000-000000000d10  terraform-azurerm-avm-res-network-dnsresolver                     dependabot/github_actions/github-actions-2c2ec0b08c  external-libs
00000000-0000-4000-8000-000000000d11  terraform-azurerm-avm-res-network-firewallpolicy                  avm-bot/managed-files-sync-20260609112328            external-libs
00000000-0000-4000-8000-000000000d12  terraform-azurerm-avm-res-network-privatednszone                  copilot/add-support-for-resource-locks               external-libs
00000000-0000-4000-8000-000000000d13  terraform-azurerm-avm-res-network-publicipaddress                 copilot/fix-95                                       external-libs
00000000-0000-4000-8000-000000000d14  terraform-azurerm-avm-res-network-virtualnetwork                  chore/avm-authoring-0.10.1                           external-libs
00000000-0000-4000-8000-000000000d15  terraform-azurerm-avm-res-resources-resourcegroup                 copilot/sub-pr-122                                   external-libs
00000000-0000-4000-8000-000000000d16  terraform-azurerm-avm-utl-network-ip-addresses                    dependabot/github_actions/github-actions-2c2ec0b08c  external-libs
00000000-0000-4000-8000-000000000d17  terraform-azurerm-avm-utl-regions                                 dependabot/github_actions/github-actions-8773a0fc5d  external-libs

````
