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
c4c4001c-284a-4f92-8ba8-bf2364c571bc  external-libs                                                                                                          external-libs
5434794c-c296-4eed-a237-0de8063d0c8e  terraform-azurerm-avm-ptn-alz                                     avm-bot/managed-files-sync-20260609102131            external-libs
eae2e33c-09f6-466f-8e67-eca6442d6321  terraform-azurerm-avm-ptn-alz-connectivity-hub-and-spoke-vnet     copilot/add-primary-region-identification            external-libs
3c32d82b-a39f-4fdb-b096-16f70af0fdd9  terraform-azurerm-avm-ptn-alz-connectivity-virtual-wan            copilot/add-primary-region-identification            external-libs
87780b89-9a4e-4921-a5fb-d0c47090d4f6  terraform-azurerm-avm-ptn-alz-management                          avm-bot/managed-files-sync-20260609102358            external-libs
d5715e4b-ae62-438f-aec1-584f2b0b5b66  terraform-azurerm-avm-ptn-monitoring-amba-alz                     arjenhuitema-fix-example-azapi-provider-alias        external-libs
e981f197-cac9-407b-afa2-0dd3cbba2f37  terraform-azurerm-avm-ptn-network-private-link-private-dns-zones  avm-bot/managed-files-sync-20260609103458            external-libs
343a3090-3180-4043-b67e-dfcf6526146a  terraform-azurerm-avm-res-network-bastionhost                     avm-bot/managed-files-sync-20260609111944            external-libs
034e89e0-15e3-4d27-8da6-2b44b7a0132a  terraform-azurerm-avm-res-network-ddosprotectionplan              avm-bot/managed-files-sync-20260609112053            external-libs
a8efdc51-a8d0-48d7-9b09-9d742573a122  terraform-azurerm-avm-res-network-dnsresolver                     dependabot/github_actions/github-actions-2c2ec0b08c  external-libs
05a3b5e7-e575-4ff0-a3c4-89a38b1f5c0a  terraform-azurerm-avm-res-network-firewallpolicy                  avm-bot/managed-files-sync-20260609112328            external-libs
93fece43-f424-49c2-8992-7136ea2e5b3c  terraform-azurerm-avm-res-network-privatednszone                  copilot/add-support-for-resource-locks               external-libs
02673634-ff21-40ef-899f-f58691cd4368  terraform-azurerm-avm-res-network-publicipaddress                 copilot/fix-95                                       external-libs
46c2f95c-4601-4215-95d9-153f0fca4ec9  terraform-azurerm-avm-res-network-virtualnetwork                  chore/avm-authoring-0.10.1                           external-libs
166d4322-d89f-4202-9fe4-691020fe5cde  terraform-azurerm-avm-res-resources-resourcegroup                 copilot/sub-pr-122                                   external-libs
79f214ca-5751-4a8f-8758-0e043283257c  terraform-azurerm-avm-utl-network-ip-addresses                    dependabot/github_actions/github-actions-2c2ec0b08c  external-libs
7e8417b8-50f2-4af4-881a-5003ca2fbac0  terraform-azurerm-avm-utl-regions                                 dependabot/github_actions/github-actions-8773a0fc5d  external-libs

````
