[AZ104-INDEX](./readme.md)

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

# 62. ¿Cómo funciona Azure Backup?

Azure Backup protege los recursos creando **Recovery Points**.

Un **Recovery Point** es una copia del estado del recurso en un momento determinado.

Todos los Recovery Points se administran desde un:

**Recovery Services Vault**

El funcionamiento habitual es:

```

Recurso

↓

Snapshot (rápido)

↓

Recovery Services Vault

↓

Recovery Points

```

Las restauraciones siempre se realizan desde un **Recovery Point**.

---

# 63. Tipos de Backup

Azure Backup utiliza dos mecanismos.

## Instant Restore (Snapshot)

Azure crea inicialmente un **Snapshot** del disco.

Ventajas:

- restauración muy rápida
- permanece en la misma región

El Snapshot solo se conserva durante unos días (según la configuración).

---

## Vault Backup

Posteriormente Azure copia el Snapshot al:

**Recovery Services Vault**

Ventajas:

- retención a largo plazo
- mayor protección
- recuperación ante pérdida del Snapshot

---

# Flujo completo

```

VM

↓

Snapshot

↓

Recovery Services Vault

↓

Recovery Points

```

---

# 64. Recovery Services Vault

El **Recovery Services Vault** es el componente central de Azure Backup.

Se encarga de:

- almacenar Recovery Points
- gestionar políticas
- programar backups
- realizar restauraciones
- aplicar retención

No almacena únicamente máquinas virtuales.

También protege:

- Azure Files
- SQL Server
- SAP HANA
- Azure VM


| Característica           | Recovery Services Vault | Backup Vault              |
| ------------------------ | ----------------------- | ------------------------- |
| Servicio                 | Clásico                 | Nuevo                     |
| Uso principal            | Azure Backup            | Data Protection Backup    |
| Máquinas Virtuales Azure | ✅ Sí                    | ✅ Sí (algunos escenarios) |
| Azure Files              | ✅ Sí                    | ❌ No                      |
| SQL Server en Azure VM   | ✅ Sí                    | ❌ No                      |
| SAP HANA en Azure VM     | ✅ Sí                    | ❌ No                      |
| Azure Blobs              | ❌ No                    | ✅ Sí (Operational Backup) |
| Azure Disks              | ❌ No                    | ✅ Sí                      |
| Estado en AZ-104         | ⭐ Muy importante        | Poco preguntado           |

---

# 65. Tipos de restauración

Dependiendo del recurso pueden realizarse distintas restauraciones.

## Virtual Machine

- Restaurar la VM completa
- Restaurar discos
- Restaurar archivos

---

## Azure Files

- Archivo individual
- Carpeta
- File Share completo

---

## SQL Server

- Base de datos completa
- Point-in-Time Restore

---

# 66. File Recovery

Una de las preguntas más habituales del AZ-104.

Cuando una VM está protegida mediante Azure Backup es posible restaurar únicamente determinados archivos.

La recuperación puede realizarse desde:

- la VM original
- otra VM
- **cualquier equipo Windows con conexión a Internet**

No es necesario restaurar toda la máquina virtual.

---

# 67. Azure Backup Agent (MARS)

El agente **MARS (Microsoft Azure Recovery Services Agent)** permite realizar copias de seguridad de:

- archivos
- carpetas

No protege la VM completa.

No realiza:

- Bare Metal Recovery
- System State
- Restauración de discos completos

Se utiliza principalmente para servidores físicos o máquinas donde solo interesa proteger determinados archivos.

---

# 68. Azure VM Backup vs MARS Agent

| Azure VM Backup | MARS Agent |
|-----------------|------------|
| Protege la VM completa | Solo archivos y carpetas |
| Snapshot + Vault | Directamente al Vault |
| Permite restaurar discos | No |
| Permite restaurar la VM | No |
| Permite File Recovery | Sí | Sí |

Microsoft recomienda **Azure VM Backup** para máquinas virtuales Azure.

---

# 69. Requisitos de Azure Backup

Para proteger una VM Azure se necesita:

- Recovery Services Vault
- VM soportada
- Managed Disks (recomendado)
- Backup Policy

No es necesario instalar ningún agente para proteger una VM Azure.

---

# 70. Backup Policy

La **Backup Policy** define:

- frecuencia del backup
- hora
- retención diaria
- retención semanal
- retención mensual
- retención anual

Varias VMs pueden compartir la misma política.

---

# 71. Preguntas trampa AZ-104

✅ Azure Backup crea **Recovery Points**.

✅ Primero crea un **Snapshot** y después lo copia al **Recovery Services Vault**.

✅ El **Recovery Services Vault** administra los Recovery Points.

✅ Azure VM Backup **no necesita instalar MARS Agent**.

✅ El **MARS Agent** protege archivos y carpetas, no la VM completa.

✅ **File Recovery** puede realizarse desde cualquier equipo Windows con conexión a Internet.

✅ Azure Files también utiliza **Recovery Services Vault**.

