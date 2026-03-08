# Command Line Interfaces


## Options to Run Automated Commands Against Azure

There are several ways to execute commands or scripts to manage Azure resources.

| Option | Description | Typical Use Cases |
|---|---|---|
| **PowerShell 7 from local machine** | Use PowerShell with the **Az module** installed on your computer to manage Azure resources. | Automation scripts, infrastructure management, integration with Windows environments. |
| **Azure CLI from local machine** | Use the **Azure CLI (`az`)** installed on your computer to execute commands against Azure. | Cross-platform scripting, DevOps pipelines, automation tasks. |
| **Azure Cloud Shell (portal.azure.com)** | Browser-based shell provided by Microsoft with **Azure CLI and Azure PowerShell preinstalled**. | Quick administration, testing commands, troubleshooting without installing tools locally. |

---

## Comparison

| Feature | PowerShell 7 | Azure CLI | Azure Cloud Shell |
|---|---|---|---|
| Installation required | Yes | Yes | No |
| Runs locally | Yes | Yes | No |
| Cross-platform | Yes | Yes | Yes |
| Supports scripting | Yes | Yes | Yes |
| Pre-authenticated session | No | No | Yes |
| Best for | PowerShell automation | Cross-platform scripts | Quick operations |

---

## Typical Automation Workflow

Examples of automation scenarios:

- Scheduled scripts from a server
- DevOps pipelines
- Infrastructure provisioning
- Resource management

Example with Azure CLI:

```bash
az vm start --name vm01 --resource-group rg-prod
```


---

## Azure CLI vs PowerShell

| Categoría                         | Ejemplo          |
| --------------------------------- | ---------------- |
| **Command Line Interfaces (CLI)** | Azure CLI        |
| **Automation / Scripting Shell**  | Azure PowerShell |


- Azure CLI está **inspirado en la estructura de** ``git y kubectl``
- Azure PowerShell sigue el estándar **Verb-Noun de PowerShell** ``(Get/New/Set/Remove)``.
  - Un **cmdlet* (se pronuncia command-let) es un comando nativo de PowerShell diseñado para realizar una tarea específica dentro del entorno de PowerShell.
  - Un **cmdlet** es una pequeña función especializada escrita en .NET que se ejecuta dentro de PowerShell para administrar sistemas o servicios.  


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


## Azure Cloud Shell

**Azure Cloud Shell** is a **browser-based interactive shell** provided by Microsoft to manage Azure resources without installing any tools locally.

It runs inside the Azure Portal and provides a ready-to-use environment with Azure management tools already installed.

### Key Characteristics

| Feature | Description |
|---|---|
| Browser based | Runs directly from the Azure Portal or shell.azure.com |
| No installation required | Azure CLI and PowerShell are preinstalled |
| Authenticated automatically | Uses the Azure account currently logged in |
| Persistent storage | Uses an Azure Storage account to persist files |
| Linux environment | Runs in a container hosted by Microsoft |


### Available Shells

Azure Cloud Shell supports two command environments:

| Shell | Description |
|---|---|
| **Bash** | Uses **Azure CLI (`az`)** commands |
| **PowerShell** | Uses **Azure PowerShell (`Az` modules)** |

### Relationship with Azure CLI

Azure Cloud Shell **is not Azure CLI itself**.

Instead, it is an **execution environment** where Azure CLI is already installed.

| Component | What it is |
|---|---|
| **Azure CLI** | Command-line tool (`az`) used to manage Azure |
| **Azure PowerShell** | PowerShell modules (`Az`) used to manage Azure |
| **Azure Cloud Shell** | Online shell environment that provides both tools |

## Example Usage in Cloud Shell

### Using Azure CLI

```bash
az vm list
```
```powershell
Get-AzVM
```

### Where It Can Be Used

Azure Cloud Shell can be accessed from:

- Azure Portal: https://shell.azure.com
- Azure mobile app: Embedded in Azure documentation
---

## PowerShell 7 

**PowerShell 7 (PowerShell Core) es multiplataforma**, a diferencia del antiguo **Windows PowerShell 5.1**, que solo funciona en Windows.

Se puede instalar en los siguientes sistemas:

| Sistema operativo | Soporte                                                                |
| ----------------- | ---------------------------------------------------------------------- |
| **Windows**       | Windows 10, Windows 11, Windows Server 2016, 2019, 2022                |
| **Linux**         | Ubuntu, Debian, RHEL, CentOS, Rocky Linux, AlmaLinux, Fedora, openSUSE |
| **macOS**         | macOS (Intel y Apple Silicon)                                          |
| **Docker**        | Imágenes oficiales de PowerShell                                       |
| **ARM**           | Windows ARM y Linux ARM                                                |

---

## Azure CLI

**Azure CLI** is a cross-platform command-line tool used to manage Azure resources.

It can be installed on multiple operating systems and environments.

---

## Supported Operating Systems

| Platform | Supported Systems |
|---|---|
| **Windows** | Windows 10, Windows 11, Windows Server 2016, 2019, 2022 |
| **Linux** | Ubuntu, Debian, RHEL, CentOS, Rocky Linux, AlmaLinux, Fedora, openSUSE |
| **macOS** | macOS (Intel and Apple Silicon) |

---

## Other Environments Where Azure CLI Is Available

| Environment | Description |
|---|---|
| **Azure Cloud Shell** | Browser-based shell with Azure CLI preinstalled |
| **Docker** | Official Microsoft container images with Azure CLI |
| **GitHub Actions** | Preinstalled in many CI/CD runners |
| **Azure DevOps Pipelines** | Available in Microsoft-hosted build agents |
| **WSL (Windows Subsystem for Linux)** | Can be installed inside Linux distributions running on Windows |

---

## Example Installation Methods

### Windows (winget)

```bash
winget install Microsoft.AzureCLI
