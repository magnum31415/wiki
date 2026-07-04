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
- [Azure Storage - Hierarchical Namespace vs Azure Files](#azure-storage---hierarchical-namespace-vs-azure-files)
- [Azure Storage Routing Preference (AZ-104)](#azure-storage-routing-preference-az-104)
- [Azure Storage - Encryption Scope](#azure-storage---encryption-scope)
---

## Modelos de autenticación en Azure Storage

| Model | Uses Claims | Uses Keys | Example | Related Concept | What it is | Where it is used |
|---|---|---|---|---|---|---|
| Microsoft Entra ID (RBAC) | ✔ | ❌ | A user or application authenticates with Entra ID and accesses a blob using a role like **Storage Blob Data Contributor**. | **Claim** | Information about an identity included in an authentication token. | OAuth / OpenID Connect / Microsoft Entra ID |
| Shared Key (Access Keys) | ❌ | ✔ | An application connects to a storage account using a **connection string with the AccountKey**. | **Access Key (Storage)** | Secret key that provides direct access to the Storage Account. | Key-based authentication |
| SAS Token | ❌ | ✔ | **Shared Access Signature Token** : A temporary URL is generated to allow download of a specific blob for a limited time. | **Access Key (or delegation key)** | Token generated from a storage key or delegation key to grant temporary access. | Delegated access to storage resources |




| Model                         | Protocol / Access Method | Blob Storage |                                               Azure Files | Queues | Tables |
| ----------------------------- | ------------------------ | -----------: | --------------------------------------------------------: | -----: | -----: |
| **Microsoft Entra ID (RBAC)** | REST / HTTPS             |            ✅ |                                                         ✅ |      ✅ |      ✅ |
| **Microsoft Entra ID (RBAC)** | SMB (`net use`)          |          N/A | ⚠️ Sí, con identidad compatible y configuración adicional |    N/A |    N/A |
| **Shared Key (Access Keys)**  | REST / HTTPS             |            ✅ |                                                         ✅ |      ✅ |      ✅ |
| **Shared Key (Access Keys)**  | SMB (`net use`)          |          N/A |                                                         ✅ |    N/A |    N/A |
| **SAS Token**                 | REST / HTTPS             |            ✅ |                                                         ✅ |      ✅ |      ✅ |



⚠️ **Sí, con identidad compatible y configuración adicional** 
- Significa que no basta con tener un usuario en Microsoft Entra ID y asignarle un rol RBAC.
- Para acceder a Azure Files mediante SMB (net use), Azure Files necesita un sistema de identidad capaz de autenticar mediante Kerberos.

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


# Tipos de replicación Azure Storage

| Tipo | Replicación | Región secundaria |
|---|---|---|
| LRS | Datacenter local | ❌ |
| ZRS | Availability Zones | ❌ |
| GRS | Otra región | ✅ |
| RA-GRS | Otra región + lectura | ✅ |

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


# Trampas típicas examen

## Trampa 1

- Pensar que: `` ZRS replica a otra región`` ❌ Incorrecto.
- ZRS replica solo dentro de la misma región.


## Diferencia rápida

| Tier | Uso |
|---|---|
| Hot | Acceso frecuente |
| Cool | Acceso poco frecuente |
| Archive | Retención largo plazo |

## Trampa 3

- Pensar que: ``Azure Data Lake es un servicio separado``  ❌ Incorrecto.
- ADLS Gen2 realmente es: ``Blob Storage + Hierarchical Namespace``

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

## Tabla resumen de Policies

| Tipo de Policy | Categoría | Objetivo | Recursos compatibles | Máximo | Ejemplo |
|---|---|---|---|---|---|
| **Stored Access Policy** | Access Control Policy | Controlar permisos y duración de una SAS | Blob Containers, File Shares, Queues, Tables | Máx. **5 policies por recurso** | Permitir a un partner leer y escribir blobs durante 6 meses mediante una SAS vinculada a la policy |
| **Immutable Blob Storage Policy** | WORM / Immutable Policy | Concepto general de protección WORM contra modificación y borrado | Blob Containers, Blob Versions | Incluye los mecanismos **Time-Based Retention** y **Legal Hold** | Proteger facturas para impedir que puedan ser modificadas o eliminadas |
| **Time-Based Retention Policy** | Immutable Blob Storage Policy | Bloquear blobs durante un tiempo definido | Blob Containers, Blob Versions | **1 policy activa por scope** | Impedir modificar o borrar facturas durante **7 años** |
| **Legal Hold Policy** | Immutable Blob Storage Policy | Bloquear blobs indefinidamente hasta retirar el bloqueo | Blob Containers, Blob Versions | **1 legal hold por scope**, con múltiples tags posibles | Bloquear documentos relacionados con una investigación judicial hasta que finalice |
| **Lifecycle Management Policy** | Lifecycle / Data Management Policy | Automatizar movimiento o borrado de datos | Storage Account / Blob Storage | **1 lifecycle management policy por Storage Account** | Mover blobs a Cool después de 30 días, a Archive después de 180 días y borrarlos después de 7 años |


**Immutable Blob Storage Policy** es el concepto general de **inmutabilidad WORM**, mientras que **Time-Based Retention** y **Legal Hold son sus dos mecanismos**.

```mermaid
graph TD

    A[Azure Storage Policies]

    A --> B[Access Control Policies]
    A --> C[Immutable Blob Storage Policies]
    A --> D[Lifecycle Management Policies]

    B --> B1[Stored Access Policy]
    B1 --> B2[Blob Containers]
    B1 --> B3[File Shares]
    B1 --> B4[Queues]
    B1 --> B5[Tables]

    C --> C1[Time-Based Retention Policy]
    C --> C2[Legal Hold Policy]

    C1 --> C3[Blob Containers]
    C1 --> C4[Blob Versions]

    C2 --> C5[Blob Containers]
    C2 --> C6[Blob Versions]

    D --> D1[Lifecycle Management Policy]
    D1 --> D2[Storage Account / Blob Storage]
```


Azure utiliza la palabra **policy** para dos mecanismos completamente diferentes.

| Tipo de policy | Objetivo | La pregunta menciona... | Máximo por container |
|---|---|---|---:|
| **Stored Access Policy** | Controlar una SAS: permisos y duración | SAS, permisos, expiración | **5** |
| **Immutable Blob Storage Policy** | Impedir modificar o borrar datos | WORM, retention, legal hold, cannot delete | **2 mecanismos: Time-based retention + Legal hold** |

## Regla para el examen

- **Stored Access Policy = acceso mediante SAS = máximo 5 por container**
- **Immutable Blob Storage = protección contra modificación y borrado = Time-based retention + Legal hold**

# 1. Stored Access Policy

## Qué es

Una:

```text
Stored Access Policy
```


La **Stored Access Policy** sirve para controlar el acceso mediante una SAS sobre recursos Azure Storage.

```text
Container
   │
   └── Stored Access Policy
           │
           └── SAS
```
En vez de poner permisos directamente dentro del SAS token, el SAS referencia una policy almacenada.


Permite definir:

- Qué operaciones se pueden realizar: Read, Write, Delete.
- Durante cuánto tiempo.
- Qué SAS utilizan esa política.

Un container admite hasta: **5 Stored Access Policies**

Ejemplo:

```text
container-partners
│
├── policy-read
├── policy-write
├── policy-partner-A
├── policy-partner-B
└── policy-temporary
```

## Recursos compatibles

| Recurso | Compatible |
|---|---|
| Blob Containers | ✅ |
| File Shares | ✅ |
| Queues | ✅ |
| Tables | ✅ |

---

## Máximo

**Azure Storage accounts **support a **maximum of 5 stored access policies per container**.

```text
Maximum 5 stored access policies per resource
```

Aplica a:
- container
- queue
- table
- file share

## Qué controla

Puede controlar:

- permisos
- expiración
- start time

## Ventaja importante

Permite:

✅ revocar SAS fácilmente  
✅ modificar expiración sin regenerar SAS  
✅ centralizar permisos  


## Importante examen

Stored Access Policies:

❌ NO protegen contra borrado  
❌ NO son WORM  
❌ NO son Immutable Policies  

Solo controlan acceso SAS.


## Ejemplo: Imagina este Storage Account:

```text
Storage Account: stcompany
│
├── container-public
├── container-partners
└── container-private
```

Cada contenedor puede tener una política de acceso diferente:

| Container | Access Policy | Permisos | Ejemplo |
|---|---|---|---|
| `container-public` | `policy-public` | Read | Los usuarios pueden descargar documentos |
| `container-partners` | `policy-partners` | Read + Write | Un partner puede subir y descargar archivos |
| `container-private` | `policy-admin` | Read + Write + Delete | Los administradores tienen control completo |

## Ejemplo: container-partners

En `container-partners` creas una **Stored Access Policy**:

```text
Policy name: partner-access
Permissions: Read + Write
Start: 2026-07-01
Expiry: 2026-12-31
```

Después generas una **Service SAS** vinculada a esa política:

```text
Container
   │
   ├── Stored Access Policy
   │       └── partner-access
   │              ├── Read
   │              ├── Write
   │              └── Expiry: 31/12/2026
   │
   └── SAS Token
           └── Vinculado a partner-access
```

La ventaja es que si 20 partners usan SAS vinculadas a `partner-access`, puedes cambiar o revocar el acceso modificando **una sola Stored Access Policy**, sin tener que regenerar individualmente las 20 SAS.

## Regla para el examen

> **Stored Access Policy → define permisos y duración → SAS utiliza la policy para acceder al container.**

Una **Stored Access Policy no se asigna directamente a usuarios o grupos de Entra ID**. Para eso se utiliza Azure RBAC.

---

# 2. Immutable Blob Storage Policy

## Qué es

Una **Immutable Blob Storage Policy** no controla quién puede acceder.

Controla si los datos pueden ser **modificados o eliminados**.

```text
Container
   │
   └── Immutable Storage
           │
           ├── Time-based retention policy
           │
           └── Legal hold
```

Los dos mecanismos de inmutabilidad son:

| Immutable policy | Qué hace | Ejemplo |
|---|---|---|
| **Time-based retention** | Impide modificar o borrar durante un período | Conservar facturas durante 7 años |
| **Legal hold** | Impide modificar o borrar hasta retirar el bloqueo | Documentos bajo investigación judicial |


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

---

# Azure Storage - Hierarchical Namespace vs Azure Files

---

# Concepto clave del examen

La pregunta evalúa la diferencia entre:

- Azure Blob Storage
- Azure Data Lake Storage Gen2
- Azure Files

y especialmente:

```text
cómo organizan contenido
```

---

# 1. Azure Blob Storage normal

Azure Blob Storage tradicional utiliza un:

```text
flat namespace
```

↓

Esto significa:

❌ no existen directorios reales  
❌ las carpetas son simuladas  

---

## Ejemplo

```text
folder1/file.txt
```

realmente es:

```text
un único nombre de blob
```

NO un directorio real.

---

# Importante examen

Blob Storage normal:

✅ almacena blobs  
❌ NO tiene jerarquía real de directorios  

---

# 2. Hierarchical Namespace (HNS)

Cuando habilitas:

```text
Hierarchical Namespace
```

↓

Azure Blob Storage se convierte en:

```text
Azure Data Lake Storage Gen2
```

---

# Qué aporta HNS

Con HNS:

✅ directorios reales  
✅ subdirectorios reales  
✅ operaciones filesystem-like  
✅ mejor organización contenido  

---

# Ejemplo

Ahora sí existe:

```text
/folder1/folder2/file.txt
```

como estructura jerárquica real.

---

# Importante examen

Para:

```text
organizar contenido en directorios reales
```

Blob Storage necesita:

```text
Hierarchical Namespace enabled
```

---

# 3. Azure Files

Azure Files funciona distinto.

Azure Files ya es:

```text
un sistema de ficheros compartido
```

---

# Qué soporta Azure Files

Azure Files soporta:

✅ directorios  
✅ subdirectorios  
✅ estructura jerárquica  

SIN necesidad de:

```text
Hierarchical Namespace
```

---

# Importante examen

Azure Files:

❌ NO necesita HNS  
✅ soporta carpetas por diseño  

---

# Trampa típica AZ-104

Pensar que:

```text
todos los servicios Azure Storage necesitan HNS
para usar directorios
```

❌ Incorrecto.

---

# Diferencia clave examen

| Servicio | Directorios reales | Requiere HNS |
|---|---|---|
| Blob Storage normal | ❌ | — |
| Blob Storage + HNS | ✅ | ✅ |
| Azure Files | ✅ | ❌ |

---

# Regla rápida examen

```text
Blob Storage needs Hierarchical Namespace for real directory organization.
```

```text
Azure Files supports directories natively.
```

---

# Ejemplo típico examen

## Escenario

La empresa necesita:

```text
organize content using directories
```

---

## Respuesta correcta

| Recurso | Resultado |
|---|---|
| Blob container SIN HNS | ❌ |
| Blob container CON HNS | ✅ |
| Azure File Share | ✅ |

---

# Frases clave AZ-104

```text
Hierarchical Namespace enables filesystem semantics in Blob Storage.
```

```text
Azure Data Lake Storage Gen2 requires Hierarchical Namespace.
```

```text
Azure Files supports directories without requiring Hierarchical Namespace.
```

```text
Blob Storage without HNS uses a flat namespace.
```
---

# Azure Storage Routing Preference (AZ-104)

---

# Qué es Azure Storage Routing Preference

Azure Storage Routing Preference permite controlar:

```text
cómo el tráfico de usuarios llega a una Storage Account
```

---

# Objetivo principal

Permite elegir si el tráfico:

- usa la red global privada de Microsoft
- o viaja más por Internet pública

---

# Opciones principales

| Routing Preference | Descripción |
|---|---|
| Microsoft network routing | Usa la red backbone global de Microsoft |
| Internet routing | Usa más Internet pública |

---

# Concepto clave examen

Si quieres:

```text
minimizar latencia
optimizar rendimiento
usar el POP más cercano
```

↓

debes usar:

```text
Microsoft network routing
```

---

# Qué es un POP

POP significa:

```text
Point Of Presence
```

---

# Qué hace un POP

Es un punto de entrada a la red global de Microsoft.

---

# Flujo con Microsoft Network Routing

```text
Usuario
   ↓
POP Microsoft más cercano
   ↓
Backbone global Microsoft
   ↓
Storage Account
```

---

# Ventajas de Microsoft Network Routing

| Ventaja | Explicación |
|---|---|
| Menor latencia | Menos recorrido por Internet pública |
| Mejor rendimiento | Backbone Microsoft optimizado |
| Mayor estabilidad | Red privada Microsoft |
| Entrada POP cercano | Mejor experiencia usuario |

---

# Qué significa "Microsoft edge network"

También llamado:

```text
Microsoft edge network
```

↓

El tráfico entra rápidamente en la red privada de Microsoft usando el POP más cercano.

---

# Backbone de Microsoft

Microsoft tiene una red global privada enorme entre datacenters Azure.

Cuando usas:

```text
Microsoft network routing
```

↓

el tráfico utiliza esa red interna lo antes posible.

---

# Internet Routing

## Qué hace

El tráfico usa más recorrido sobre:

```text
Internet pública
```

antes de entrar en la red Microsoft.

---

# Ventajas

Puede reducir costes de egress en algunos escenarios.

---

# Desventajas

| Problema | Explicación |
|---|---|
| Mayor latencia | Más recorrido Internet |
| Menor predictibilidad | Dependencia ISP |
| Menor optimización | Menos backbone Microsoft |

---

# Comparativa examen

| Característica | Microsoft Network Routing | Internet Routing |
|---|---|---|
| Usa backbone Microsoft | ✅ | Parcial |
| Usa POP más cercano | ✅ | ❌ |
| Menor latencia | ✅ | ❌ |
| Mejor rendimiento | ✅ | ❌ |
| Más Internet pública | ❌ | ✅ |
| Optimizado para usuarios globales | ✅ | ❌ |

---

# Caso típico examen

## Requisito

```text
Ensure inbound user traffic uses the Microsoft POP closest to the user.
```

↓

Respuesta correcta:

```text
Microsoft network routing
```

---

# Configuración conceptual

```text
Storage Account
    ↓
Routing Preference
    ↓
Microsoft Network Routing
```

---

# Importante examen

Routing Preference afecta:

```text
cómo entra el tráfico a Azure
```

NO:
- cifrado
- replicación
- firewall
- private endpoints

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
Internet routing = más rápido
```

❌ Incorrecto.

---

## Trampa 2

Confundir:

```text
Routing Preference
```

con:

```text
Traffic Manager / Front Door
```

---

## Trampa 3

Pensar que esto afecta tráfico interno Azure.

❌ Principalmente afecta tráfico usuario → Storage.

---

# Arquitectura conceptual

## Microsoft Network Routing

```text
User
 ↓
Nearest Microsoft POP
 ↓
Microsoft Backbone
 ↓
Azure Storage
```

---

## Internet Routing

```text
User
 ↓
Internet pública
 ↓
Azure region
 ↓
Azure Storage
```

---

# Reglas rápidas AZ-104

```text
Microsoft network routing uses the Microsoft global backbone network.
```

```text
Microsoft routing minimizes latency and improves performance.
```

```text
Internet routing uses more public Internet paths.
```

```text
POP means Point Of Presence.
```

---

# Frases clave AZ-104

```text
Microsoft network routing routes traffic through the Microsoft global network.
```

```text
The closest Microsoft POP is used to optimize inbound traffic.
```

```text
Microsoft network routing improves performance and reduces latency.
```
---
# Azure Storage - Encryption Scope

La clave de esta pregunta está en una frase:

> **One container must use a separate encryption key for data encrypted at rest.**

La respuesta correcta es **Create an encryption scope**.

En una cuenta de Azure Storage, por defecto los datos se cifran **a nivel de Storage Account**. Pero si quieres que un contenedor concreto use una clave diferente, necesitas un **Encryption Scope**.

| Concepto | Configuración | ¿Qué protege/controla? | Ejemplo |
|---|---|---|---|
| 🔐 Cifrado independiente | **Encryption Scope** | Datos **at rest** con una clave diferente | `container-sensitive` usa su propia clave |
| 🔑 Autenticación | Access Keys | Acceso al Storage Account | Una aplicación se autentica con la key |
| 🌐 Cifrado en tránsito | Minimum TLS version | Comunicación cliente ↔ Azure | Obligar a usar TLS 1.2 |
| 🎟️ Acceso delegado | SAS | Acceso temporal y limitado | Permitir descargar un blob durante 2 horas |

## ¿Qué es exactamente un Encryption Scope?

Imagina este Storage Account con 10 contenedores:

```text
Storage Account
│
├── container-01 ─┐
├── container-02  │
├── container-03  │── Clave por defecto del Storage Account 🔑 A
├── container-04  │
├── ...
├── container-09 ─┘
│
└── container-sensitive ── Encryption Scope ── 🔑 B
```

El **Encryption Scope** crea un "ámbito de cifrado" que puede asociarse a un contenedor.

```text
Encryption Scope: sensitive-data
        │
        ▼
Container: confidential
        │
        ▼
Cifrado con una clave diferente 🔑
```

Por eso, antes de crear el contenedor:

```text
1. Crear Encryption Scope
          ↓
2. Crear el Blob Container
          ↓
3. Asignarle el Encryption Scope
```

## Encryption Scope vs Customer-Managed Key (CMK)

| Configuración | Alcance |
|---|---|
| **Customer-managed key (CMK)** del Storage Account | Todo el Storage Account |
| **Encryption Scope** | Contenedores o blobs concretos |

Un Encryption Scope puede usar:

- **Microsoft-managed key**
- **Customer-managed key** almacenada en Key Vault o Managed HSM

La idea importante es que el **scope** define qué clave se aplica a determinados datos.

## Por qué `Rotate the access keys` es incorrecto

Las **Access Keys no cifran los datos almacenados**. Sirven para autenticarse contra el Storage Account.

```text
Access Key
    ↓
¿Puedes acceder al Storage Account?
```

Mientras que:

```text
Encryption Scope
    ↓
¿Con qué clave se cifran estos datos?
```

Son capas de seguridad completamente diferentes.

## Cómo identificar la respuesta en AZ-104

| Si la pregunta dice... | Piensa en... |
|---|---|
| **Different key per container/blob** | 🔐 Encryption Scope |
| **Encrypt entire Storage Account with own key** | 🔐 Customer-managed key |
| **Regenerate/compromised account key** | 🔑 Rotate Access Keys |
| **Data in transit / protocol version** | 🌐 TLS |
| **Temporary delegated access** | 🎟️ SAS |

## Regla para el examen

 **Encryption Scope = usar claves de cifrado diferentes para blobs o contenedores específicos dentro del mismo Storage Account.**

 ---


 # Azure File Shares (AZ-104)

## Concepto

**Azure Files** proporciona recursos compartidos de archivos totalmente administrados en Azure.

Permite acceder a los mismos archivos desde:

- Windows
- Linux
- macOS
- Aplicaciones

```text
Storage Account
        │
        └── File Share
                │
                ├── Folder
                ├── Folder
                └── Files
```

---

## Protocolos soportados

| Protocolo | Puerto | Uso principal |
|---|---:|---|
| **SMB** | **TCP 445** | Montar unidades de red desde Windows, Linux o macOS |
| **NFS** | **TCP 2049** | Montar File Shares desde clientes Linux/Unix |
| **REST / HTTPS** | **TCP 443** | Acceso mediante API, SDK o herramientas |

---

## SMB (Server Message Block)

SMB permite montar un Azure File Share como una unidad de red.

Ejemplo en Windows:

```cmd
net use Z: \\apexcorestorage.file.core.windows.net\data
```

Flujo:

```text
Windows 10
    │
    │ TCP 445
    ▼
SMB
    │
    ▼
Azure File Share
```

### Regla AZ-104

> **Map network drive + Azure File Share = SMB = TCP 445**

---

## Puertos importantes

| Puerto | Protocolo | Uso |
|---:|---|---|
| **445** | SMB | Azure File Shares |
| **443** | HTTPS | REST API / Portal / SDK |
| **2049** | NFS | NFS File Shares |
| **3389** | RDP | Acceso remoto a Windows |
| **80** | HTTP | Tráfico web sin cifrar |

---

## Acceso desde Internet

Para montar directamente un Azure File Share mediante SMB desde un equipo externo:

```text
Home Computer
      │
      │ TCP 445
      ▼
Internet
      │
      ▼
Azure File Share
```

El puerto **TCP 445 outbound** debe estar permitido.

Algunos proveedores de Internet y redes corporativas bloquean el puerto 445.

### Alternativas

Si TCP 445 está bloqueado:

- VPN hacia Azure
- ExpressRoute
- Azure File Sync

---

## Modelos de autenticación

| Modelo de autenticación | Protocolo / acceso | Azure Files | Soportado |
|---|---|---|---|
| **Shared Key (Access Key)** | SMB (`net use`) | Storage Account Key como contraseña | ✅ |
| **Shared Key (Access Key)** | REST / HTTPS | Autenticación mediante Account Key | ✅ |
| **Identity-based authentication** | SMB | Entra Kerberos / AD DS / Entra Domain Services | ✅ |
| **SAS Token** | REST / HTTPS | Acceso temporal y delegado | ✅ |
| **SAS Token** | SMB (`net use`) | SAS como contraseña | ❌ |

---

## SAS y Azure Files

Una SAS puede utilizarse para acceder a Azure Files mediante REST/HTTPS:

```text
Application
    │
    │ HTTPS / REST
    │ SAS Token
    ▼
Azure File Share
```

Una SAS no puede utilizarse como contraseña para SMB:

```text
net use
    │
    │ SMB
    │ SAS Token
    ▼
Azure File Share
    │
    └── ❌ No access
```

### Regla AZ-104

> **SAS + REST/HTTPS → Azure Files ✅**

> **SAS + SMB / net use → Azure Files ❌**

---

## Autenticación basada en identidad para SMB

Azure Files soporta autenticación SMB mediante:

| Sistema de identidad | SMB |
|---|---|
| **Microsoft Entra Kerberos** | ✅ |
| **Active Directory Domain Services (AD DS)** | ✅ |
| **Microsoft Entra Domain Services** | ✅ |

Flujo:

```text
Usuario
    │
    ▼
Sistema de identidad
    │
    ▼
Kerberos
    │
    ▼
SMB
    │
    ▼
Azure File Share
```

También deben configurarse:

1. Autenticación basada en identidad.
2. Permisos Azure RBAC sobre el File Share.
3. Permisos de directorio y archivo.

---

## Roles RBAC habituales

| Rol | Permisos |
|---|---|
| **Storage File Data SMB Share Reader** | Lectura |
| **Storage File Data SMB Share Contributor** | Lectura, escritura y eliminación |
| **Storage File Data SMB Share Elevated Contributor** | Permisos elevados, incluido modificar ACL |

---

## Azure RBAC vs permisos de archivos

El acceso basado en identidad utiliza dos niveles:

```text
Usuario
    │
    ▼
Azure RBAC
¿Puede acceder al File Share?
    │
    ▼
Permisos de directorio/archivo
¿Puede acceder a esta carpeta o archivo?
```

| Nivel | Controla |
|---|---|
| **Azure RBAC** | Acceso al File Share |
| **Permisos de directorio/archivo** | Acceso a carpetas y archivos concretos |

---

## Azure File Sync

Azure File Sync permite mantener archivos en un Windows Server local y sincronizarlos con Azure Files.

```text
Windows Server
      │
      │ Azure File Sync
      ▼
Azure File Share
```

Componentes principales:

| Componente | Función |
|---|---|
| **Storage Sync Service** | Recurso principal de Azure File Sync |
| **Sync Group** | Define qué ubicaciones se sincronizan |
| **Cloud Endpoint** | Azure File Share |
| **Server Endpoint** | Carpeta de Windows Server |
| **Azure File Sync Agent** | Agente instalado en Windows Server |

---

## Cloud Tiering

Cloud Tiering permite mantener localmente los archivos más utilizados y mover el contenido menos utilizado a Azure.

```text
Archivos frecuentes
        │
        └── Windows Server

Archivos poco utilizados
        │
        └── Azure File Share
```

Los archivos movidos a Azure siguen apareciendo en el servidor como archivos disponibles.

---

## Azure Files vs Blob Storage

| Característica | Azure Files | Blob Storage |
|---|---|---|
| Sistema de archivos compartido | ✅ | ❌ |
| SMB | ✅ | ❌ |
| NFS | ✅ | ⚠️ Según configuración |
| Map network drive | ✅ | ❌ |
| Acceso REST | ✅ | ✅ |
| Object Storage | ❌ | ✅ |
| Uso típico | File Server compartido | Objetos, backups, imágenes, logs |

---

## Reglas rápidas para el examen

| Si la pregunta dice... | Piensa en... |
|---|---|
| **Map network drive** | SMB |
| **SMB** | TCP 445 |
| **NFS** | TCP 2049 |
| **REST / HTTPS** | TCP 443 |
| **`net use`** | SMB |
| **SAS + `net use`** | ❌ No access |
| **SAS + REST** | ✅ Soportado |
| **On-premises file server synchronization** | Azure File Sync |
| **Azure copy of the files** | Cloud Endpoint |
| **Local Windows Server folder** | Server Endpoint |
| **Keep frequently used files locally** | Cloud Tiering |

## Regla principal

> **Azure File Share + map network drive = SMB = TCP 445**
