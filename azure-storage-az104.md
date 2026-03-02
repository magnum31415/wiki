[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 📑 Índice – Azure Storage AZ-104

- [📦 Azure Storage – Resumen ampliado para AZ-104](#-azure-storage--resumen-ampliado-para-az-104)
- [🧱 Servicios principales de Azure Storage](#-servicios-principales-de-azure-storage)
- [2️⃣ Azure Blob Storage](#2️⃣-azure-blob-storage)
- [3️⃣ Azure Files](#3️⃣-azure-files)
- [4️⃣ AzCopy](#4️⃣-azcopy)
- [5️⃣ Azure Queue Storage](#5️⃣-azure-queue-storage)
- [6️⃣ Azure Table Storage](#6️⃣-azure-table-storage)
- [7️⃣ Comparativa rápida](#7️⃣-comparativa-rápida)
- [🎯 Puntos críticos para AZ-104](#-puntos-críticos-para-az-104)
- [🧠 Claves de examen](#-claves-de-examen)
- [🔄 Conversiones permitidas entre tipos de redundancia en Azure Storage](#-conversiones-permitidas-entre-tipos-de-redundancia-en-azure-storage)
- [🗂 Tipos de Storage Account (según la pregunta AZ-104)](#-tipos-de-storage-account-según-la-pregunta-az-104)
- [🎯 ¿Qué cuenta puede convertirse a ZRS mediante Live Migration?](#-qué-cuenta-puede-convertirse-a-zrs-mediante-live-migration)
- [✅ Respuesta correcta](#-respuesta-correcta)

---

# 📦 Azure Storage – Resumen ampliado para AZ-104
[⬆ Volver al índice](#-índice--azure-storage-az-104)

## 1️⃣ ¿Qué es Azure Storage?

Azure Storage es la plataforma de almacenamiento en la nube de Microsoft. Está diseñada para ser:


- Masivamente escalable  
- Altamente disponible  
- Segura  
- Durable  
- Accesible desde cualquier lugar
  
Data in an Azure Storage account is always replicated three times in the primary region.

Permite almacenar distintos tipos de datos según el escenario.

---

# 🧱 Servicios principales de Azure Storage
[⬆ Volver al índice](#-índice--azure-storage-az-104)
| Servicio | Tipo de datos | Caso típico en examen |
|-----------|--------------|-----------------------|
| **Blob Storage** | Objetos (no estructurados) | Imágenes, backups, logs |
| **Azure Files** | File share (SMB) | Migrar file servers |
| **Disk Storage** | Discos para VMs | IaaS |
| **Queue Storage** | Mensajería | Comunicación entre apps |
| **Table Storage** | NoSQL key-value | Datos estructurados simples |

---

# 2️⃣ Azure Blob Storage
[⬆ Volver al índice](#-índice--azure-storage-az-104)




| Categoría | Contenido |
|------------|------------|
| **¿Qué es?** | Servicio de almacenamiento de **objetos** optimizado para grandes volúmenes de datos **no estructurados** (sin esquema definido). |
| **Ejemplos de datos no estructurados** | Texto, archivos binarios, imágenes, vídeos, logs |
| **Casos de uso principales** | Servir imágenes/documentos a navegador, almacenamiento distribuido, streaming vídeo/audio, logs, backup & DR, archivado, análisis de datos |
| **Tipos de Blob** | **Block Blob** → Archivos normales (docs, imágenes, backups)<br>**Append Blob** → Logs<br>**Page Blob** → Discos de VM |

---

# 3️⃣ Azure Files

| Categoría | Contenido |
|------------|------------|
| **¿Qué es?** | Servicio de **file shares en la nube** accesible vía **SMB** y **REST API**. Permite que múltiples VMs compartan archivos con lectura y escritura. |
| **Protocolos soportados** | SMB (Server Message Block) <br> REST API |
| **Diferencia clave vs file server tradicional** | Acceso desde cualquier lugar mediante URL pública del storage + autenticación segura. |
| **Ejemplo de acceso** | `https://storageaccount.file.core.windows.net/share/file` |
| **¿Qué es un SAS Token?** | Mecanismo que permite **acceso temporal** con **permisos limitados** (read/write/delete) a recursos privados. |
| **Casos de uso comunes** | Migración de file servers on-prem, compartir configuración entre VMs, herramientas compartidas de equipo, almacenamiento de logs, métricas y crash dumps |

---

# 4️⃣ AzCopy
[⬆ Volver al índice](#-índice--azure-storage-az-104)

| Categoría | Contenido |
|------------|------------|
| **¿Qué es?** | Herramienta de **línea de comandos** para copiar datos hacia o desde una **Storage Account**. |
| **Servicios soportados** | ✅ Blob <br> ✅ File |
| **No soporta** | ❌ Table <br> ❌ Queue |
| **Pregunta típica de examen** | ¿Qué servicios puede copiar AzCopy? → ✔ Blob y File <br> ✘ Table y Queue |
---

# 5️⃣ Azure Queue Storage
[⬆ Volver al índice](#-índice--azure-storage-az-104)
- Servicio de mensajería
- Permite desacoplar aplicaciones
- Comunicación entre componentes

📌 No compatible con AzCopy.

---

# 6️⃣ Azure Table Storage

- Base de datos NoSQL
- Modelo clave/valor
- Alto rendimiento y bajo coste

📌 No compatible con AzCopy.

---

# 7️⃣ Comparativa rápida
[⬆ Volver al índice](#-índice--azure-storage-az-104)
| Servicio | Tipo | Protocolo | Caso típico |
|-----------|------|------------|--------------|
| Blob | Object storage | REST | Imágenes, backup |
| Files | File share | SMB + REST | Migración file server |
| Table | NoSQL | REST | Datos estructurados simples |
| Queue | Messaging | REST | Comunicación entre apps |

---

# 🎯 Puntos críticos para AZ-104
[⬆ Volver al índice](#-índice--azure-storage-az-104)

| Concepto | Resumen clave |
|-----------|---------------|
| **Blob vs Files** | **Blob** → Almacenamiento de objetos <br> **Files** → File share vía SMB |
| **AzCopy** | Funciona solo con **Blob y File** <br> No funciona con **Table ni Queue** |
| **SAS Token** | Proporciona **acceso temporal** con **permisos limitados**. Muy preguntado en escenarios de seguridad. |

---

### 🔥 Cuándo usar cada servicio

| Categoría | Contenido |
|------------|------------|
| **Cuándo usar cada servicio** | **Backup** → Blob <br> **Logs** → Blob (Append) <br> **Compartir archivos entre VMs** → Files <br> **Comunicación entre servicios** → Queue <br> **Datos estructurados simples** → Table |
| **Claves de examen (palabra clave → servicio)** | “object storage” → Blob <br> “SMB” / “file share” → Azure Files <br> “NoSQL” → Table <br> “messaging” → Queue <br> “command-line copy tool” → AzCopy (solo Blob + File) |

---

# Conversiones permitidas entre tipos de redundancia en Azure Storage
[⬆ Volver al índice](#-índice--azure-storage-az-104)
## 📌 Regla general importante

- ✅ Puedes **aumentar el nivel de resiliencia**
- ⚠️ Algunas conversiones requieren que la **región soporte ZRS / GZRS**
- ❌ No puedes cambiar entre **LRS y ZRS directamente** (requiere migración manual)
- ❌ No puedes convertir de **GRS a ZRS directamente**
- ❌ No puedes convertir de **GZRS a ZRS**
- 🔎 La migración en vivo (Live Migration) a ZRS/GZRS solo está soportada inicialmente para cuentas con **LRS o GRS**

---

## 🔄 Tabla de conversiones entre replicaciones

| Desde ↓ | Puede convertirse a → |
|----------|------------------------|
| **LRS** | GRS, RA-GRS, ZRS*, GZRS*, RA-GZRS* |
| **ZRS** | GZRS, RA-GZRS |
| **GRS** | RA-GRS, GZRS*, RA-GZRS* |
| **RA-GRS** | GRS, GZRS*, RA-GZRS* |
| **GZRS** | RA-GZRS |
| **RA-GZRS** | GZRS |

\* Puede requerir migración manual vía Azure Support y que la región soporte zonas de disponibilidad.

---

# 🗂 Tipos de Storage Account (según la pregunta AZ-104)
[⬆ Volver al índice](#-índice--azure-storage-az-104)

| Nombre        | Kind                  | Performance | Replication | Access Tier |
|--------------|-----------------------|-------------|-------------|------------|
| tdaccount1   | General-purpose v2    | Standard    | LRS         | Cool       |
| tdaccount2   | General-purpose v2    | Premium     | RA-GRS      | Hot        |
| tdaccount3   | General-purpose v1    | Premium     | GRS         | None       |
| tdaccount4   | BlobStorage           | Standard    | LRS         | Hot        |

---

# 🎯 ¿Qué cuenta puede convertirse a ZRS mediante Live Migration?
[⬆ Volver al índice](#-índice--azure-storage-az-104)
## 🔎 Reglas clave para Live Migration a ZRS

- ✔ Solo soportado inicialmente para cuentas con **LRS o GRS**
- ✔ Si es **RA-GRS**, primero debe cambiarse a LRS o GRS
- ✔ Debe ser **Standard**, no Premium
- ✔ Debe estar en región que soporte ZRS
- ✔ Algunas combinaciones de Kind no soportan ZRS

---

## 📊 Análisis cuenta por cuenta

### ❌ tdaccount2
- Premium
- RA-GRS
- Requiere cambio previo y además Premium no soporta este escenario
- **No válida**

### ✅ tdaccount1
- General-purpose v2
- Standard
- LRS
- Cumple requisitos para Live Migration a ZRS
- **Correcta**

### ❌ tdaccount3
- General-purpose v1
- Premium
- GRS
- GPv1 + Premium no soporta ZRS
- **No válida**

### ❌ tdaccount4
- BlobStorage
- LRS
- Este tipo no soporta migración directa a ZRS en este escenario
- **No válida**

---

# ✅ Respuesta correcta
[⬆ Volver al índice](#-índice--azure-storage-az-104)
```text
tdaccount1

