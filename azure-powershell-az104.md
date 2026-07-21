[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)


- [⬅️ Volver a: Azure PowerShell - Cmdlets más importantes para el AZ-104](#azure-powershell---cmdlets-más-importantes-para-el-az-104)
- [⬅️ Volver a: ARM Templates - ¿Qué cmdlet utilizar para desplegar? (AZ-104)](#arm-templates---qué-cmdlet-utilizar-para-desplegar-az-104)

---

# Azure PowerShell - Cmdlets más importantes para el AZ-104

En el AZ-104 aparecen principalmente cmdlets relacionados con:

- ARM Templates / Bicep
- Resource Groups
- Virtual Machines
- Storage
- Virtual Networks
- Azure Backup
- Microsoft Entra ID (RBAC)

No es necesario memorizar todos los cmdlets de Azure PowerShell, pero sí reconocer los más habituales y, especialmente, **el ámbito (scope) donde despliegan recursos**.

---

# 1. ARM / Bicep Deployments ⭐⭐⭐⭐⭐

Estos son los cmdlets que más aparecen en el examen.

| Cmdlet | Ámbito | ¿Cuándo se usa? | Ejemplo |
|---------|--------|-----------------|----------|
| **New-AzResourceGroupDeployment** | Resource Group | Desplegar recursos dentro de un Resource Group | Crear una VM en RG1 |
| **New-AzSubscriptionDeployment** | Subscription | Crear Resource Groups, Policies o RBAC a nivel de suscripción | Crear un nuevo Resource Group |
| **New-AzManagementGroupDeployment** | Management Group | Desplegar políticas o RBAC para varias suscripciones | Asignar una Policy a un Management Group |
| **New-AzTenantDeployment** | Tenant | Desplegar recursos a nivel Tenant | Crear un Management Group |

---

## Regla para el examen

| Quiero crear... | Cmdlet |
|-----------------|---------|
| Recursos dentro de un Resource Group | **New-AzResourceGroupDeployment** |
| Resource Groups | **New-AzSubscriptionDeployment** |
| Policies para varias suscripciones | **New-AzManagementGroupDeployment** |
| Management Groups | **New-AzTenantDeployment** |

---

# 2. Resource Groups

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzResourceGroup** | Obtener Resource Groups | Listar todos los RG |
| **New-AzResourceGroup** | Crear Resource Group | Crear RG-Production |
| **Remove-AzResourceGroup** | Eliminar Resource Group | Borrar RG-Test |

---

# 3. Virtual Machines

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzVM** | Mostrar VMs | Obtener VM1 |
| **New-AzVM** | Crear una VM | Crear Windows Server |
| **Start-AzVM** | Encender una VM | Iniciar VM1 |
| **Stop-AzVM** | Apagar una VM | Detener VM1 |
| **Restart-AzVM** | Reiniciar una VM | Reiniciar VM1 |
| **Remove-AzVM** | Eliminar una VM | Borrar VM-Test |

---

# 4. Storage Accounts

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzStorageAccount** | Obtener Storage Accounts | Mostrar Storage1 |
| **New-AzStorageAccount** | Crear Storage Account | Crear stprod001 |
| **Remove-AzStorageAccount** | Eliminar Storage Account | Borrar Storage1 |

---

# 5. Blob Storage

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzStorageBlob** | Listar blobs | Mostrar blobs |
| **Set-AzStorageBlobContent** | Subir un blob | Subir backup.zip |
| **Get-AzStorageBlobContent** | Descargar un blob | Descargar informe.pdf |
| **Remove-AzStorageBlob** | Eliminar blob | Eliminar backup.zip |

---

# 6. Virtual Networks

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzVirtualNetwork** | Mostrar VNets | Obtener VNet1 |
| **New-AzVirtualNetwork** | Crear una VNet | Crear HUB-VNET |
| **Add-AzVirtualNetworkSubnetConfig** | Crear una subnet | Crear Subnet-App |
| **Set-AzVirtualNetwork** | Aplicar cambios a la VNet | Guardar una nueva subnet |

---

# 7. Network Security Groups

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzNetworkSecurityGroup** | Mostrar NSGs | Obtener NSG-Web |
| **New-AzNetworkSecurityGroup** | Crear NSG | Crear NSG-App |
| **Add-AzNetworkSecurityRuleConfig** | Crear regla | Permitir HTTPS |
| **Set-AzNetworkSecurityGroup** | Guardar cambios | Aplicar reglas |

---

# 8. Public IP

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **New-AzPublicIpAddress** | Crear Public IP | Crear PIP-AppGW |
| **Get-AzPublicIpAddress** | Mostrar Public IP | Ver PIP1 |

---

# 9. Azure Backup

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzRecoveryServicesVault** | Mostrar Recovery Services Vault | Obtener Vault1 |
| **Set-AzRecoveryServicesVaultContext** | Seleccionar Vault | Trabajar sobre Vault1 |
| **Backup-AzRecoveryServicesBackupItem** | Ejecutar Backup | Lanzar backup manual |

---

# 10. Azure Monitor

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzMetric** | Consultar métricas | CPU de una VM |
| **New-AzMetricAlertRuleV2** | Crear alerta | Alerta CPU > 80% |

---

# 11. Azure RBAC

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzRoleAssignment** | Ver permisos | Ver roles de User1 |
| **New-AzRoleAssignment** | Asignar rol | Dar Contributor |
| **Remove-AzRoleAssignment** | Eliminar rol | Quitar Reader |

---

# 12. Azure Policy

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzPolicyDefinition** | Mostrar Policies | Listar políticas |
| **New-AzPolicyAssignment** | Asignar Policy | Denegar Public IP |
| **Remove-AzPolicyAssignment** | Eliminar Policy | Quitar política |

---

# 13. Azure Key Vault

| Cmdlet | Descripción | Ejemplo |
|---------|-------------|----------|
| **Get-AzKeyVault** | Mostrar Key Vaults | Obtener KV1 |
| **New-AzKeyVault** | Crear Key Vault | Crear kv-prod |
| **Set-AzKeyVaultSecret** | Crear secreto | Guardar contraseña |

---

# 14. Los 10 cmdlets que más aparecen en el AZ-104

| Cmdlet | Debes conocerlo |
|---------|:---------------:|
| **New-AzResourceGroupDeployment** | ⭐⭐⭐⭐⭐ |
| **New-AzSubscriptionDeployment** | ⭐⭐⭐⭐⭐ |
| **New-AzManagementGroupDeployment** | ⭐⭐⭐⭐ |
| **New-AzTenantDeployment** | ⭐⭐⭐⭐ |
| **New-AzVM** | ⭐⭐⭐⭐ |
| **New-AzStorageAccount** | ⭐⭐⭐⭐ |
| **New-AzVirtualNetwork** | ⭐⭐⭐⭐ |
| **New-AzRoleAssignment** | ⭐⭐⭐⭐ |
| **New-AzPolicyAssignment** | ⭐⭐⭐ |
| **New-AzMetricAlertRuleV2** | ⭐⭐⭐ |

---

# Regla mnemotécnica

```text
Deployment

↓

¿Dónde despliego?
```

```text
Tenant

↓

Todo Azure
```

```text
Management Group

↓

Varias suscripciones
```

```text
Subscription

↓

Una suscripción
```

```text
Resource Group

↓

Los recursos
```

---

> [!IMPORTANT]
> **Claves para el AZ-104**
>
> - Lo más importante no es memorizar la sintaxis completa de los cmdlets, sino **identificar el cmdlet correcto según el ámbito (scope)**.
> - Las preguntas más frecuentes están relacionadas con los cmdlets **New-Az*Deployment**:
>   - **New-AzResourceGroupDeployment** → Resource Group.
>   - **New-AzSubscriptionDeployment** → Subscription.
>   - **New-AzManagementGroupDeployment** → Management Group.
>   - **New-AzTenantDeployment** → Tenant.
> - En el resto de servicios, normalmente basta con reconocer el patrón:
>   - **Get-** → Consultar.
>   - **New-** → Crear.
>   - **Set-** → Modificar.
>   - **Remove-** → Eliminar.
>   - **Start / Stop / Restart** → Operaciones sobre recursos existentes.
>  

---

# ARM Templates - ¿Qué cmdlet utilizar para desplegar? (AZ-104)

## Escenario

Quieres desplegar una **máquina virtual** utilizando un **ARM Template** desde Azure Cloud Shell.

El comando aparece incompleto:

```powershell
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString

____________

-TemplateUri "https://..."
-adminUsername LocalAdministrator
-adminPassword $adminPassword
```

La pregunta es:

> **¿Qué cmdlet debe ir al principio?**

La respuesta correcta es:

```powershell
New-AzResourceGroupDeployment
```

---

# ¿Por qué?

Porque un **ARM Template no despliega directamente una VM**.

Un ARM Template siempre se despliega en un **scope** (ámbito).

Azure admite cuatro scopes:

```text
Tenant
    │
Management Group
    │
Subscription
    │
Resource Group
```

Cada uno tiene su propio cmdlet.

---

# En esta pregunta

El recurso que quieres desplegar es:

```text
Virtual Machine
```

Las máquinas virtuales son recursos que siempre pertenecen a un:

```text
Resource Group
```

Por tanto el despliegue debe hacerse sobre:

```text
RG1
```

Y el cmdlet correcto es:

```powershell
New-AzResourceGroupDeployment
```

---

# Esquema

```text
Azure

Tenant
│
├── Management Group
│
├── Subscription
│
└── Resource Group (RG1)
      │
      ├── VM
      ├── Storage
      ├── NIC
      └── NSG
```

El ARM Template crea recursos dentro de:

```text
RG1
```

---

# ¿Por qué las demás respuestas son incorrectas?

## New-AzVM

```powershell
New-AzVM
```

Este cmdlet:

- Crea una VM directamente.
- NO utiliza un ARM Template.

Por tanto:

❌ Incorrecto.

---

## New-AzResource

```powershell
New-AzResource
```

Sirve para crear un recurso genérico mediante Azure Resource Manager.

No despliega un ARM Template.

❌ Incorrecto.

---

## New-AzTemplateSpec

Un **Template Spec** es un recurso donde se almacenan ARM Templates.

```text
ARM Template

↓

Template Spec

↓

Repositorio de plantillas
```

No despliega la plantilla.

❌ Incorrecto.

---

# ¿Cómo sería el comando?

```powershell
New-AzResourceGroupDeployment `
   -ResourceGroupName RG1 `
   -TemplateUri https://... `
   -adminUsername LocalAdministrator `
   -adminPassword $adminPassword
```

---

# ¿Cómo saber qué cmdlet usar?

Depende del ámbito donde despliegas.

| Scope | Cmdlet |
|---------|---------|
| Resource Group | **New-AzResourceGroupDeployment** |
| Subscription | **New-AzSubscriptionDeployment** |
| Management Group | **New-AzManagementGroupDeployment** |
| Tenant | **New-AzTenantDeployment** |

---

# Regla visual

```text
¿Dónde va el recurso?
```

## Una VM

```text
VM

↓

Siempre dentro de un Resource Group

↓

New-AzResourceGroupDeployment
```

---

## Un nuevo Resource Group

```text
Resource Group

↓

Pertenece a una Subscription

↓

New-AzSubscriptionDeployment
```

---

## Una Policy para varias suscripciones

```text
Management Group

↓

New-AzManagementGroupDeployment
```

---

## Un nuevo Management Group

```text
Tenant

↓

New-AzTenantDeployment
```

---

# Regla mnemotécnica

```text
VM

↓

Resource Group

↓

New-AzResourceGroupDeployment
```

---

> [!IMPORTANT]
> **Claves para el AZ-104**
>
> - Los **ARM Templates** siempre se despliegan sobre un **scope** (Tenant, Management Group, Subscription o Resource Group).
> - Una **Virtual Machine** siempre pertenece a un **Resource Group**.
> - Para desplegar recursos mediante un ARM Template dentro de un Resource Group se utiliza **New-AzResourceGroupDeployment**.
> - **New-AzVM** crea una VM directamente, pero **no despliega una plantilla ARM**.
> - **New-AzTemplateSpec** administra plantillas almacenadas, pero **no ejecuta el despliegue**.
