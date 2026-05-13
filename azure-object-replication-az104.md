[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Storage Services y Replicación (AZ-104)


# Azure Storage Account Types y Object Replication (AZ-104)

| Account Type | Blob Storage | Azure Files | Queue Storage | Table Storage | Soporta Blob Containers | Soporta Object Replication | Uso principal |
|---|---|---|---|---|---|---|---|
| `StorageV2` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Storage account general-purpose moderno |
| `BlobStorage` | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ | Optimizado exclusivamente para Blob Storage |
| `FileStorage` | ❌ | ✅ Premium | ❌ | ❌ | ❌ | ❌ | Azure Files premium (SMB/NFS) |


# Azure Storage Account

Un Storage Account puede contener varios servicios:

| Servicio | Uso principal |
|---|---|
| Blob Storage | Archivos y objetos |
| Azure Files | Compartición SMB/NFS |
| Queue Storage | Mensajería |
| Table Storage | NoSQL key-value |

---

# Blob Storage

# Qué es

Servicio de almacenamiento de objetos no estructurados.

---

# Uso típico

- Imágenes
- Videos
- PDFs
- Backups
- Logs
- Archivos grandes
- Data Lakes

---

# Jerarquía

```text
Storage Account
    ↓
Container
    ↓
Blob
```

---

# Tipos de blobs

| Tipo | Uso |
|---|---|
| blockBlob | Archivos normales |
| appendBlob | Logs |
| pageBlob | VHDs |

---

# Block Blob

## Uso típico

✅ Imágenes  
✅ PDFs  
✅ Videos  
✅ CSV  
✅ Backups  

---

# Append Blob

## Uso típico

✅ Logs  
✅ Auditoría  
✅ Datos append-only  

---

# Page Blob

## Uso típico

✅ Discos VHD de VMs Azure

---

# Object Replication en Blob Storage

## Soportado

✅ Sí

---

# Cómo funciona

```text
Source Container
        ↓
Destination Container
```

---

# Características

| Feature | Soportado |
|---|---|
| Replicación automática | ✅ |
| Políticas de replicación | ✅ |
| Multi-region | ✅ |
| Filtrado por prefijos | ✅ |
| Replicación asíncrona | ✅ |

---

# Requisitos

| Requisito | Necesario |
|---|---|
| Source container | ✅ |
| Destination container | ✅ |
| Blob versioning | ✅ |
| Block blobs | ✅ |

---

# Importante

Object Replication:

✅ SOLO soporta Block Blobs

---

# NO soporta

❌ Append blobs  
❌ Page blobs  

---

# Regla importante

```text
Object Replication is ONLY supported for Blob Storage.
```

---

# Azure Files

# Qué es

Servicio de compartición de archivos administrado.

---

# Protocolos soportados

| Protocolo | Soportado |
|---|---|
| SMB | ✅ |
| NFS | ✅ |

---

# Uso típico

- File shares
- Lift & Shift
- Shared folders
- Reemplazo File Server

---

# Jerarquía

```text
Storage Account
    ↓
File Share
    ↓
Files/Folders
```

---

# Objeto principal

```text
File Share
```

---

# Object Replication en Azure Files

## Soportado

❌ NO

---

# Entonces, ¿cómo se replica?

Azure Files usa:

---

# 1. Redundancy del Storage Account

| Tipo | Replica |
|---|---|
| LRS | misma región |
| ZRS | varias Availability Zones |
| GRS | paired region |
| GZRS | AZ + paired region |

---

# 2. Azure File Sync

Permite sincronizar:

```text
Windows Server ↔ Azure File Share
```

---

# 3. Herramientas de copia

- AzCopy
- Robocopy
- Azure Data Factory

---

# Regla importante

```text
Azure Files does NOT support Object Replication.
```

---

# Queue Storage

# Qué es

Servicio de colas de mensajes.

---

# Uso típico

- Comunicación entre aplicaciones
- Procesamiento asíncrono
- Desacoplamiento

---

# Jerarquía

```text
Storage Account
    ↓
Queue
    ↓
Messages
```

---

# Objeto principal

```text
Queue
```

---

# Object Replication en Queue Storage

## Soportado

❌ NO

---

# Entonces, ¿cómo se replica?

Mediante replicación interna del Storage Account:

| Tipo | Replica |
|---|---|
| LRS | misma región |
| ZRS | varias AZ |
| GRS | paired region |
| RA-GRS | lectura secundaria |
| GZRS | AZ + región secundaria |

---

# Importante

NO existe:

```text
Queue -> Queue replication policy
```

---

# Regla importante

```text
Queue Storage relies on Storage Account redundancy, not Object Replication.
```

---

# Table Storage

# Qué es

Servicio NoSQL key-value.

---

# Uso típico

- Datos estructurados simples
- Telemetría
- Inventarios
- Metadata

---

# Jerarquía

```text
Storage Account
    ↓
Table
    ↓
Entities
```

---

# Objeto principal

```text
Table
```

---

# Object Replication en Table Storage

## Soportado

❌ NO

---

# Entonces, ¿cómo se replica?

Mediante redundancia del Storage Account:

| Tipo | Replica |
|---|---|
| LRS | misma región |
| ZRS | varias AZ |
| GRS | paired region |
| RA-GRS | lectura secundaria |
| GZRS | AZ + región secundaria |

---

# Importante

NO existe:

```text
Table -> Table replication policy
```

---

# Diferencia fundamental

# Object Replication

## Características

✅ Configurable  
✅ Entre containers  
✅ Basado en políticas  
✅ Solo Blob Storage  

---

# Redundancy (GRS/ZRS/LRS)

## Características

✅ Transparente  
✅ Automática  
✅ Nivel Storage Account  
✅ Todos los servicios storage  

---

# Tipos de redundancia

| Tipo | Descripción |
|---|---|
| LRS | 3 copias misma región |
| ZRS | varias Availability Zones |
| GRS | paired region |
| RA-GRS | lectura secundaria |
| GZRS | ZRS + GRS |
| RA-GZRS | lectura secundaria + GZRS |

---

# Tabla resumen completa

| Servicio | Objeto principal | Object Replication |
|---|---|---|
| Blob Storage | Container | ✅ |
| Azure Files | File Share | ❌ |
| Queue Storage | Queue | ❌ |
| Table Storage | Table | ❌ |

---

# Cómo se replica cada servicio

| Servicio | Método de replicación |
|---|---|
| Blob Storage | Object Replication + Redundancy |
| Azure Files | Redundancy + File Sync |
| Queue Storage | Redundancy |
| Table Storage | Redundancy |

---

# Trampas típicas AZ-104

# Trampa 1

Pensar que Object Replication aplica a todos los servicios storage.

❌ Incorrecto

Solo Blob Storage.

---

# Trampa 2

Pensar que File Shares usan containers.

❌ Incorrecto

Usan:

```text
File Shares
```

---

# Trampa 3

Confundir Queue Storage con Service Bus.

| Servicio | Uso |
|---|---|
| Queue Storage | Simple queue |
| Service Bus | Enterprise messaging |

---

# Trampa 4

Pensar que Object Replication replica Storage Accounts completos.

❌ Incorrecto

Replica:

```text
Container -> Container
```

---

# Reglas rápidas examen

| Necesidad | Servicio |
|---|---|
| Imágenes/PDFs | Blob Storage |
| SMB File Share | Azure Files |
| Mensajería simple | Queue Storage |
| NoSQL key-value | Table Storage |

---

# Frases clave AZ-104

```text
Object Replication is only supported for Blob Storage.
```

```text
Azure Files, Queues and Tables rely on Storage Account redundancy for replication.
```

```text
Blob Object Replication works between blob containers.
```
