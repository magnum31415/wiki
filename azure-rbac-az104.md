[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice
- [Azure Files - Storage File Data Roles (AZ-104)](#azure-files---storage-file-data-roles-az-104)
- [Comparativa RBAC: Virtual Machine Contributor vs Disk Snapshot Contributor](#comparativa-rbac-virtual-machine-contributor-vs-disk-snapshot-contributor)
- [Diferencia: Storage Account Encryption Scope Contributor vs Storage Account Key Operator Service Role (AZ-104)](#diferencia-storage-account-encryption-scope-contributor-vs-storage-account-key-operator-service-role-az-104)
- [RBAC - Rol Contributor (AZ-104)](#rbac---rol-contributor-az-104)
- [Rol para crear una subnet en VNet1](#rol-para-crear-una-subnet-en-vnet1)
- [Rol Attribute Definition/Assignment Administrator](#rol-attribute-definitionassignment-administrator)
- [Azure Custom Roles - Actions vs DataActions](#azure-custom-roles---actions-vs-dataactions)

---
# Azure Files - Storage File Data Roles (AZ-104)

| Rol | Plane | ¿A qué accede? | Lectura archivos | Escritura archivos | Eliminar archivos | Cambiar permisos NTFS | Control total | Uso típico |
|---|---|---|---|---|---|---|---|---|
| Storage File Data SMB Share Reader | Data Plane | Datos dentro del File Share | ✅ | ❌ | ❌ | ❌ | ❌ | Usuarios solo lectura |
| Storage File Data SMB Share Contributor | Data Plane | Datos dentro del File Share | ✅ | ✅ | ✅ | ❌ | ❌ | Usuarios lectura/escritura |
| Storage File Data SMB Share Elevated Contributor | Data Plane | Datos + permisos SMB/NTFS avanzados | ✅ | ✅ | ✅ | ✅ Parcialmente | ❌ | Administración avanzada SMB |
| Storage File Data SMB Share Owner | Data Plane | Control completo sobre datos SMB | ✅ | ✅ | ✅ | ✅ | ✅ | Administrador completo del share |
| Reader | Control Plane | Recurso Azure Storage/File Share | ❌ | ❌ | ❌ | ❌ | ❌ | Ver configuración recursos |
| Contributor | Control Plane | Administración recursos Azure | ❌ | ❌ | ❌ | ❌ | ❌ | Crear/modificar File Shares |
| Owner | Control Plane | Administración + RBAC | ❌ | ❌ | ❌ | ❌ | ❌ | Administración completa Azure |

---

# Diferencia importante

| Plane | Qué administra |
|---|---|
| Control Plane | Recursos Azure |
| Data Plane | Datos dentro del recurso |

---

# Ejemplo visual

```text
Storage Account
 └── File Share
       └── archivo.docx


---
# Comparativa RBAC: Virtual Machine Contributor vs Disk Snapshot Contributor

| Acción / Permiso | Virtual Machine Contributor | Disk Snapshot Contributor |
|---|---|---|
| Crear máquinas virtuales | ✅ | ❌ |
| Eliminar máquinas virtuales | ✅ | ❌ |
| Iniciar / detener VM | ✅ | ❌ |
| Reiniciar VM | ✅ | ❌ |
| Redimensionar VM | ✅ | ❌ |
| Administrar configuración VM | ✅ | ❌ |
| Attach / Detach disks | ✅ | ❌ |
| Leer configuración de discos asociados | ✅ | ❌ |
| Crear managed disks | ✅ Parcialmente (en contexto VM) | ❌ |
| Eliminar managed disks | ✅ Parcialmente (en contexto VM) | ❌ |
| Administrar NICs relacionadas con la VM | ✅ | ❌ |
| Crear snapshots | ❌ | ✅ |
| Eliminar snapshots | ❌ | ✅ |
| Leer snapshots | ❌ | ✅ |
| Restaurar snapshots | ❌ | ✅ |
| Exportar snapshots | ❌ | ✅ |
| Administrar recursos `Microsoft.Compute/snapshots` | ❌ | ✅ |
| Administrar recursos `Microsoft.Compute/virtualMachines` | ✅ | ❌ |
| Login RDP/SSH | ❌ | ❌ |
| Gestionar RBAC | ❌ | ❌ |
| Crear role assignments | ❌ | ❌ |

---

## Recursos Azure afectados

| Rol | Recursos principales |
|---|---|
| Virtual Machine Contributor | `Microsoft.Compute/virtualMachines` |
| Disk Snapshot Contributor | `Microsoft.Compute/snapshots` |

---

## Diferencia clave para AZ-104

| Concepto | Explicación |
|---|---|
| VM Contributor | Administra VMs y discos asociados a la VM |
| Disk Snapshot Contributor | Administra snapshots como recursos independientes |
| Snapshot | Tiene RBAC separado de VM y Managed Disk |

---

## Trampa típica de examen

Muchos candidatos creen:

```text
Virtual Machine Contributor = control total sobre discos y snapshots
```

❌ Incorrecto.

Los snapshots requieren permisos específicos.

---

## Regla rápida para memorizar

| Necesidad | Rol mínimo recomendado |
|---|---|
| Administrar VMs | Virtual Machine Contributor |
| Administrar snapshots | Disk Snapshot Contributor |
| Login VM | Virtual Machine User Login |
| Login administrador VM | Virtual Machine Administrator Login |

---

## Frase clave AZ-104

```text
Snapshots are independent Azure resources and require separate RBAC permissions.
```

# Diferencia: Storage Account Encryption Scope Contributor vs Storage Account Key Operator Service Role (AZ-104)

| Característica | Storage Account Encryption Scope Contributor | Storage Account Key Operator Service Role | Storage Account Contributor | Reader and Data Access |
|---|---|---|---|---|
| Objetivo principal | Administrar Encryption Scopes | Administrar Access Keys | Administrar Storage Account | Lectura recursos + acceso datos |
| Tipo de gestión | Cifrado | Credenciales acceso | Administración general storage | Lectura + acceso datos |
| Leer recursos (Management Plane) | ✅ Parcialmente | ✅ Parcialmente | ✅ | ✅ |
| Acceso a datos del storage | ❌ | ❌ | ❌ | ✅ |
| Leer Storage Account Keys | ❌ | ✅ | ✅ | ✅ |
| Regenerar Storage Account Keys | ❌ | ✅ | ✅ | ❌ |
| Crear Encryption Scopes | ✅ | ❌ | ✅ |
| Modificar Encryption Scopes | ✅ | ❌ | ✅ | ❌ |
| Eliminar Encryption Scopes | ✅ | ❌ | ✅ | ❌ |
| Administrar Customer Managed Keys (CMK) asociados | ✅ Parcialmente | ❌ | ✅ Parcialmente | ❌ |
| Crear Storage Accounts | ❌ | ❌ | ✅ | ❌ |
| Eliminar Storage Accounts | ❌ | ❌ | ✅ | ❌ |
| Configurar networking Storage Account | ❌ | ❌ | ✅ | ❌ |
| Configurar replication (LRS/GRS/ZRS) | ❌ | ❌ | ✅ | ❌ |
| Administrar containers/blob services | ❌ | ❌ | ✅ Parcialmente | ❌ |
| Administración completa Storage Account | ❌ | ❌ | ✅ | ❌ |
| Management Plane | ✅ | ✅ | ✅ | ✅ Lectura |
| Data Plane | ❌ | ❌ | ❌ | ✅ |
| Gestionar RBAC | ❌ | ❌ | ❌ | ❌ |
| Crear role assignments | ❌ | ❌ | ❌ | ❌ |

---

## Concepto clave

### Storage Account Encryption Scope Contributor

Administra:

```text
cómo se cifran los datos
```

NO:

```text
quién accede al Storage Account
```

---

### Storage Account Key Operator Service Role

Administra:

```text
las claves de acceso del Storage Account
```

NO:

```text
el cifrado de los datos
```

---

## Qué son Access Keys

Son las claves:

```text
key1
key2
```

del Storage Account.

Permiten:

- Connection Strings
- Autenticación total
- SAS generation
- Acceso completo al storage

---

## Qué son Encryption Scopes

Permiten definir:

- Distintas claves de cifrado
- Diferentes políticas de cifrado
- Customer Managed Keys (CMK)

para blobs específicos.

---

## Diferencia importante

| Concepto | Relacionado con |
|---|---|
| Access Keys | Autenticación |
| Encryption Scopes | Cifrado |

---

## Trampa típica AZ-104

Muchos candidatos piensan:

```text
Encryption = keys de acceso
```

❌ Incorrecto.

---

## Ejemplo visual

### Access Keys

```text
Usuario/App
 ↓
key1 / key2
 ↓
Acceso Storage Account
```

---

### Encryption Scope

```text
Blob
 ↓
Encryption Scope
 ↓
CMK / Encryption policy
```

---

## Cuándo usar cada rol

### Storage Account Key Operator Service Role

Cuando el usuario necesita:

✅ Ver keys  
✅ Rotar keys  
✅ Regenerar keys  

---

### Storage Account Encryption Scope Contributor

Cuando el usuario necesita:

✅ Administrar cifrado blobs  
✅ Configurar encryption scopes  
✅ Gestionar políticas cifrado  

---

## Regla rápida examen

| Necesidad | Rol correcto |
|---|---|
| Listar/regenerar keys | Storage Account Key Operator Service Role |
| Administrar cifrado blobs | Storage Account Encryption Scope Contributor |

---

## Frases clave AZ-104

```text
Access keys are authentication credentials.
```

```text
Encryption scopes control data encryption, not access permissions.
```

```text
Storage Account Key Operator Service Role manages storage account keys.
```

# RBAC - Rol Contributor (AZ-104)

| Acción / Permiso | Contributor |
|---|---|
| Crear recursos Azure | ✅ |
| Modificar recursos Azure | ✅ |
| Eliminar recursos Azure | ✅ |
| Crear máquinas virtuales | ✅ |
| Redimensionar máquinas virtuales | ✅ |
| Reiniciar VMs | ✅ |
| Iniciar / detener VMs | ✅ |
| Crear discos | ✅ |
| Modificar discos | ✅ |
| Crear redes virtuales | ✅ |
| Crear Storage Accounts | ✅ |
| Crear Resource Groups | ✅ |
| Administrar App Services | ✅ |
| Administrar bases de datos | ✅ |
| Administrar Networking | ✅ |
| Administrar NSG | ✅ |
| Administrar Load Balancers | ✅ |
| Administrar Key Vault (management plane) | ✅ |
| Leer recursos | ✅ |
| Aplicar tags | ✅ |
| Desplegar ARM/Bicep/Terraform | ✅ |
| Administrar recursos dentro del scope asignado | ✅ |

---

## Qué NO permite

| Acción / Permiso | Contributor |
|---|---|
| Gestionar RBAC | ❌ |
| Crear role assignments | ❌ |
| Asignar roles IAM | ❌ |
| Elevar privilegios | ❌ |
| Acceso automático al Data Plane | ❌ |
| Leer secretos Key Vault | ❌ |
| Leer blobs Storage | ❌ |
| Acceso RDP/SSH automático | ❌ |

---

## Concepto importante

Contributor:

```text
puede administrar recursos
```

pero NO:

```text
administrar permisos
```

---

## Diferencia importante

| Rol | Puede administrar recursos | Puede asignar roles |
|---|---|---|
| Reader | ❌ | ❌ |
| Contributor | ✅ | ❌ |
| Owner | ✅ | ✅ |

---

## Ejemplo de la pregunta

User1 tiene:

```text
Contributor → Corp-MG2
```

Corp-MG2 contiene:

```text
SubC
 ↓
RG-Compute
 ↓
ProdVM01
```

Por herencia RBAC:

✅ User1 puede administrar ProdVM01.

---

## Por qué puede resize la VM

Redimensionar una VM es una operación de:

```text
Microsoft.Compute/virtualMachines/write
```

y Contributor permite operaciones:

```text
write
```

sobre recursos del scope.

---

## Scope inheritance importante

RBAC hereda permisos:

```text
Management Group
 ↓
Subscription
 ↓
Resource Group
 ↓
Resource
```

---

## En este caso

| Scope asignado | Recursos afectados |
|---|---|
| Corp-MG2 | Todo dentro de MG2 |
| SubC | Toda la subscription |
| RG-Compute | Todos los recursos RG |
| ProdVM01 | Solo esa VM |

---

## Trampa típica AZ-104

Muchos candidatos creen:

```text
Contributor = acceso total
```

❌ Incorrecto.

NO puede:

- asignar roles
- modificar IAM
- gestionar RBAC

---

## Regla rápida examen

```text
Contributor can manage resources but cannot manage access.
```

---

## Frases clave AZ-104

```text
Contributor allows full resource management.
```

```text
Contributor does not allow RBAC role assignments.
```

```text
RBAC permissions are inherited from parent scopes.
```

# Rol para crear una subnet en VNet1

| Usuario | Rol asignado | ¿Puede crear una subnet en VNet1? | Motivo |
|---|---|---|---|
| User1 | Owner | ✅ Sí | Tiene permisos completos sobre los recursos, incluida la gestión de redes y subnets. |
| User2 | Security Admin | ❌ No | Es un rol de Microsoft Entra/security; no permite administrar recursos de red Azure como VNets o subnets. |
| User3 | Network Contributor | ✅ Sí | Puede administrar recursos de red, incluyendo virtual networks y subnets. |


| Acción | Permiso/Rol necesario |
|---|---|
| Crear una subnet dentro de una VNet | Owner |
| Crear una subnet dentro de una VNet | Contributor |
| Crear una subnet dentro de una VNet | Network Contributor |


# Rol Attribute Definition/Assignment Administrator

| Rol | Qué puede hacer | Ejemplo |
|---|---|---|
| Global Administrator | Administración completa del tenant Microsoft Entra ID | Crear usuarios, resetear passwords, administrar licencias, configurar Conditional Access |
| Attribute Definition Administrator | Crear y administrar Custom Security Attributes | Crear atributo `ClearanceLevel` o `DataSensitivity` |
| Attribute Assignment Administrator | Asignar valores de Custom Security Attributes a identidades soportadas | Asignar `ClearanceLevel=High` a UserA |

---

## Diferencia importante

| Rol | Gestiona definición | Gestiona asignación |
|---|---|---|
| Global Administrator | Parcialmente/No automáticamente | Parcialmente/No automáticamente |
| Attribute Definition Administrator | ✅ | ❌ |
| Attribute Assignment Administrator | ❌ | ✅ |

---

# #Concepto clave AZ-104

| Acción | Rol correcto |
|---|---|
| Crear atributo personalizado | Attribute Definition Administrator |
| Asignar atributo a un usuario | Attribute Assignment Administrator |
| Administración global tenant | Global Administrator |

---

## Importante

```text
Global Administrator NO recibe automáticamente permisos completos sobre Custom Security Attributes.
```

Microsoft utiliza:

```text
roles especializados
```

para separar:

- definición atributos
- asignación atributos
- administración general


---


# Azure Custom Roles - Actions vs DataActions

---
## azure Custom Roles

| Campo | Para qué sirve | Tipo de permiso | Ejemplo | Explicación |
|---|---|---|---|---|
| `actions` | Permisos del management plane | Administración recursos Azure | "Microsoft.Compute/virtualMachines/start/action" | Permite arrancar VMs |
| `notActions` | Excluir permisos dentro de actions | Exclusión management plane | "Microsoft.Compute/virtualMachines/delete" | Permite administrar VMs excepto borrarlas |
| `dataActions` | Permisos del data plane | Acceso a datos/uso recurso | "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read" | Leer blobs |
| `notDataActions` | Excluir permisos dentro de dataActions | Exclusión data plane | "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete" | Leer blobs pero no borrarlos |
| `assignableScopes` | Define dónde puede asignarse el rol | Scope RBAC | "/subscriptions/xxxx" | El rol puede asignarse en esa subscripción |
| `roleType` | Tipo de rol | Metadato | "CustomRole" | Indica que el rol es personalizado |

---



# Concepto clave

En Azure RBAC, un rol personalizado puede definir permisos en dos planos diferentes:

- Management plane
- Data plane


| Permiso RBAC | Tipo | Va en | Qué hace realmente | Por qué |
|---|---|---|---|---|
| `Microsoft.Compute/virtualMachines/start/action` | Management Plane | `actions` | Arranca una VM Azure | Administra el recurso Azure |
| `Microsoft.Compute/virtualMachines/restart/action` | Management Plane | `actions` | Reinicia una VM | Administración infraestructura |
| `Microsoft.Compute/virtualMachines/deallocate/action` | Management Plane | `actions` | Detiene/libera VM | Administración recurso Compute |
| `Microsoft.Compute/virtualMachines/delete` | Management Plane | `actions` | Borra una VM | Gestión del recurso |
| `Microsoft.Compute/virtualMachines/login/action` | Data Plane | `dataActions` | Permite login RBAC en VM | Uso/acceso operativo al recurso |
| `Microsoft.Compute/virtualMachines/loginAsAdmin/action` | Data Plane | `dataActions` | Permite login administrador RBAC | Acceso operativo privilegiado |

---

# Regla mental correcta

| Pregunta | Resultado |
|---|---|
| ¿Estoy administrando el recurso Azure? | `actions` |
| ¿Estoy usando/accediendo al recurso? | `dataActions` |

---

# Ejemplo práctico

## actions

```text
Start VM
Stop VM
Resize VM
Delete VM
```
## dataActions

````text
RDP login
SSH login
Access blob data
Read Key Vault secret
````




---

# 1. actions

## Qué controla

Permisos sobre:

```text
Management Plane
```

↓

Administración de recursos Azure.

---

## Ejemplos típicos

| Acción | Ejemplo |
|---|---|
| Crear VM | `Microsoft.Compute/virtualMachines/write` |
| Borrar VM | `Microsoft.Compute/virtualMachines/delete` |
| Leer recursos | `*/read` |

---

## Ejemplo real

```json
"actions": [
  "Microsoft.Compute/virtualMachines/*"
]
```

↓

Permite administrar VMs.

---

# 2. notActions

## Qué controla

Permite excluir permisos definidos en:

```text
actions
```

---

## Ejemplo

```json
"actions": [
  "Microsoft.Compute/virtualMachines/*"
],
"notActions": [
  "Microsoft.Compute/virtualMachines/delete"
]
```

↓

Puede administrar VMs:

✅ crear  
✅ modificar  
❌ borrar  

---

# 3. dataActions

## Qué controla

Permisos sobre:

```text
Data Plane
```

↓

Acceso al contenido o uso operativo del recurso.

---

## Ejemplos típicos

| Recurso | DataAction |
|---|---|
| Blob Storage | Leer blobs |
| Key Vault | Leer secretos |
| VM Login | Iniciar sesión |

---

## Ejemplo VM Login

```json
"dataActions": [
  "Microsoft.Compute/virtualMachines/login/action"
]
```

↓

Permite login RBAC en VM.

---

# 4. notDataActions

## Qué controla

Excluye permisos definidos en:

```text
dataActions
```

---

## Ejemplo

```json
"dataActions": [
  "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/*"
],
"notDataActions": [
  "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete"
]
```

↓

Puede trabajar con blobs:

✅ leer  
✅ escribir  
❌ borrar  

---

# 5. assignableScopes

## Qué controla

Define:

```text
dónde puede asignarse el rol
```

---

## Ejemplo

```json
"assignableScopes": [
  "/subscriptions/xxxx"
]
```

↓

El rol puede asignarse:

✅ en esa subscripción  
✅ debajo de ella (RG/resources)  

---

# Importante examen

El rol NO puede asignarse fuera de esos scopes.

---

# 6. roleType

## Qué controla

Indica el tipo de rol.

---

## Valores típicos

| Valor | Significado |
|---|---|
| BuiltInRole | Rol integrado Azure |
| CustomRole | Rol personalizado |

---

## Ejemplo

```json
"roleType": "CustomRole"
```

---

# Diferencia importante examen

| Campo | Management Plane | Data Plane |
|---|---|---|
| actions | ✅ | ❌ |
| notActions | ✅ | ❌ |
| dataActions | ❌ | ✅ |
| notDataActions | ❌ | ✅ |

---

# Regla rápida AZ-104

```text
Actions manage Azure resources.
```

```text
DataActions access the resource data itself.
```

```text
AssignableScopes defines where the role can be assigned.
```
