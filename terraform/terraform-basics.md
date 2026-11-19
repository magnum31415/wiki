# Terraform - Guía de Repaso Rápido

## ¿Qué es Terraform y para qué sirve?

Terraform es una herramienta de Infrastructure as Code (IaC) desarrollada por HashiCorp.

Permite definir infraestructura mediante código declarativo y desplegarla de forma automática en múltiples plataformas:

- Azure
- AWS
- Google Cloud
- Kubernetes
- GitHub
- Azure DevOps
- VMware
- etc.

### Ventajas

- Infraestructura versionada en Git
- Reproducible
- Automatizable
- Idempotente
- Multi-cloud

---

# Orden en que Terraform interpreta los ficheros

Terraform NO ejecuta los ficheros por orden alfabético.

Todos los ficheros `.tf` de un directorio se combinan como si fueran un único fichero.

## Orden lógico interno

1. Lee todos los `.tf`
2. Carga variables
3. Carga locals
4. Inicializa providers
5. Lee data sources
6. Construye el grafo de dependencias
7. Calcula recursos y módulos
8. Calcula outputs
9. Ejecuta según dependencias

## Estructura habitual

```text
terraform.tf
variables.tf
locals.tf
data.tf
main.tf
outputs.tf
```

---

# Conceptos básicos y sintaxis HCL

## Resource

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "rg-demo"
  location = "westeurope"
}
```

## Variable

```hcl
variable "location" {
  type = string
}
```

## Output

```hcl
output "rg_name" {
  value = azurerm_resource_group.rg.name
}
```

## Local

```hcl
locals {
  region = "westeurope"
}
```

## Data Source

```hcl
data "azurerm_client_config" "current" {}
```

---

# Flujo de trabajo Terraform

## Inicializar

```bash
terraform init
```

## Formatear

```bash
terraform fmt
```

## Validar

```bash
terraform validate
```

## Generar plan

```bash
terraform plan
```

## Aplicar cambios

```bash
terraform apply
```

## Destruir recursos

```bash
terraform destroy
```

---

# State y Backend

## Terraform State

Terraform guarda el estado real en:

```text
terraform.tfstate
```

Contiene:

- Recursos creados
- IDs
- Dependencias
- Metadata

## Backend Local

```text
terraform.tfstate
```

## Backend Azure

```hcl
terraform {
  backend "azurerm" {}
}
```

Ejemplo de inicialización:

```bash
terraform init \
-backend-config="resource_group_name=rg-tfstate" \
-backend-config="storage_account_name=sttfstate" \
-backend-config="container_name=tfstate" \
-backend-config="key=alz.tfstate"
```

### Ventajas

- State compartido
- Locking
- Versionado
- Trabajo en equipo

---

# ¿Qué recursos se tienen que definir obligatoriamente?

Terraform NO obliga a crear recursos.

Un proyecto mínimo necesita:

```hcl
terraform {
  required_providers {}
}
```

Normalmente se definen:

## Terraform Block

```hcl
terraform {}
```

## Provider

```hcl
provider "azurerm" {}
```

## Resource

```hcl
resource "..." "..." {}
```

---

# Variables y Outputs

## Variable

```hcl
variable "location" {
  type = string
}
```

## Uso

```hcl
location = var.location
```

## tfvars

```hcl
location = "westeurope"
```

## Output

```hcl
output "location" {
  value = var.location
}
```

---

# Data Sources

Permiten leer recursos existentes.

## Ejemplo

```hcl
data "azurerm_client_config" "current" {}
```

Uso:

```hcl
tenant_id = data.azurerm_client_config.current.tenant_id
```

## Otro ejemplo

```hcl
data "azurerm_resource_group" "network" {
  name = "rg-network"
}
```

---

# Dependencias

## Implícitas

Terraform las detecta automáticamente.

```hcl
resource_group_name = azurerm_resource_group.rg.name
```

## Explícitas

```hcl
depends_on = [
  azurerm_resource_group.rg
]
```

---

# Expressions y Functions

## Referencias

```hcl
var.location
```

```hcl
local.region
```

```hcl
module.network.id
```

## Functions

### lower

```hcl
lower("HELLO")
```

### upper

```hcl
upper("hello")
```

### join

```hcl
join("-", ["dev","app","01"])
```

### split

```hcl
split("-", "dev-app-01")
```

### format

```hcl
format("%s-%s", "dev", "web")
```

### length

```hcl
length(var.items)
```

### contains

```hcl
contains(var.regions, "westeurope")
```

---

# Loops

## count

```hcl
resource "azurerm_resource_group" "rg" {
  count = 3

  name     = "rg-${count.index}"
  location = "westeurope"
}
```

## for_each

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = toset([
    "dev",
    "test",
    "prod"
  ])

  name     = "rg-${each.key}"
  location = "westeurope"
}
```

---

# Modules

Permiten reutilizar código.

## Llamada

```hcl
module "network" {
  source = "./modules/network"

  location = "westeurope"
}
```

## Estructura

```text
modules/
└── network/
    ├── main.tf
    ├── variables.tf
    └── outputs.tf
```

---

# Terraform para Azure Landing Zone

## Management Groups

```hcl
resource "azurerm_management_group" "platform" {
  display_name = "Platform"
}
```

Ejemplo:

```text
Tenant Root
│
├── Platform
│   ├── Connectivity
│   ├── Identity
│   └── Management
│
├── LandingZones
│   ├── Corp
│   └── Online
│
├── Sandbox
└── Decommissioned
```

## Policy Assignment

```hcl
resource "azurerm_management_group_policy_assignment" "allowed_locations" {
  name                 = "allowed-locations"
  management_group_id  = azurerm_management_group.platform.id
  policy_definition_id = data.azurerm_policy_definition.allowed_locations.id
}
```

---

# Terraform en Azure DevOps

## Flujo típico

```text
Git
 │
 ├── Pull Request
 │
 ├── terraform fmt
 │
 ├── terraform validate
 │
 ├── terraform plan
 │
 ├── Approval
 │
 └── terraform apply
```

## OIDC / Federated Identity

Azure DevOps NO almacena secretos.

Flujo:

```text
Azure DevOps
      │
      ▼
OIDC Token
      │
      ▼
Entra ID
      │
      ▼
Service Principal
      │
      ▼
Azure
```

Ventajas:

- Sin Client Secret
- Más seguro
- Rotación automática

---

# Temas avanzados que deberías dominar

## Modules complejos

- Módulos anidados
- Reutilización
- Arquitecturas enterprise

---

## Remote State Data

Leer outputs de otro state.

```hcl
data "terraform_remote_state" "network" {}
```

---

## Dynamic Blocks

Generación dinámica de bloques.

```hcl
dynamic "subnet" {
  for_each = var.subnets

  content {
    name = subnet.value.name
  }
}
```

---

## For Expressions

```hcl
[for rg in var.rgs : rg.name]
```

---

## Conditional Expressions

```hcl
var.env == "prod" ? 3 : 1
```

---

## Lifecycle

```hcl
lifecycle {
  prevent_destroy = true
}
```

```hcl
lifecycle {
  ignore_changes = [
    tags
  ]
}
```

---

## Import

Importar recursos existentes.

```bash
terraform import azurerm_resource_group.rg /subscriptions/xxx/resourceGroups/rg-demo
```

---

## Workspaces

Separar estados.

```bash
terraform workspace new dev
terraform workspace new prod
```

---

## Provider Aliases

```hcl
provider "azurerm" {
  alias = "management"
}
```

Uso:

```hcl
provider = azurerm.management
```

---

## AzureRM 4.x

Cambios importantes:

- subscription_id obligatorio
- Mejor soporte Azure
- Más recursos soportados
- Breaking changes respecto a 3.x

---

## OIDC/Federated Identity en Azure DevOps

Service Connection:

```text
Azure DevOps
      │
      ▼
OIDC
      │
      ▼
Service Principal
      │
      ▼
Azure
```

Evita:

```text
ARM_CLIENT_SECRET
```

---

## Testing de módulos

### Validación

```bash
terraform validate
```

### Formato

```bash
terraform fmt
```

### Plan

```bash
terraform plan
```

### Terratest

```text
Go + Terraform
```

### Checkov

```text
Análisis de seguridad
```

### TFLint

```text
Buenas prácticas Terraform
```
