[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
## Index

- [Moving Resources in Azure: Native Move vs Azure Resource Mover](#moving-resources-in-azure-native-move-vs-azure-resource-mover)
  - [1. Native Move (Azure Resource Manager Move)](#1-native-move-azure-resource-manager-move)
  - [2. Azure Resource Mover](#2-azure-resource-mover)
  - [3. Comparación rápida](#3-comparación-rápida)
  - [4. Regla simple](#4-regla-simple)
- [Azure Resource Mobility Matrix (Resource Group, Subscription, Region)](#azure-resource-mobility-matrix-resource-group-subscription-region)
- [Azure Resource Move - Resource Group, Region y Azure Policy (AZ-104)](#azure-resource-move---resource-group-region-y-azure-policy-az-104)
  
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


---

# Azure Resource Move - Resource Group, Region y Azure Policy (AZ-104)

---

# Concepto clave

Mover un recurso Azure entre:

```text
Resource Groups
```

NO cambia:

```text
la región física del recurso
```

PERO sí cambia:

```text
las políticas y RBAC heredados
```

del nuevo scope.

---

# Qué ocurre al mover un recurso

Cuando mueves un recurso a otro:

| Elemento | Cambia |
|---|---|
| Resource Group | ✅ |
| Azure Policies aplicadas | ✅ |
| RBAC heredado | ✅ |
| Región física (location) | ❌ |
| Datacenter Azure | ❌ |

---

# La clave de la pregunta

## Situación inicial

```text
CustomerWeb01
```

↓

Está en:

```text
ResGrp1
```

↓

Región:

```text
West Europe
```

↓

Policy aplicada:

```text
Pol1
```

---

# Después del move

```text
CustomerWeb01
```

↓

Se mueve a:

```text
ResGrp2
```

---

# Qué NO cambia

La región:

```text
West Europe
```

permanece igual.

---

# Por qué

Mover un recurso entre Resource Groups:

```text
NO redeploya el recurso
```

NO:
- recrea recursos
- migra región
- cambia datacenter

Simplemente cambia:

```text
su scope administrativo
```

---

# Importante examen

Para cambiar de región Azure normalmente necesitas:

- redeployment
- migration
- replication
- recreate resource

NO un simple:

```text
Move Resource
```

---

# Qué SÍ cambia

Ahora el recurso pertenece a:

```text
ResGrp2
```

↓

Por tanto:

```text
hereda las policies de ResGrp2
```

↓

Se aplica:

```text
Pol2
```

---

# Regla mental correcta

## Región

La región depende del:

```text
resource location
```

NO del Resource Group.

---

## Policies

Las Azure Policies dependen del:

```text
scope actual
```

↓

Management Group
Subscription
Resource Group
Resource

---

# Resultado correcto

| Elemento | Resultado |
|---|---|
| Región | West Europe |
| Policy aplicada | Pol2 |

---

# Por qué las otras respuestas son incorrectas

---

# ❌ "App Service plan moves to North Europe"

Incorrecto porque:

```text
mover Resource Group ≠ mover región
```

---

# ❌ "Pol1 applies"

Incorrecto porque:

```text
las policies se heredan del nuevo Resource Group
```

↓

Ahora aplica:

```text
Pol2
```

---

# Concepto importante Azure Policy

Azure Policy se evalúa según:

```text
scope actual del recurso
```

---

# Ejemplo típico

## Antes

```text
ResGrp1
 └── Policy: Deny Public IP
```

↓

VM dentro de ResGrp1:

```text
NO puede crear Public IP
```

---

## Después move a ResGrp2

```text
ResGrp2
 └── Policy: Allow Public IP
```

↓

Ahora:

```text
sí puede crear Public IP
```

---

# Diferencia importante examen

| Concepto | Depende de |
|---|---|
| Región recurso | Resource location |
| Azure Policy | Scope actual |
| RBAC | Scope actual |
| Tags | Pueden mantenerse |
| Datacenter físico | Región original |

---

# Arquitectura conceptual

## Antes

```text
ResGrp1
 └── Pol1
      └── CustomerWeb01 (West Europe)
```

---

## Después

```text
ResGrp2
 └── Pol2
      └── CustomerWeb01 (West Europe)
```

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
Resource Group determina región
```

❌ Incorrecto.

---

# Trampa 2

Pensar que mover RG mueve físicamente recursos.

❌ Incorrecto.

---

# Trampa 3

Pensar que policies antiguas permanecen.

❌ Incorrecto.

↓

Se aplican las del nuevo scope.

---

# Tabla resumen examen

| Operación | Cambia región | Cambia Policy |
|---|---|---|
| Move Resource Group | ❌ | ✅ |
| Redeploy Resource | ✅ posible | ✅ |
| Move Subscription | ❌ | ✅ |
| Change Management Group | ❌ | ✅ |

---

# Reglas rápidas AZ-104

```text
Moving a resource between resource groups does not change its region.
```

```text
Azure Policies are inherited from the current scope.
```

```text
A moved resource inherits policies from the destination resource group.
```

---

# Frases clave AZ-104

```text
Resource location is independent from the resource group.
```

```text
Azure Policy is scope-based.
```

```text
Moving a resource changes its administrative scope, not its physical location.
```
