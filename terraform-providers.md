[Terraform](https://github.com/magnum31415/wiki/blob/main/terraform.md)

# Providers en Terraform

Un **Provider** es un plugin que permite a Terraform comunicarse con una plataforma externa.

Sin providers Terraform no puede crear, modificar ni leer recursos.

Ejemplos:

| Provider | Plataforma |
|-----------|------------|
| azurerm | Azure |
| aws | Amazon AWS |
| google | Google Cloud |
| kubernetes | Kubernetes |
| github | GitHub |
| azuredevops | Azure DevOps |

---

# ¿Qué significa este bloque?

```hcl
required_providers {
  alz = {
    source  = "Azure/alz"
    version = "~> 0.21"
  }

  azurerm = {
    source  = "hashicorp/azurerm"
    version = "~> 4.0"
  }

  azapi = {
    source  = "Azure/azapi"
    version = "~> 2.0"
  }

  local = {
    source  = "hashicorp/local"
    version = "~> 2.5"
  }
}
```

Le indica a Terraform:

- Qué providers necesita descargar
- Desde dónde descargarlos
- Qué versión utilizar

Durante:

```bash
terraform init
```

Terraform descarga automáticamente estos plugins.

---

# Provider azurerm

```hcl
azurerm = {
  source  = "hashicorp/azurerm"
  version = "~> 4.0"
}
```

Es el provider principal de Azure.

Permite crear recursos Azure mediante Terraform.

Ejemplos:

```hcl
resource "azurerm_resource_group" "rg" {
}
```

```hcl
resource "azurerm_virtual_network" "vnet" {
}
```

```hcl
resource "azurerm_storage_account" "sa" {
}
```

```hcl
resource "azurerm_management_group" "mg" {
}
```

### Se utiliza para

- Resource Groups
- VNets
- Subnets
- NSGs
- Key Vaults
- Storage Accounts
- Azure Firewall
- VPN Gateway
- Management Groups
- Policies
- RBAC
- etc.

---

# Provider azapi

```hcl
azapi = {
  source  = "Azure/azapi"
  version = "~> 2.0"
}
```

Permite acceder directamente a las APIs ARM de Azure.

Se utiliza cuando:

- Azure acaba de sacar una funcionalidad nueva
- azurerm todavía no la soporta
- Necesitamos propiedades avanzadas

Ejemplo:

```hcl
resource "azapi_resource" "example" {
}
```

### Se utiliza para

- Recursos nuevos de Azure
- Configuraciones avanzadas
- Funcionalidades Preview
- Casos no soportados por azurerm

---

# Provider alz

```hcl
alz = {
  source  = "Azure/alz"
  version = "~> 0.21"
}
```

Provider específico del Azure Landing Zone Accelerator.

Permite trabajar con:

```text
Architectures
Archetypes
Policies
Policy Assignments
Role Assignments
Management Groups
```

En tu proyecto se utiliza junto con:

```text
lib/
├── archetype_definitions
├── architecture_definitions
└── alz_library_metadata.json
```

Este provider interpreta esos ficheros YAML y genera la estructura ALZ.

### Se utiliza para

- Azure Landing Zones
- Arquitecturas CAF
- Governance
- Guardrails
- Policy inheritance

---

# Provider local

```hcl
local = {
  source  = "hashicorp/local"
  version = "~> 2.5"
}
```

Permite trabajar con recursos locales del equipo o agente de pipeline.

Ejemplos:

```hcl
resource "local_file" "config" {
}
```

```hcl
data "local_file" "template" {
}
```

### Se utiliza para

- Crear ficheros
- Leer ficheros
- Generar configuraciones
- Procesar plantillas

---

# Diferencia entre azurerm y azapi

## azurerm

Más sencillo.

```hcl
resource "azurerm_virtual_network" "vnet" {
}
```

Terraform conoce todas las propiedades.

---

## azapi

Más flexible.

```hcl
resource "azapi_resource" "vnet" {
}
```

Permite llamar directamente a la API Azure.

---

# ¿Por qué aparecen varios providers azurerm?

Por ejemplo:

```hcl
provider "azurerm" {
  alias = "management"
}
```

```hcl
provider "azurerm" {
  alias = "connectivity"
}
```

Porque una Landing Zone suele desplegar recursos en varias subscriptions.

Ejemplo:

| Alias | Subscription |
|---------|---------|
| management | Management Subscription |
| connectivity | Connectivity Subscription |

Uso:

```hcl
resource "azurerm_resource_group" "rg" {
  provider = azurerm.management
}
```

o

```hcl
resource "azurerm_virtual_network" "vnet" {
  provider = azurerm.connectivity
}
```

---

# Resumen

| Provider | Para qué sirve |
|-----------|-----------|
| azurerm | Crear y gestionar recursos Azure |
| azapi | Acceder directamente a APIs ARM de Azure |
| alz | Gestionar Azure Landing Zone Accelerator |
| local | Crear o leer recursos locales (ficheros, plantillas, etc.) |
