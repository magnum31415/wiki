[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice

- [Comparativa RBAC: Virtual Machine Contributor vs Disk Snapshot Contributor](#comparativa-rbac-virtual-machine-contributor-vs-disk-snapshot-contributor)
- [Diferencia: Storage Account Encryption Scope Contributor vs Storage Account Key Operator Service Role (AZ-104)](#diferencia-storage-account-encryption-scope-contributor-vs-storage-account-key-operator-service-role-az-104)


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


| Característica | Storage Account Encryption Scope Contributor | Storage Account Key Operator Service Role | Storage Account Contributor |
|---|---|---|---|
| Objetivo principal | Administrar Encryption Scopes | Administrar Access Keys | Administrar Storage Account |
| Tipo de gestión | Cifrado | Credenciales acceso | Administración general storage |
| Listar Storage Account Keys | ❌ | ✅ | ✅ |
| Regenerar Storage Account Keys | ❌ | ✅ | ✅ |
| Crear Encryption Scopes | ✅ | ❌ | ✅ |
| Modificar Encryption Scopes | ✅ | ❌ | ✅ |
| Eliminar Encryption Scopes | ✅ | ❌ | ✅ |
| Administrar Customer Managed Keys (CMK) asociados | ✅ Parcialmente | ❌ | ✅ Parcialmente |
| Crear Storage Accounts | ❌ | ❌ | ✅ |
| Eliminar Storage Accounts | ❌ | ❌ | ✅ |
| Configurar networking Storage Account | ❌ | ❌ | ✅ |
| Configurar replication (LRS/GRS/ZRS) | ❌ | ❌ | ✅ |
| Administrar containers/blob services | ❌ | ❌ | ✅ Parcialmente |
| Acceso blobs/data | ❌ | ❌ | ❌ |
| Administración completa Storage Account | ❌ | ❌ | ✅ |
| Management Plane | ✅ | ✅ | ✅ |
| Data Plane | ❌ | ❌ | ❌ |
| Gestionar RBAC | ❌ | ❌ | ❌ |
| Crear role assignments | ❌ | ❌ | ❌ |

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

