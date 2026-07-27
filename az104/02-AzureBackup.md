# 01 - Azure Storage (AZ-104)

# Parte 5 - Protección de datos, Replicación y Preguntas Trampa

---

# Índice

- Redundancia
- Backup vs Replicación
- Encryption
- Customer Managed Keys
- Infrastructure Encryption
- Storage Encryption
- Versioning
- Soft Delete
- Point-in-Time Restore
- Immutable Storage
- Buenas prácticas
- Trampas del examen
- Resumen ejecutivo

---

# 61. Replicación vs Backup

Es una de las confusiones más frecuentes del AZ-104.

## Replicación

La **replicación** protege frente a fallos de infraestructura.

Ejemplos:

- fallo de disco
- fallo de rack
- pérdida de una Availability Zone
- pérdida de una región (GRS)

La replicación **NO** protege frente a:

- borrado accidental
- ransomware
- modificaciones realizadas por un usuario

---

## Backup

El **backup** protege frente a:

- borrados
- ransomware
- modificaciones
- corrupción de datos

Permite volver a un estado anterior.

---

# Comparativa

| Replicación | Backup |
|-------------|---------|
| Fallos hardware | Borrados |
| Fallos Azure | Ransomware |
| Alta disponibilidad | Recuperación histórica |
| Automática | Recovery Points |

---

# 62. Encryption at Rest

Todos los datos almacenados en Azure Storage están cifrados automáticamente.

No es necesario activar ninguna opción para disponer de cifrado en reposo.

Esta es una pregunta muy habitual del examen.

---

# 63. Microsoft Managed Keys

Por defecto Azure administra automáticamente las claves.

Ventajas:

- sin mantenimiento
- sin rotación manual
- alta disponibilidad

Es la configuración predeterminada.

---

# 64. Customer Managed Keys

Cuando la organización necesita controlar completamente las claves de cifrado debe utilizar:

**Customer Managed Keys (CMK)**

Las claves se almacenan en:

**Azure Key Vault**

Permiten:

- rotación
- revocación
- auditoría

---

# 65. Encryption Scope

Si únicamente un determinado **Blob Container** necesita utilizar otra clave de cifrado distinta del resto del Storage Account:

Debe configurarse un:

**Encryption Scope**

No es necesario crear otro Storage Account.

---

# 66. Infrastructure Encryption

Infrastructure Encryption añade una segunda capa de cifrado.

Características:

- doble cifrado
- transparente para las aplicaciones
- solo puede habilitarse durante la creación del Storage Account

Pregunta muy repetida.

---

# 67. Versioning + Soft Delete

Microsoft recomienda utilizar conjuntamente:

- Blob Versioning
- Blob Soft Delete

¿Por qué?

Versioning protege frente a modificaciones.

Soft Delete protege frente a eliminaciones.

La combinación proporciona una protección mucho mayor.

---

# 68. Point-in-Time Restore

Point-in-Time Restore depende de varias características.

Debe estar habilitado:

- Blob Versioning
- Blob Soft Delete
- Change Feed

Si alguna de ellas falta, Point-in-Time Restore no estará disponible.

Esta relación aparece con frecuencia en preguntas avanzadas.

---

# 69. Immutable Storage

Immutable Storage impide modificar o eliminar los blobs.

Existen dos mecanismos.

## Time-based Retention

Bloquea el blob durante un número determinado de días o años.

Finalizado el período, el blob vuelve a ser modificable.

---

## Legal Hold

Bloquea el blob indefinidamente.

Solo un administrador puede retirar manualmente el bloqueo.

---

# Comparativa

| Time-based | Legal Hold |
|------------|------------|
| Tiempo fijo | Sin fecha fin |
| Cumplimiento normativo | Litigios |
| Se desbloquea automáticamente | Debe retirarse manualmente |

---

# 70. Cambios posibles tras crear un Storage Account

Una pregunta muy habitual consiste en distinguir qué propiedades pueden modificarse después de crear el Storage Account.

## Normalmente sí pueden modificarse

- Access Keys
- Firewall
- RBAC
- Encryption Type (MMK ↔ CMK)
- Lifecycle Policies
- Blob Versioning
- Soft Delete
- Networking

---

## Normalmente NO pueden modificarse

- Infrastructure Encryption
- Algunas características Premium
- Determinadas opciones de rendimiento

Siempre revisa la documentación específica del recurso.

---

# 71. Buenas prácticas de seguridad

Microsoft recomienda:

1. Utilizar **Microsoft Entra ID**.

2. Asignar permisos mediante **Azure RBAC**.

3. Utilizar **User Delegation SAS** cuando sea necesario compartir acceso temporal.

4. Evitar el uso de **Access Keys**.

5. Utilizar **Private Endpoints** para recursos críticos.

6. Habilitar **Versioning** y **Soft Delete**.

7. Proteger datos críticos mediante **Immutable Storage**.

---

# 72. Preguntas trampa frecuentes

## Blob vs Azure Files

Blob:

- objetos

Azure Files:

- archivos compartidos

---

## Backup vs Replicación

Replicación:

No recupera archivos borrados.

Backup:

Sí recupera archivos borrados.

---

## Service Endpoint

Utiliza:

IP pública

No crea una Private IP.

---

## Private Endpoint

Siempre utiliza:

Private IP

---

## SAS

No crea usuarios.

Solo genera un token temporal.

---

## Blob Versioning

No realiza backups.

Simplemente conserva versiones anteriores del mismo blob.

---

## Change Feed

No restaura datos.

Solo registra eventos.

---

## Archive Tier

No puede leerse directamente.

Siempre requiere **rehidratación**.

---

## Azure File Sync

No es un sistema de backup.

Es un sistema de sincronización.

---

## LRS

No protege frente a la pérdida completa de una región.

---

## RA-GRS

Es la única redundancia que permite:

Lectura desde la región secundaria.

---

# Resumen general de Azure Storage

| Servicio | Uso principal |
|-----------|---------------|
| Blob Storage | Objetos |
| Azure Files | Recursos compartidos |
| Queue Storage | Mensajería |
| Table Storage | NoSQL |

---

# Resumen de autenticación

| Método | Recomendado |
|----------|------------|
| Entra ID + RBAC | ⭐⭐⭐⭐⭐ |
| User Delegation SAS | ⭐⭐⭐⭐ |
| Service SAS | ⭐⭐⭐ |
| Account SAS | ⭐⭐ |
| Access Keys | ⭐ |

---

# Resumen de seguridad

| Tecnología | Objetivo |
|------------|----------|
| RBAC | Permisos |
| SAS | Acceso temporal |
| Firewall | Restringir acceso |
| Service Endpoint | Acceso por backbone (IP pública) |
| Private Endpoint | Acceso mediante Private IP |
| Private DNS Zone | Resolver el Private Endpoint |

---

# Resumen de protección

| Tecnología | Protege frente a |
|------------|------------------|
| Versioning | Modificaciones |
| Soft Delete | Eliminaciones |
| Snapshot | Recuperación puntual |
| Point-in-Time Restore | Restauración histórica |
| Immutable Storage | Modificaciones y borrados |
| Backup | Ransomware y recuperación completa |
| Replicación | Fallos de infraestructura |

---

# Lo más preguntado en el AZ-104

⭐ SAS (Service, Account y User Delegation)

⭐ Private Endpoint vs Service Endpoint

⭐ Blob Versioning

⭐ Soft Delete

⭐ Lifecycle Management

⭐ Archive Tier

⭐ Azure Files (SMB 445)

⭐ Azure File Sync

⭐ Recovery Services Vault para Azure Files

⭐ RBAC de Storage

⭐ Encryption Scope

⭐ Infrastructure Encryption

⭐ RA-GRS

⭐ LRS

⭐ Point-in-Time Restore

⭐ Stored Access Policies

⭐ Blob Containers

---

# Fin del documento

Con este documento se cubren prácticamente todos los conceptos de **Azure Storage** que aparecen de forma recurrente en los simulacros del examen **AZ-104**.
