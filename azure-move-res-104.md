[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
## Index

- [Moving Resources in Azure: Native Move vs Azure Resource Mover](#moving-resources-in-azure-native-move-vs-azure-resource-mover)
  - [1. Native Move (Azure Resource Manager Move)](#1-native-move-azure-resource-manager-move)
  - [2. Azure Resource Mover](#2-azure-resource-mover)
  - [3. Comparación rápida](#3-comparación-rápida)
  - [4. Regla simple](#4-regla-simple)
- [Azure Resource Mobility Matrix (Resource Group, Subscription, Region)](#azure-resource-mobility-matrix-resource-group-subscription-region)
  
# Moving Resources in Azure: Native Move vs Azure Resource Mover

## 1. Native Move (Azure Resource Manager Move)

### Qué es

El **Native Move** es la operación estándar de **Azure Resource Manager (ARM)** que permite mover recursos **entre Resource Groups o entre Subscriptions dentro de la misma región**.

Esta operación se ejecuta mediante:

- Azure Portal
- Azure CLI
- Azure PowerShell
- Azure REST API

Internamente utiliza la operación ARM: ``POST .../moveResources``


No implica recrear el recurso.

El recurso **mantiene su identidad y configuración original**.

---

### Características

| Característica | Native Move |
|---|---|
| Movimiento entre Resource Groups | Sí |
| Movimiento entre Subscriptions | Sí |
| Movimiento entre Regiones | No |
| Recreación del recurso | No |
| Cambia Resource ID | Sí |
| Cambia dirección IP | No |
| Downtime | Normalmente no |

---

### Qué ocurre técnicamente

Azure simplemente **actualiza el Resource ID** del recurso.

Ejemplo:

- **Antes** ``/subscriptions/sub1/resourceGroups/rg-old/providers/Microsoft.Compute/virtualMachines/vm01``
- **Después** ``/subscriptions/sub1/resourceGroups/rg-new/providers/Microsoft.Compute/virtualMachines/vm01``

El recurso **sigue siendo exactamente el mismo objeto en Azure**.

---

### Cuándo se usa

Se usa cuando se necesita:

- reorganizar **Resource Groups**
- reorganizar **Subscriptions**
- mejorar la **gobernanza**
- adaptar recursos a una nueva **estructura de landing zone**

---

## 2. Azure Resource Mover

### Qué es

**Azure Resource Mover** es un servicio de Azure diseñado para **mover recursos entre regiones**.

No realiza un movimiento directo.

En realidad ejecuta un proceso de: ``replicate → recreate → switch``


Es decir:

1. Replica la configuración del recurso
2. Replica datos o discos
3. Crea un recurso equivalente en la región destino
4. Cambia dependencias
5. Permite eliminar el recurso original

---

### Características

| Característica | Azure Resource Mover |
|---|---|
| Movimiento entre Resource Groups | No |
| Movimiento entre Subscriptions | No |
| Movimiento entre Regiones | Sí |
| Recreación del recurso | Sí |
| Cambia Resource ID | Sí |
| Cambia dirección IP | Sí |
| Downtime | Puede haber |

---

### Qué ocurre técnicamente

El recurso **se vuelve a crear en la nueva región**.

Ejemplo:


| Estado | Región | VM |
|---|---|---|
| VM original | West Europe | prod-vm01 |
| VM destino | North Europe | prod-vm01 |


El recurso destino **no es el mismo objeto**, sino uno **nuevo basado en el original**.

---

### Recursos que puede mover

Azure Resource Mover soporta mover:

| Tipo de recurso |
|---|
| Virtual Machines |
| Managed Disks |
| Network Interfaces |
| Virtual Networks |
| Network Security Groups |
| Availability Sets |

---

### Cuándo se usa

Se usa cuando se necesita:

- mover infraestructura entre **regiones**
- realizar **disaster recovery relocation**
- migrar cargas a otra región
- optimizar **latencia o compliance**

---

## 3. Comparación rápida

| Característica | Native Move | Azure Resource Mover |
|---|---|---|
| Tipo de operación | Reorganización | Migración |
| Cambia región | No | Sí |
| Recrea recurso | No | Sí |
| Cambia Resource ID | Sí | Sí |
| Tiempo de operación | Muy rápido | Más largo |
| Impacto en servicio | Bajo | Puede existir |

---

## 4. Regla simple

- Resource Group move → Native Move
- Subscription move → Native Move
- Region move → Azure Resource Mover


---

| Caso | Movimiento | Ejemplo | Solución |
|---|---|---|---|
| Caso 1 | Mover VM a otro Resource Group | rg-app-old → rg-app-prod | Native Move |
| Caso 2 | Mover VM a otra Subscription | subscription-dev → subscription-prod | Native Move |
| Caso 3 | Mover VM a otra región | West Europe → North Europe | Azure Resource Mover |


# Azure Resource Mobility Matrix (Resource Group, Subscription, Region)

La tabla muestra **qué recursos de Azure pueden moverse entre Resource Groups, Subscriptions y Regions** y qué método permite hacerlo.  
También indica **si el movimiento se puede realizar con el método nativo de Azure (ARM Move) o requiere herramientas como Azure Resource Mover**.

[MSFT - allowed move resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources)

| Resource Type | Native Move (Resource Group) | Native Move (Subscription) | Native Move (Region) | Move with Azure Resource Mover (Region) |
|---|---|---|---|---|
| Virtual Machine (VM) | ✅ | ✅ | ❌ | ✅ |
| Managed Disk | ✅ | ✅ | ❌ | ✅ |
| Availability Set | ✅ | ✅ | ❌ | ✅ |
| Network Interface (NIC) | ✅ | ✅ | ❌ | ✅ |
| Virtual Network (VNet) | ✅ | ✅ | ❌ | ✅ |
| Subnet | ❌ | ❌ | ❌ | ❌ |
| Network Security Group (NSG) | ✅ | ✅ | ❌ | ✅ |
| Public IP | ⚠️ (depends on SKU / association) | ⚠️ (depends on SKU / association) | ❌ | ⚠️ (depends on SKU) |
| Load Balancer | ⚠️ (Standard SKU only) | ⚠️ (Standard SKU only) | ❌ | ❌ |
| Application Gateway | ⚠️ (depends on SKU / configuration) | ⚠️ (depends on SKU / configuration) | ❌ | ❌ |
| Azure Firewall | ⚠️ (limited scenarios) | ⚠️ (limited scenarios) | ❌ | ❌ |
| VPN Gateway | ❌ | ❌ | ❌ | ❌ |
| ExpressRoute Gateway | ❌ | ❌ | ❌ | ❌ |
| Storage Account | ✅ | ✅ | ❌ | ❌ |
| Azure SQL Server | ✅ | ✅ | ❌ | ❌ |
| Azure SQL Database | ⚠️ (same server) | ❌ | ❌ | ❌ |
| Cosmos DB Account | ✅ | ✅ | ❌ | ❌ |
| Key Vault | ✅ | ✅ | ❌ | ❌ |
| App Service Plan | ✅ | ✅ | ❌ | ❌ |
| Web App (App Service) | ✅ | ✅ | ❌ | ❌ |
| Function App | ✅ | ✅ | ❌ | ❌ |
| Container Registry | ✅ | ✅ | ❌ | ❌ |
| Log Analytics Workspace | ✅ | ✅ | ❌ | ❌ |
| Recovery Services Vault | ❌ | ❌ | ❌ | ❌ |
| Backup Vault | ❌ | ❌ | ❌ | ❌ |
| Azure Kubernetes Service (AKS) | ❌ | ❌ | ❌ | ❌ |
| Managed Identity | ✅ | ✅ | ❌ | ❌ |
| Private Endpoint | ⚠️ (depends on dependencies) | ⚠️ (depends on dependencies) | ❌ | ❌ |
| Route Table | ✅ | ✅ | ❌ | ❌ |
| Bastion Host | ⚠️ (depends on configuration) | ⚠️ (depends on configuration) | ❌ | ❌ |
| Service Bus Namespace | ✅ | ✅ | ❌ | ❌ |
| Event Hub Namespace | ✅ | ✅ | ❌ | ❌ |
| Redis Cache | ⚠️ (depends on SKU) | ⚠️ (depends on SKU) | ❌ | ❌ |
| Data Factory | ✅ | ✅ | ❌ | ❌ |
| Synapse Workspace | ✅ | ✅ | ❌ | ❌ |
