[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Storage

# Índice

- [Azure Storage](#azure-storage)
- [Modelos de autenticación en Azure Storage](#modelos-de-autenticación-en-azure-storage)
- [Permissions in Azure can be assigned at different scopes within the resource hierarchy](#permissions-in-azure-can-be-assigned-at-different-scopes-within-the-resource-hierarchy)
- [Authentication Methods for Azure Storage](#authentication-methods-for-azure-storage)
- [Comparison](#comparison)
- [Recommended Method](#recommended-method)
- [Summary](#summary)
- [Azure Storage Accounts, Data Lake y Replication (AZ-104)](#azure-storage-accounts-data-lake-y-replication-az-104)
- [Qué es un Azure Storage Account](#qué-es-un-azure-storage-account)
- [Azure Data Lake Storage Gen2 (ADLS Gen2)](#azure-data-lake-storage-gen2-adls-gen2)
- [Hierarchical Namespace](#hierarchical-namespace)
- [Access Tiers](#access-tiers)
- [Tipos de replicación Azure Storage](#tipos-de-replicación-azure-storage)
- [GRS vs ZRS](#grs-vs-zrs)
- [Conceptos importantes AZ-104](#conceptos-importantes-az-104)
- [Trampas típicas examen](#trampas-típicas-examen)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Frases clave AZ-104](#frases-clave-az-104)
- [Stored Access Policies (Azure Storage)](#stored-access-policies-azure-storage)
- [Immutable Blob Storage Policies](#immutable-blob-storage-policies)
---

## Modelos de autenticación en Azure Storage

| Model | Uses Claims | Uses Keys | Example | Related Concept | What it is | Where it is used |
|---|---|---|---|---|---|---|
| Microsoft Entra ID (RBAC) | ✔ | ❌ | A user or application authenticates with Entra ID and accesses a blob using a role like **Storage Blob Data Contributor**. | **Claim** | Information about an identity included in an authentication token. | OAuth / OpenID Connect / Microsoft Entra ID |
| Shared Key (Access Keys) | ❌ | ✔ | An application connects to a storage account using a **connection string with the AccountKey**. | **Access Key (Storage)** | Secret key that provides direct access to the Storage Account. | Key-based authentication |
| SAS Token | ❌ | ✔ | **Shared Access Signature Token** : A temporary URL is generated to allow download of a specific blob for a limited time. | **Access Key (or delegation key)** | Token generated from a storage key or delegation key to grant temporary access. | Delegated access to storage resources |

---

## Permissions in Azure can be assigned at different scopes within the resource hierarchy

| Scope | Description | Example |
|---|---|---|
| Container | Permissions apply only to a specific container inside a storage account. | A user can read blobs only in `images-container`. |
| Storage Account | Permissions apply to all containers and services within a storage account. | A user can manage all blobs in `mystorageaccount`. |
| Resource Group | Permissions apply to all resources inside a resource group. | A user can manage the storage account and VMs inside `rg-app-prod`. |
| Subscription | Permissions apply to every resource in the subscription. | An administrator can manage all resources in the subscription. |

---

# Authentication Methods for Azure Storage

## 1. Microsoft Entra ID (RBAC)

Authentication is performed using identities managed in Microsoft Entra ID.

Users, applications, or managed identities authenticate with Entra ID and receive a token containing claims.

Azure then evaluates RBAC permissions.

### How it works

```text
User / Application
        │
        ▼
Microsoft Entra ID authentication
        │
        ▼
Token with claims
        │
        ▼
Azure RBAC evaluates permissions
        │
        ▼
Access to Storage resource
```

### Characteristics

| Feature | Description |
|---|---|
| Identity-based | Yes |
| Uses tokens | Yes |
| Uses RBAC roles | Yes |
| Supports least privilege | Yes |
| Supports audit and governance | Yes |

Example roles:

- Storage Blob Data Reader
- Storage Blob Data Contributor
- Storage Blob Data Owner

---

## 2. Shared Key (Access Keys)

Authentication is performed using a secret key associated with the storage account.

Each storage account has two keys:

```text
Key1
Key2
```

### How it works

```text
Application
      │
      ▼
Uses Storage Account Access Key
      │
      ▼
Azure Storage authenticates request
      │
      ▼
Access granted
```

### Characteristics

| Feature | Description |
|---|---|
| Identity-based | No |
| Uses tokens | No |
| Uses secret keys | Yes |
| Access scope | Full storage account |
| Security risk | Higher if keys are exposed |

---

## 3. SAS Token (Shared Access Signature)

A SAS token provides delegated and time-limited access to specific storage resources.

It is generated using:

- Access Key
- User Delegation Key

### How it works

```text
Storage Account
      │
Generate SAS token
      │
      ▼
Client receives token
      │
      ▼
Temporary access to specific resource
```

### Example

```text
https://storage.blob.core.windows.net/container/file.txt?sv=...&sig=...
```

### Characteristics

| Feature | Description |
|---|---|
| Identity-based | Optional |
| Time limited | Yes |
| Granular permissions | Yes |
| Scope | Specific container/blob/file |
| Common use case | Temporary sharing |

---

# Comparison

| Method | Identity-based | Security Level | Typical Use |
|---|---|---|---|
| Microsoft Entra ID (RBAC) | Yes | Highest | Applications and services |
| Shared Key | No | Lower | Legacy integrations |
| SAS Token | Partial | Medium | Temporary access |

---

# Recommended Method

The recommended method is:

```text
Microsoft Entra ID + RBAC
```

Reasons:

- least privilege
- governance
- auditing
- avoids secret exposure

---

# Summary

| Method | Recommended |
|---|---|
| Microsoft Entra ID (RBAC) | Yes |
| SAS Token | Temporary delegated access |
| Shared Key | Avoid when possible |

---

# Azure Storage Accounts, Data Lake y Replication (AZ-104)

## Qué es un Azure Storage Account

Es el servicio principal de almacenamiento en Azure.

---

## Qué puede almacenar

| Tipo | Ejemplo |
|---|---|
| Blob Storage | Archivos |
| Azure Files | SMB shares |
| Queues | Mensajes |
| Tables | NoSQL |

---

# Azure Data Lake Storage Gen2 (ADLS Gen2)

## Qué es

ADLS Gen2 es una extensión de Azure Blob Storage optimizada para Big Data y Analytics.

---

## Uso típico

| Caso | Ejemplo |
|---|---|
| Big Data | Hadoop |
| Analytics | Synapse |
| Data Engineering | ETL pipelines |
| Machine Learning | Data Science |

---

# Hierarchical Namespace

## Qué es

Permite organizar blobs usando:

```text
carpetas y directorios reales
```

---

## Importante

Azure Data Lake Storage Gen2 requiere:

```text
Hierarchical Namespace
```

---

## Sin Hierarchical Namespace

```text
Flat namespace
```

---

## Con Hierarchical Namespace

```text
/storage/folder1/folder2/file.csv
```

---

# Access Tiers

## Hot Tier

Optimizado para:

✅ acceso frecuente

| Característica | Hot |
|---|---|
| Storage cost | Alto |
| Access cost | Bajo |
| Access frequency | Alta |

---

## Cool Tier

Optimizado para:

✅ acceso poco frecuente

| Característica | Cool |
|---|---|
| Storage cost | Bajo |
| Access cost | Más alto |
| Access frequency | Baja |

---

## Archive Tier

Optimizado para:

✅ retención largo plazo

| Característica | Archive |
|---|---|
| Storage cost | Muy bajo |
| Retrieval latency | Alta |
| Immediate access | ❌ |

---

# Tipos de replicación Azure Storage

| Tipo | Replicación | Región secundaria |
|---|---|---|
| LRS | Datacenter local | ❌ |
| ZRS | Availability Zones | ❌ |
| GRS | Otra región | ✅ |
| RA-GRS | Otra región + lectura | ✅ |

---

# LRS

```text
Locally Redundant Storage
```

Replica datos dentro de un único datacenter.

---

# ZRS

```text
Zone-Redundant Storage
```

Replica datos entre Availability Zones dentro de la misma región.

---

# GRS

```text
Geo-Redundant Storage
```

Replica datos automáticamente a otra región Azure.

---

# RA-GRS

```text
Read-Access Geo-Redundant Storage
```

Igual que GRS pero permitiendo lectura desde la región secundaria.

---

# GRS vs ZRS

| Feature | GRS | ZRS |
|---|---|---|
| Replicación cross-region | ✅ | ❌ |
| Availability Zones | ❌ | ✅ |
| Disaster Recovery | ✅ | Parcial |
| Alta disponibilidad regional | ❌ | ✅ |

---

# Conceptos importantes AZ-104

| Concepto | Importancia |
|---|---|
| GRS vs ZRS | Muy alta |
| Cool vs Hot Tier | Muy alta |
| Hierarchical Namespace | Muy alta |
| ADLS Gen2 | Muy alta |

---

# Trampas típicas examen

## Trampa 1

Pensar que:

```text
ZRS replica a otra región
```

❌ Incorrecto.

ZRS replica solo dentro de la misma región.

---

## Trampa 2

Confundir:

```text
Cool Tier
```

con:

```text
Archive Tier
```

---

## Diferencia rápida

| Tier | Uso |
|---|---|
| Hot | Acceso frecuente |
| Cool | Acceso poco frecuente |
| Archive | Retención largo plazo |

---

## Trampa 3

Pensar que:

```text
Azure Data Lake es un servicio separado
```

❌ Incorrecto.

ADLS Gen2 realmente es:

```text
Blob Storage + Hierarchical Namespace
```

---

# Tabla resumen examen

| Necesidad | Solución |
|---|---|
| Data Lake | Hierarchical Namespace |
| Datos poco accedidos | Cool Tier |
| Replicación otra región | GRS |
| Alta disponibilidad misma región | ZRS |

---

# Frases clave AZ-104

```text
Azure Data Lake Storage Gen2 requires hierarchical namespace.
```

```text
Geo-redundant storage replicates data to another region.
```

```text
Cool tier reduces costs for infrequently accessed data.
```
---

# Stored Access Policies (Azure Storage)

## Qué son las Stored Access Policies

Una:

```text
Stored Access Policy
```

es una política almacenada dentro de Azure Storage que define:

- permisos
- expiración
- restricciones acceso

para SAS Tokens.

---

## Para qué sirven

Permiten:

✅ Centralizar permisos SAS  
✅ Reutilizar configuraciones acceso  
✅ Revocar SAS indirectamente  
✅ Modificar expiración/permisos sin regenerar SAS  

---

## Relación con SAS Tokens

Un SAS Token puede:

| Tipo | Descripción |
|---|---|
| Ad-hoc SAS | Lleva permisos embebidos directamente |
| SAS con Stored Access Policy | Referencia una policy almacenada |

---

## Flujo conceptual

```text
Stored Access Policy
        ↓
Define permisos/expiración
        ↓
SAS Token referencia policy
        ↓
Cliente usa SAS
```

---

## Ventaja importante

Si el SAS usa una Stored Access Policy:

```text
puedes revocar el acceso modificando o eliminando la policy
```

sin regenerar todos los SAS Tokens.

---

## Recursos soportados y límites

| Recurso | Soporta Stored Access Policies | Máximo policies | Ejemplo |
|---|---|---|---|
| Blob Container | ✅ | 5 | `images-container` |
| File Share | ✅ | 5 | `share-finance` |
| Queue | ✅ | 5 | `orders-queue` |
| Table | ✅ | 5 | `employees-table` |

---

## Ejemplo Blob Container

### Policy

| Campo | Valor |
|---|---|
| Nombre | `policy-read` |
| Permisos | Read |
| Expira | 7 días |

---

### SAS asociado

```text
https://mystorage.blob.core.windows.net/images/photo.jpg?...&si=policy-read
```

---

## Ejemplo File Share

### Caso típico

Compartir temporalmente:

```text
\\storage.file.core.windows.net\share-finance
```

con permisos controlados.

---

## Ejemplo Queue

### Caso típico

Aplicación externa puede:

✅ leer mensajes  
❌ borrar mensajes  

durante un tiempo limitado.

---

## Ejemplo Table

### Caso típico

Aplicación partner puede:

✅ consultar entidades  
❌ modificar datos  

usando SAS controlado.

---

## Límite importante examen

Azure Storage soporta:

```text
máximo 5 Stored Access Policies por recurso
```

---

## Importante

El límite es:

```text
por container/share/queue/table individual
```

NO por Storage Account.

---

## Diferencia importante

| Concepto | Qué es |
|---|---|
| SAS Token | Token acceso temporal |
| Stored Access Policy | Política reutilizable para SAS |

---

## Trampa típica AZ-104

Muchos candidatos creen:

```text
puedes crear policies ilimitadas
```

❌ Incorrecto.

---

## Otra trampa típica

Pensar que:

```text
Stored Access Policies solo existen para blobs
```

❌ Incorrecto.

También existen para:

- File Shares
- Queues
- Tables

---

## Regla rápida AZ-104

```text
Azure Storage supports up to five stored access policies per resource.
```

---

## Frases clave AZ-104

```text
Stored access policies are used with SAS tokens.
```

```text
Stored access policies centralize SAS permissions and expiration settings.
```

```text
Blob containers, file shares, queues, and tables support up to five stored access policies each.
```
---

# Immutable Blob Storage Policies 


- [Immutable Blob Storage Policies (AZ-104)](#immutable-blob-storage-policies-az-104)
- [Qué es Immutable Blob Storage](#qué-es-immutable-blob-storage)
- [Qué significa immutable](#qué-significa-immutable)
- [Para qué sirve](#para-qué-sirve)
- [Tipos principales de políticas](#tipos-principales-de-políticas)
- [Time-Based Retention Policy](#time-based-retention-policy)
- [Legal Hold Policy](#legal-hold-policy)
- [Qué protege realmente](#qué-protege-realmente)
- [WORM Storage](#worm-storage)
- [Dónde se configura](#dónde-se-configura)
- [Importante sobre modificación y borrado](#importante-sobre-modificación-y-borrado)
- [Estados de una policy](#estados-de-una-policy)
- [Diferencia importante examen](#diferencia-importante-examen)
- [Casos típicos de uso](#casos-típicos-de-uso)
- [Trampas típicas AZ-104](#trampas-típicas-az-104)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

# Qué es Immutable Blob Storage

Azure Immutable Blob Storage permite almacenar datos en modo:

```text
WORM
```

---

# Qué significa immutable

```text
Immutable
```

significa:

❌ no puede modificarse  
❌ no puede borrarse  

durante un período definido.

---

# WORM Storage

WORM significa:

```text
Write Once Read Many
```

↓

Los datos:

✅ pueden escribirse una vez  
✅ pueden leerse muchas veces  
❌ no pueden modificarse  

---

# Para qué sirve

Se usa para:

- cumplimiento legal
- auditoría
- regulación financiera
- protección ransomware
- evidencias digitales
- logs inmutables

---

# Tipos principales de políticas

| Política | Función |
|---|---|
| Time-Based Retention Policy | Bloqueo durante tiempo definido |
| Legal Hold | Bloqueo indefinido legal |

---

# Time-Based Retention Policy

## Qué hace

Impide:

- borrar blobs
- modificar blobs

durante un tiempo específico.

---

## Ejemplo

```text
Retention = 7 years
```

↓

Durante 7 años:

❌ no borrar  
❌ no modificar  

---

# Legal Hold Policy

## Qué hace

Bloquea blobs:

```text
hasta eliminar explícitamente el legal hold
```

---

## Uso típico

- litigios
- investigaciones
- auditorías legales

---

# Qué protege realmente

Immutable Blob Storage protege:

| Acción | Permitido |
|---|---|
| Leer blob | ✅ |
| Crear blob | ✅ |
| Modificar blob | ❌ |
| Eliminar blob | ❌ |

---

# Dónde se configura

Normalmente sobre:

| Nivel | Soportado |
|---|---|
| Blob Container | ✅ |
| Version level | ✅ |

---

# Importante sobre modificación y borrado

Durante la retención:

```text
ni siquiera administradores pueden borrar/modificar blobs protegidos
```

---

# Estados de una policy

| Estado | Significado |
|---|---|
| Unlocked | Configurable |
| Locked | Inmutable definitiva |

---

# Importante examen

Una vez:

```text
Locked
```

↓

la política:

❌ no puede reducirse  
❌ no puede eliminarse fácilmente  

---

# Diferencia importante examen

| Feature | Objetivo |
|---|---|
| Soft Delete | Recuperación accidental |
| Versioning | Historial versiones |
| Immutable Policy | Bloqueo WORM |

---

# Casos típicos de uso

| Escenario | Uso |
|---|---|
| Logs regulatorios | Immutable |
| Evidencias auditoría | Immutable |
| Protección ransomware | Immutable |
| Backup normal | Soft Delete |

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
Soft Delete = Immutable
```

❌ Incorrecto.

---

## Trampa 2

Pensar que administradores pueden borrar blobs protegidos.

❌ Incorrecto.

---

## Trampa 3

Confundir:

```text
Versioning
```

con:

```text
WORM retention
```

---

# Tabla resumen examen

| Feature | Permite modificar | Permite borrar |
|---|---|---|
| Soft Delete | ✅ | Recuperable |
| Versioning | ✅ | ✅ |
| Immutable Policy | ❌ | ❌ |

---

# Reglas rápidas AZ-104

```text
Immutable Blob Storage provides WORM protection.
```

```text
Time-based retention policies prevent deletion and modification.
```

```text
Legal holds preserve blobs indefinitely.
```

---

# Frases clave AZ-104

```text
Immutable storage protects blobs from modification and deletion.
```

```text
WORM means Write Once Read Many.
```

```text
Immutable policies are commonly used for compliance and regulatory requirements.
```
