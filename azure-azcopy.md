[Azure](https://github.com/magnum31415/wiki/blob/main/azure-azcopy.md)
# AzCopy

# 📦 Azure Storage – Resumen ampliado para AZ-104

## 1️⃣ ¿Qué es Azure Storage?

Azure Storage es la plataforma de almacenamiento en la nube de Microsoft. Está diseñada para ser:

- Masivamente escalable  
- Altamente disponible  
- Segura  
- Durable  
- Accesible desde cualquier lugar  

Permite almacenar distintos tipos de datos según el escenario.

---

# 🧱 Servicios principales de Azure Storage

| Servicio | Tipo de datos | Caso típico en examen |
|-----------|--------------|-----------------------|
| **Blob Storage** | Objetos (no estructurados) | Imágenes, backups, logs |
| **Azure Files** | File share (SMB) | Migrar file servers |
| **Disk Storage** | Discos para VMs | IaaS |
| **Queue Storage** | Mensajería | Comunicación entre apps |
| **Table Storage** | NoSQL key-value | Datos estructurados simples |

---

# 2️⃣ Azure Blob Storage

## 🔹 ¿Qué es?

Servicio de almacenamiento de **objetos** optimizado para grandes cantidades de datos no estructurados.

📌 Datos no estructurados = sin esquema definido  
Ejemplos:
- Texto
- Archivos binarios
- Imágenes
- Vídeos
- Logs

---

## 🔹 Casos de uso importantes

- Servir imágenes o documentos a un navegador
- Almacenamiento distribuido de archivos
- Streaming de vídeo/audio
- Guardar logs
- Backup y Disaster Recovery
- Archivado
- Análisis de datos (Azure u on-prem)

---

## 🔹 Tipos de blobs (pregunta típica)

| Tipo | Uso |
|------|------|
| **Block Blob** | Archivos normales (docs, imágenes, backups) |
| **Append Blob** | Logs |
| **Page Blob** | Discos de VM |

---

# 3️⃣ Azure Files

## 🔹 ¿Qué es?

Servicio de **file shares en la nube** accesible mediante:

- SMB (Server Message Block)
- REST API

Permite que múltiples VMs compartan archivos con lectura y escritura.

---

## 🔹 Diferencia clave respecto a un file server tradicional

Se puede acceder desde cualquier lugar usando:
``
https://storageaccount.file.core.windows.net/share/file
``

Con un **SAS Token (Shared Access Signature)**.

---

## 🔹 ¿Qué es un SAS Token?

Permite:
- Acceso temporal
- Permisos limitados (read/write/delete)
- Acceso a recursos privados

📌 Muy importante para el examen.

---

## 🔹 Casos de uso comunes

- Migrar aplicaciones on-prem que usan file shares
- Compartir configuración entre VMs
- Herramientas comunes para equipos
- Guardar logs, métricas, crash dumps

---

# 4️⃣ AzCopy

## 🔹 ¿Qué es?

Herramienta de línea de comandos para copiar datos hacia o desde una Storage Account.

---

## 🔹 Servicios soportados por AzCopy

✅ Blob  
✅ File  

❌ No soporta:
- Table
- Queue

---

## 🔹 Pregunta típica

¿Qué servicios puede copiar AzCopy?

✔ Blob y File  
✘ Table y Queue  

---

# 5️⃣ Azure Queue Storage

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

| Servicio | Tipo | Protocolo | Caso típico |
|-----------|------|------------|--------------|
| Blob | Object storage | REST | Imágenes, backup |
| Files | File share | SMB + REST | Migración file server |
| Table | NoSQL | REST | Datos estructurados simples |
| Queue | Messaging | REST | Comunicación entre apps |

---

# 🎯 Puntos críticos para AZ-104

### 🔥 Blob vs Files
- Blob = almacenamiento de objetos
- Files = file share SMB

---

### 🔥 AzCopy
- Solo funciona con Blob y File
- No funciona con Table ni Queue

---

### 🔥 SAS Token
- Acceso temporal
- Permisos limitados
- Muy preguntado en seguridad

---

### 🔥 Cuándo usar cada servicio

| Necesidad | Servicio |
|------------|-----------|
| Backup | Blob |
| Logs | Blob (Append) |
| Compartir archivos entre VMs | Files |
| Comunicación entre servicios | Queue |
| Datos estructurados simples | Table |

---

# 🧠 Claves de examen

Si la pregunta menciona:

- “object storage” → Blob  
- “SMB” o “file share” → Azure Files  
- “NoSQL” → Table  
- “messaging” → Queue  
- “command-line copy tool” → AzCopy (solo Blob + File)  

---


