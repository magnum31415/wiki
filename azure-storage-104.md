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
- [Azure Storage Policies](#azure-storage-policies)
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

# Azure Storage Policies 

---

# Tabla resumen de Policies

| Tipo de Policy | Objetivo | Recursos compatibles | Máximo |
|---|---|---|---|
| Stored Access Policy | Controlar permisos SAS | Blob Containers, File Shares, Queues, Tables | Máx. 5 policies por recurso |
| Immutable Blob Storage Policy | Protección WORM contra modificación y borrado | Blob Containers, Blob Versions | 1 policy de inmutabilidad por scope |
| Time-Based Retention Policy | Bloquear blobs durante un tiempo definido | Blob Containers, Blob Versions | 1 policy activa por scope |
| Legal Hold Policy | Bloquear blobs indefinidamente | Blob Containers, Blob Versions | 1 legal hold policy por scope (con múltiples tags posibles) |
| Lifecycle Management Policy | Automatizar movimiento/borrado de datos | Storage Account / Blob Storage | 1 lifecycle management policy por Storage Account |

---

# 1. Stored Access Policy

## Qué es

Una:

```text
Stored Access Policy
```

permite definir permisos SAS reutilizables sobre recursos Azure Storage.

En vez de poner permisos directamente dentro del SAS token,
el SAS referencia una policy almacenada.

---

## Recursos compatibles

| Recurso | Compatible |
|---|---|
| Blob Containers | ✅ |
| File Shares | ✅ |
| Queues | ✅ |
| Tables | ✅ |

---

## Máximo

```text
Maximum 5 stored access policies per resource
```

Aplica a:
- container
- queue
- table
- file share

---

## Qué controla

Puede controlar:

- permisos
- expiración
- start time

---

## Ventaja importante

Permite:

✅ revocar SAS fácilmente  
✅ modificar expiración sin regenerar SAS  
✅ centralizar permisos  

---

## Importante examen

Stored Access Policies:

❌ NO protegen contra borrado  
❌ NO son WORM  
❌ NO son Immutable Policies  

Solo controlan acceso SAS.

---

# 2. Immutable Blob Storage Policy

## Qué es

Protección:

```text
WORM
```

sobre blobs.

---

## WORM significa

```text
Write Once Read Many
```

↓

Los datos:

✅ pueden escribirse  
✅ pueden leerse  
❌ no pueden modificarse  
❌ no pueden borrarse  

---

## Recursos compatibles

| Recurso | Compatible |
|---|---|
| Blob Containers | ✅ |
| Blob Versions | ✅ |
| Tables | ❌ |
| Queues | ❌ |
| File Shares | ❌ |

---

## Máximo

```text
1 immutable policy per scope
```

---

## Qué protege

Impide:

❌ modificar blobs  
❌ borrar blobs  

incluso para administradores.

---

## Importante examen

Immutable Policies:
- solo existen en Blob Storage
- NO existen para Tables o Queues

---

# 3. Time-Based Retention Policy

## Qué es

Tipo de Immutable Policy.

Bloquea blobs durante un tiempo definido.

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

## Recursos compatibles

| Recurso | Compatible |
|---|---|
| Blob Containers | ✅ |
| Blob Versions | ✅ |

---

## Máximo

```text
1 active retention policy per scope
```

---

## Estados importantes

| Estado | Significado |
|---|---|
| Unlocked | Editable |
| Locked | Inmutable definitiva |

---

## Importante examen

Una vez:

```text
Locked
```

↓

❌ no puede reducirse fácilmente  
❌ no puede eliminarse fácilmente  

---

# 4. Legal Hold Policy

## Qué es

Tipo de Immutable Policy.

Bloquea blobs indefinidamente.

---

## Cómo funciona

Los blobs quedan protegidos:

```text
hasta eliminar manualmente el legal hold
```

---

## Uso típico

- litigios
- auditorías
- investigaciones

---

## Recursos compatibles

| Recurso | Compatible |
|---|---|
| Blob Containers | ✅ |
| Blob Versions | ✅ |

---

## Máximo

```text
1 legal hold policy per scope
```

pero:

✅ puede contener múltiples tags  

---

## Diferencia clave examen

| Feature | Duración |
|---|---|
| Time-Based Retention | Tiempo definido |
| Legal Hold | Indefinido |

---

# 5. Lifecycle Management Policy

## Qué es

Policy automática para mover o borrar blobs.

---

## Qué permite

Automatizar:

- mover Hot → Cool
- mover Cool → Archive
- borrar blobs antiguos
- borrar snapshots/versiones

---

## Recursos compatibles

| Recurso | Compatible |
|---|---|
| Storage Account | ✅ |

---

## Máximo

```text
1 lifecycle management policy per Storage Account
```

---

## Importante

Dentro de esa única policy:

✅ múltiples reglas permitidas  

---

## Ejemplo típico

```text
Move blobs to Cool after 30 days
Move blobs to Archive after 90 days
Delete after 365 days
```

---

# Comparativa rápida examen

| Policy | Protege borrado | Controla acceso | Automatiza lifecycle |
|---|---|---|---|
| Stored Access Policy | ❌ | ✅ | ❌ |
| Immutable Policy | ✅ | ❌ | ❌ |
| Time-Based Retention | ✅ | ❌ | ❌ |
| Legal Hold | ✅ | ❌ | ❌ |
| Lifecycle Management | ❌ | ❌ | ✅ |

---

# Frases clave AZ-104

```text
Stored Access Policies control SAS permissions.
```

```text
Immutable Blob Storage provides WORM protection.
```

```text
Time-Based Retention prevents modification and deletion during retention.
```

```text
Legal Holds preserve blobs indefinitely.
```

```text
Lifecycle Management Policies automate blob tiering and deletion.
```

