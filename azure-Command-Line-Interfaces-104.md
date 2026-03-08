# Command Line Interfaces


| Categoría                         | Ejemplo          |
| --------------------------------- | ---------------- |
| **Command Line Interfaces (CLI)** | Azure CLI        |
| **Automation / Scripting Shell**  | Azure PowerShell |


- Azure CLI está **inspirado en la estructura de** ``git y kubectl``
- Azure PowerShell sigue el estándar **Verb-Noun de PowerShell** ``(Get/New/Set/Remove)``.


| Azure CLI | PowerShell | Description |
|-----------|------------|-------------|
| **Pattern:** `az <service> <verb>` | **Pattern:** `<Verb>-Az<Service>` | General command pattern used by each tool. Azure CLI groups commands by service then action; PowerShell uses verb-noun cmdlets. |
| `az login` | `Connect-AzAccount` | Authenticates to Azure and starts a session. |
| `az account show` | `Get-AzContext` | Shows the current subscription and tenant context being used. |
| `az account list` | `Get-AzSubscription` | Lists the subscriptions available to the logged-in user. |
| `az account set --subscription <id>` | `Set-AzContext -Subscription <id>` | Changes the active subscription used by subsequent commands. |
| `az group create --name <rg> --location <region>` | `New-AzResourceGroup -Name <rg> -Location <region>` | Creates a new Azure Resource Group. |
| `az group list` | `Get-AzResourceGroup` | Lists all resource groups in the current subscription. |
| `az group delete --name <rg>` | `Remove-AzResourceGroup -Name <rg>` | Deletes a resource group and all resources inside it. |
| `az vm create ...` | `New-AzVM ...` | Creates a new virtual machine. |
| `az vm list` | `Get-AzVM` | Lists virtual machines in the subscription or resource group. |
| `az vm start --name <vm> --resource-group <rg>` | `Start-AzVM -Name <vm> -ResourceGroupName <rg>` | Starts a virtual machine. |
| `az vm stop --name <vm> --resource-group <rg>` | `Stop-AzVM -Name <vm> -ResourceGroupName <rg>` | Stops or deallocates a virtual machine. |
| `az vm delete --name <vm> --resource-group <rg>` | `Remove-AzVM -Name <vm> -ResourceGroupName <rg>` | Deletes a virtual machine. |
| `az storage account create ...` | `New-AzStorageAccount ...` | Creates a new Azure Storage Account. |
| `az storage account list` | `Get-AzStorageAccount` | Lists storage accounts in the subscription. |
| `az storage account delete ...` | `Remove-AzStorageAccount ...` | Deletes a storage account. |
| `az network vnet create ...` | `New-AzVirtualNetwork ...` | Creates a virtual network (VNet). |
| `az network vnet list` | `Get-AzVirtualNetwork` | Lists virtual networks in the subscription. |
| `az network nsg create ...` | `New-AzNetworkSecurityGroup ...` | Creates a Network Security Group (NSG). |
| `az role assignment create ...` | `New-AzRoleAssignment ...` | Assigns an Azure RBAC role to a user, group, or service principal. |
| `az role assignment list` | `Get-AzRoleAssignment` | Lists RBAC role assignments. |
| `az policy assignment create ...` | `New-AzPolicyAssignment ...` | Assigns an Azure Policy to a scope (subscription, resource group, etc.). |
| `az policy assignment list` | `Get-AzPolicyAssignment` | Lists Azure Policy assignments. |



| PowerShell                       | Azure CLI      | Qué hace                      |
| -------------------------------- | -------------- | ----------------------------- |
| `Get-Command`                    | `az`           | Lista comandos disponibles    |
| `Get-Command *vm*`               | `az find vm`   | Busca comandos relacionados   |
| `Get-Command -Module Az.Compute` | `az vm`        | Lista comandos de un servicio |
| `Get-Help Get-AzVM`              | `az vm --help` | Muestra ayuda de un comando   |
