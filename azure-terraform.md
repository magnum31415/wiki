[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Terraform Troubleshooting Guide (azure-400)

## 🔍 1. Check if a Lock is Active

Before taking any action, verify whether the Terraform state file is currently locked.

Terraform uses Azure Blob leases to lock the state file during operations.

### Option 1 — Basic Check

```bash
az storage blob show \
  --account-name sttfstatealzlabweu01 \
  --container-name tfstate \
  --name mgmt-groups.tfstate \
  --query "properties.lease"
```

### Option 2 — Using Azure AD Authentication (Recommended)

```bash
az storage blob show \
  --account-name sttfstatealzlabweu01 \
  --container-name tfstate \
  --name mgmt-groups.tfstate \
  --query "properties.lease" \
  --auth-mode login
```

---

## 🔒 2. Breaking Terraform State Lock (Azure Storage)

When using Azure Storage as a backend for Terraform, the state file can become locked (for example, due to a failed or interrupted run).

If a lock is confirmed and no Terraform process is running, you can safely remove it.

### 🔧 Break the Lease (Remove Lock)

```bash
az storage blob lease break \
  --account-name sttfstatealzlabweu01 \
  --container-name tfstate \
  --blob-name mgmt-groups.tfstate \
  --auth-mode login
```

---

## 🧠 Notes

- Terraform uses blob leases to implement state locking.
- If a lease is active, Terraform operations like `plan` or `apply` will fail.
- Always verify that no other Terraform process is running before breaking the lease.
- Prefer Azure AD authentication (`--auth-mode login`) over storage account keys in enterprise environments.
- Breaking a lease manually should be considered a recovery action, not a normal workflow.

# Data Source

En Terraform, un bloque data no crea recursos. Su función es leer (consultar) un recurso que ya existe en Azure para poder utilizar sus atributos en el resto de la configuración.

En tu caso:
````
data "azurerm_network_watcher" "gwc" {
  provider = azurerm.connectivity

  name                = "NetworkWatcher_germanywestcentral"
  resource_group_name = "NetworkWatcherRG"
}
````
## ¿Qué hace exactamente?

Le dice a Terraform:

"Busca un Network Watcher que ya existe en Azure con este nombre y este Resource Group, y pon su información a mi disposición."

No crea nada.

## ¿Para qué sirve?

Por ejemplo, si más adelante quieres crear un Flow Log, necesitas indicar el network_watcher_id.

Puedes hacerlo así:
````

resource "azurerm_network_watcher_flow_log" "gwc" {
  network_watcher_name = data.azurerm_network_watcher.gwc.name
  resource_group_name  = data.azurerm_network_watcher.gwc.resource_group_name

  ...
}
````

o utilizando directamente su id:
````
data.azurerm_network_watcher.gwc.id
````


## ¿Qué información puedes obtener?

Por ejemplo:
````
data.azurerm_network_watcher.gwc.id

data.azurerm_network_watcher.gwc.name

data.azurerm_network_watcher.gwc.location

data.azurerm_network_watcher.gwc.resource_group_name
````

## Diferencia entre data y resource

### Data Source (lee recursos existentes)
````
data "azurerm_network_watcher" "gwc" {
  name                = "NetworkWatcher_germanywestcentral"
  resource_group_name = "NetworkWatcherRG"
}

````
- ✅ Consulta un recurso existente.
- ✅ No modifica Azure.
- ✅ No aparece como "to create" en el terraform plan.

### Resource (crea o gestiona recursos)
````
resource "azurerm_network_watcher" "gwc" {
  name                = "NetworkWatcher_germanywestcentral"
  location            = "germanywestcentral"
  resource_group_name = "NetworkWatcherRG"
}
````
- ✅ Terraform crea el recurso si no existe.
- ✅ Después lo gestiona en su estado (terraform.tfstate).
- ✅ Aparecerá en terraform plan como + create si aún no existe.

# TAGS
## locals.tf
````
# Common tags applied to all resources in the Connectivity platform.
locals {
  tags = {
    environment = "prod"
    managed_by  = "terraform"
    platform    = "connectivity"
  }
}
````

## extend local tags

````
tags = merge(local.tags, {
  environment = "dev"
  workload    = "vnet-flow-logs"
})
````
