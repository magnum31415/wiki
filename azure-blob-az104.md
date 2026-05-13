[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Blob Inventory y Blob Types (AZ-104)

# Qué es Azure Blob Inventory

Azure Blob Inventory permite generar automáticamente reportes sobre blobs almacenados en un Storage Account.

Los reportes pueden incluir:

- Nombre de blobs
- Tipo de blob
- Snapshots
- Versions
- Metadata
- Última modificación
- Tier
- Estado de replicación
- etc.

---

# Objetivo típico

Blob Inventory se usa para:

| Caso de uso | Ejemplo |
|---|---|
| Auditoría | Inventario de blobs |
| Compliance | Detectar blobs antiguos |
| Cost optimization | Encontrar blobs no usados |
| Lifecycle management | Aplicar políticas |
| Reporting | Exportar información CSV |

---

# Formatos soportados

Blob Inventory puede generar:

| Formato | Soportado |
|---|---|
| CSV | ✅ |
| Parquet | ✅ |

---

# Frecuencia soportada

| Schedule | Soportado |
|---|---|
| Daily | ✅ |
| Weekly | ❌ |

En AZ-104 normalmente verás:

```json
"schedule": "daily"
```

---

# Estructura típica de una Inventory Rule

```json
{
  "enabled": true,
  "destination": "CSV",
  "definition": {
    "filters": {
      "blobTypes": ["blockBlob"],
      "prefixMatch": ["images/finance"]
    },
    "format": "string",
    "objectType": "blob",
    "schedule": "daily",
    "schemaFields": ["Name"]
  }
}
```

---

# blobTypes

Define qué tipos de blobs se incluirán en el inventario.

---

# Tipos de blobs en Azure

| Blob Type | Uso típico |
|---|---|
| blockBlob | Archivos normales |
| appendBlob | Logs |
| pageBlob | Discos VHD |

---

# Block Blob

## Uso típico

✅ Imágenes  
✅ PDFs  
✅ Documentos  
✅ Backups  
✅ CSV  
✅ Videos  
✅ Archivos generales  

---

## Características

- Blob más común
- Optimizado para streaming
- Optimizado para uploads/downloads
- Datos no modificados frecuentemente

---

## Ejemplos

```text
image.jpg
backup.zip
report.csv
video.mp4
```

---

# Append Blob

## Uso típico

✅ Logs  
✅ Auditoría  
✅ Datos append-only  

---

## Características

- Solo se puede añadir contenido al final
- Optimizado para escritura secuencial

---

## Ejemplos

```text
application.log
audit.log
events.log
```

---

# Page Blob

## Uso típico

✅ VHDs  
✅ Azure VM disks  

---

## Características

- Acceso aleatorio
- Tamaño fijo
- Optimizado para IO de discos

---

## Ejemplos

```text
osdisk.vhd
datadisk.vhd
```

---

# Tabla resumen Blob Types

| Tipo | Uso principal | Ejemplo |
|---|---|---|
| blockBlob | Archivos normales | imágenes |
| appendBlob | Logs | logs |
| pageBlob | VHDs | discos VM |

---

# Regla importante AZ-104

```text
Images, documents, backups and CSV files are usually stored as block blobs.
```

---

# prefixMatch

Permite filtrar blobs por prefijo.

---

# Ejemplo

```json
"prefixMatch": ["images/finance"]
```

Incluye blobs como:

```text
images/finance-report.csv
images/finance2026.jpg
images/finance/invoice01.pdf
```

---

# Importante

El formato normalmente es:

```text
container-name/prefix
```

---

# includeBlobVersions

## Qué hace

Incluye versiones de blobs en el reporte.

---

## Ejemplo

```json
"includeBlobVersions": true
```

---

# includeSnapshots

## Qué hace

Incluye snapshots de blobs.

---

## Ejemplo

```json
"includeSnapshots": true
```

---

# Snapshots vs Versions

| Feature | Snapshot | Version |
|---|---|---|
| Punto en el tiempo | ✅ | ✅ |
| Gestión manual | Normalmente sí | Automática |
| Requiere versioning | ❌ | ✅ |

---

# objectType

Define qué objetos inventariar.

---

## Valores típicos

| Valor | Significado |
|---|---|
| blob | Blobs |
| container | Containers |

---

# schemaFields

Define qué campos incluir en el reporte.

---

## Ejemplo

```json
"schemaFields": ["Name"]
```

---

# Campos típicos

| Campo | Significado |
|---|---|
| Name | Nombre blob |
| LastModified | Última modificación |
| BlobType | Tipo blob |
| AccessTier | Tier |
| ContentLength | Tamaño |

---

# Trampas típicas AZ-104

# Trampa 1

Pensar que imágenes usan appendBlob.

❌ Incorrecto

Imágenes usan:

✅ blockBlob

---

# Trampa 2

Pensar que pageBlob sirve para archivos normales.

❌ Incorrecto

pageBlob es para:

✅ VHDs

---

# Trampa 3

Confundir appendBlob con blockBlob.

| Tipo | Uso |
|---|---|
| appendBlob | logs |
| blockBlob | archivos normales |

---

# Regla rápida examen

| Tipo de dato | Blob Type |
|---|---|
| Imagen | blockBlob |
| PDF | blockBlob |
| CSV | blockBlob |
| Backup | blockBlob |
| Logs | appendBlob |
| VHD | pageBlob |

---

# Frases clave AZ-104

```text
Block blobs are optimized for storing unstructured data such as images and documents.
```

```text
Append blobs are optimized for append operations such as logging.
```

```text
Page blobs are optimized for random read/write operations such as virtual hard disks.
```
