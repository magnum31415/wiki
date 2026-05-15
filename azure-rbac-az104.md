[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice

- [Comparativa RBAC: Virtual Machine Contributor vs Disk Snapshot Contributor](#comparativa-rbac-virtual-machine-contributor-vs-disk-snapshot-contributor)
- [Diferencia: Storage Account Encryption Scope Contributor vs Storage Account Key Operator Service Role (AZ-104)](#diferencia-storage-account-encryption-scope-contributor-vs-storage-account-key-operator-service-role-az-104)
- [RBAC - Rol Contributor (AZ-104)](#rbac---rol-contributor-az-104)
- [Rol para crear una subnet en VNet1](#rol-para-crear-una-subnet-en-vnet1)
- [Rol Attribute Definition/Assignment Administrator](#rol-attribute-definitionassignment-administrator)
- [Azure Custom Roles - Actions vs DataActions](#azure-custom-roles---actions-vs-dataactions)

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

# Concepto clave

En Azure RBAC, un rol personalizado puede definir permisos en dos planos diferentes:

- Management plane
- Data plane

---

# 1. Actions

`actions` define permisos de administración sobre recursos Azure.

Sirve para operaciones como:

- crear recursos
- leer configuración
- modificar recursos
- eliminar recursos
- desplegar ARM templates

---

# Ejemplos de Actions

```json
"Microsoft.Compute/virtualMachines/*"
```

Permite administrar máquinas virtuales como recurso Azure.

Ejemplos:

- crear VM
- modificar VM
- arrancar/parar VM
- cambiar configuración

---

# 2. DataActions

`dataActions` define permisos sobre el plano de datos u operaciones internas del recurso.

Sirve para acceder o interactuar con el contenido/funcionalidad del recurso.

---

# Ejemplo importante VM Login

Para permitir inicio de sesión en una VM mediante Azure RBAC se usan permisos como:

```text
Microsoft.Compute/virtualMachines/login/action
```

o:

```text
Microsoft.Compute/virtualMachines/loginAsAdmin/action
```

Estos permisos van en:

```text
dataActions
```

---

# Diferencia clave

| Sección | Qué controla | Ejemplo |
|---|---|---|
| actions | Administración del recurso | Crear/modificar una VM |
| dataActions | Acceso operativo/datos del recurso | Iniciar sesión en una VM |
| notActions | Excluir permisos de actions | Permitir todo excepto borrar |
| notDataActions | Excluir permisos de dataActions | Permitir dataActions excepto una acción concreta |

---

# Por qué la respuesta es DataActions

La pregunta pide:

```text
users can sign in to virtual machines
```

Eso no es simplemente administrar el recurso VM.

Es una acción operativa sobre la VM.

Por eso el permiso debe añadirse en:

```text
dataActions
```

---

# Regla rápida examen

```text
If the permission is about managing Azure resources, use actions.
```

```text
If the permission is about accessing data or signing in/using the resource, use dataActions.
```

---

# Ejemplo mental

| Necesidad | Sección |
|---|---|
| Crear una VM | actions |
| Borrar una VM | actions |
| Leer propiedades de una VM | actions |
| Iniciar sesión en una VM | dataActions |
| Leer secretos de Key Vault | dataActions |
| Leer blobs | dataActions |

---

# Frases clave AZ-104

```text
Actions are for management plane permissions.
```

```text
DataActions are for data plane permissions.
```

```text
VM login permissions must be added to dataActions.
```
