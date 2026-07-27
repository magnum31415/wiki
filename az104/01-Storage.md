# 01 - Azure Storage (AZ-104)

> Este documento recopila toda la teoría relacionada con **Azure Storage** necesaria para el examen **AZ-104**. El contenido está consolidado a partir de múltiples simulacros, eliminando preguntas repetidas y destacando las trampas más frecuentes del examen.

---

# Índice

- Tipos de Storage Account
- Tipos de datos soportados
- Performance Tiers
- Redundancia
- Storage Account
- Access Keys
- Shared Access Signature (SAS)
- Encryption
- Routing Preference
- Storage Explorer
- AzCopy
- Blob Storage *(Parte 2)*
- Azure Files *(Parte 3)*
- Seguridad *(Parte 4)*
- Protección de datos *(Parte 5)*

---

# 1. Azure Storage Account

Una **Storage Account** es el recurso que proporciona acceso a todos los servicios de almacenamiento de Azure.

Una misma Storage Account puede contener varios tipos de datos dependiendo del tipo de cuenta utilizado.

Los cuatro servicios principales son:

| Servicio | Uso |
|-----------|-----|
| **Blob Storage** | Objetos (imágenes, backups, documentos, vídeos...) |
| **Azure Files** | Recursos compartidos SMB/NFS |
| **Queue Storage** | Mensajería |
| **Table Storage** | Base de datos NoSQL |

---

# 2. Tipos de datos soportados

Una Storage Account puede almacenar distintos tipos de datos:

| Servicio | Tipo de datos |
|-----------|---------------|
| Blob | Objetos |
| Files | Compartición de archivos |
| Queue | Mensajes |
| Table | Datos NoSQL |

**Pregunta típica**

> ¿Qué tipos de datos soporta Azure Storage?

Respuesta:

- Blob
- Files
- Queue
- Table

---

# 3. Tipos de Storage Account

En AZ-104 prácticamente siempre aparece:

## Standard Storage Account

Utiliza discos HDD.

Adecuado para la inmensa mayoría de aplicaciones.

Compatible con:

- Blob
- Files
- Queue
- Table

---

## Premium Storage Account

Utiliza discos SSD.

Pensado para:

- Azure Files Premium
- Page Blobs
- Discos administrados

Mayor rendimiento y menor latencia.

---

# 4. Performance Tier

Existen dos niveles:

| Tipo | Uso |
|------|-----|
| Standard | HDD |
| Premium | SSD |

No todos los servicios soportan Premium.

---

# 5. Redundancia

La redundancia protege los datos frente a fallos de hardware o de una región.

## LRS

**Locally Redundant Storage**

Mantiene **3 copias** dentro de un único datacenter de una región.

Protege únicamente frente a fallos locales.

---

## ZRS

**Zone Redundant Storage**

Mantiene tres copias distribuidas entre distintas **Availability Zones**.

Protege frente a la pérdida de una zona completa.

---

## GRS

**Geo Redundant Storage**

Mantiene:

- 3 copias en la región primaria
- 3 copias en la región secundaria

No permite lectura desde la región secundaria.

---

## RA-GRS

**Read Access Geo Redundant Storage**

Igual que GRS, pero además permite acceder en modo lectura a la región secundaria.

Es habitual en preguntas donde se menciona el **Secondary Endpoint**.

---

## GZRS

Combina:

- Zone Redundancy
- Geo Redundancy

Es la opción con mayor disponibilidad.

---

# Resumen

| Redundancia | Copias | Región secundaria | Lectura secundaria |
|--------------|---------|-------------------|--------------------|
| LRS | 3 | ❌ | ❌ |
| ZRS | 3 | ❌ | ❌ |
| GRS | 6 | ✅ | ❌ |
| RA-GRS | 6 | ✅ | ✅ |
| GZRS | 6 | ✅ | ❌ |

---

# 6. Storage Account Keys

Cada Storage Account dispone de **dos Access Keys**.

Características:

- Proporcionan acceso total.
- No utilizan Microsoft Entra ID.
- Deben evitarse cuando sea posible.

**Trampa del examen**

Si la pregunta pide:

> principio de mínimo privilegio

La respuesta **nunca** será **Access Keys**.

---

# 7. Access Control (IAM)

Siempre que un usuario necesite acceder al Storage Account se recomienda utilizar:

**Microsoft Entra ID + Azure RBAC**

Ventajas:

- Mínimo privilegio.
- Auditoría.
- Revocación sencilla.
- Sin compartir claves.

Ejemplo típico:

Para escribir blobs:

**Storage Blob Data Contributor**

---

# 8. Encryption

Todos los Storage Accounts cifran automáticamente los datos en reposo.

Existen dos opciones:

## Microsoft Managed Keys (MMK)

Azure administra automáticamente las claves.

Es la opción predeterminada.

---

## Customer Managed Keys (CMK)

Las claves se almacenan en:

**Azure Key Vault**

El cliente controla:

- Rotación
- Revocación
- Acceso

---

# 9. Infrastructure Encryption

Añade una **segunda capa de cifrado** sobre los datos.

Características importantes:

- Solo puede habilitarse **durante la creación del Storage Account**.
- No puede activarse posteriormente.

Es una pregunta muy frecuente en el AZ-104.

---

# 10. Routing Preference

El tráfico hacia un Storage Account puede utilizar dos rutas distintas.

## Microsoft Network

Utiliza la red troncal global de Microsoft.

Mayor rendimiento.

---

## Internet Routing

Utiliza Internet para parte del recorrido.

Ventaja:

- Puede reducir costes.

No modifica el firewall ni la seguridad del Storage Account.

---

# 11. Azure Storage Explorer

Herramienta gráfica para administrar:

- Blob Containers
- File Shares
- Queues
- Tables

Permite:

- Subir archivos
- Descargar archivos
- Crear contenedores
- Eliminar blobs
- Administrar snapshots

---

# 12. AzCopy

**AzCopy** es la herramienta recomendada para copiar grandes volúmenes de datos.

Ejemplo habitual:

```bash
azcopy copy C:\Datos https://storage.blob.core.windows.net/container --recursive
```

El parámetro:

```text
--recursive
```

permite copiar directorios completos.

Es una pregunta muy repetida en el examen.

---

# Trampas frecuentes del AZ-104

✅ Una **Storage Account** puede contener varios servicios de almacenamiento.

✅ **Access Keys** proporcionan acceso completo y no siguen el principio de mínimo privilegio.

✅ **Microsoft Entra ID + RBAC** es el método recomendado para conceder acceso a usuarios.

✅ **Infrastructure Encryption** solo puede habilitarse durante la creación del Storage Account.

✅ **RA-GRS** es la única redundancia que permite leer desde la región secundaria.

✅ **Internet Routing** puede reducir costes, pero no cambia la seguridad del Storage Account.

---

**Fin de la Parte 1**

La siguiente parte cubrirá exclusivamente **Blob Storage**, incluyendo:

- Blob Containers
- Access Tiers
- Lifecycle Management
- Versioning
- Soft Delete
- Change Feed
- Snapshots
- Encryption Scope
- Point-in-Time Restore
- Immutable Storage
- Legal Hold
- Time-based Retention
