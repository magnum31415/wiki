[AZ104-INDEX](./readme.md)

# 01 - Azure Storage (AZ-104)

> Resumen ejecutivo con los conceptos de Azure Storage más preguntados en el examen AZ-104.

---

# Índice

- Storage Account
- Tipos de almacenamiento
- Redundancia
- Blob Storage
- Azure Files
- Seguridad
- Networking
- Protección de datos
- Replicación vs Backup
- Preguntas trampa

---

# 1. Azure Storage Account

Una **Storage Account** es el contenedor que proporciona acceso a los servicios de almacenamiento de Azure.

Tipos de datos soportados:

| Servicio | Uso |
|----------|-----|
| Blob | Objetos (imágenes, documentos, backups...) |
| Files | Recursos compartidos SMB/NFS |
| Queue | Mensajes |
| Table | Base de datos NoSQL |

Todos los servicios comparten:

- Seguridad
- Redundancia
- Cifrado
- Networking

---

# 2. Tipos de Storage Account

Los tipos más utilizados son:

| Tipo | Uso |
|------|-----|
| StorageV2 (General Purpose v2) | Recomendado para la mayoría de escenarios |
| Premium BlockBlobStorage | Alto rendimiento para Blob |
| Premium FileStorage | Azure Files Premium |

Para el examen, casi siempre la respuesta correcta será:

**StorageV2**

---

# 3. Performance

Dos niveles:

| Tipo | Tecnología |
|------|------------|
| Standard | HDD |
| Premium | SSD |

Premium se utiliza principalmente para:

- Managed Disks
- Azure Files Premium
- Block Blob Premium

---

# 4. Redundancia

| Tipo | Protección | Lectura secundaria |
|------|------------|-------------------|
| LRS | Un datacenter | ❌ |
| ZRS | Availability Zones | ❌ |
| GRS | Región secundaria | ❌ |
| RA-GRS | Región secundaria | ✅ |
| GZRS | Zonas + Región | ❌ |
| RA-GZRS | Zonas + Región | ✅ |

### Regla para el examen

- LRS → un datacenter.
- ZRS → varias zonas.
- GRS → otra región.
- RA-GRS → otra región + lectura.

---

# 5. Blob Storage

Blob Storage almacena objetos.

Tipos:

| Tipo | Uso |
|------|-----|
| Block Blob | Archivos normales |
| Append Blob | Logs |
| Page Blob | Managed Disks |

**Block Blob** es el más utilizado.

---

# 6. Access Tiers

Permiten optimizar costes.

| Tier | Uso |
|------|-----|
| Hot | Acceso frecuente |
| Cool | Acceso ocasional |
| Archive | Archivo |

### Archive

- Menor coste.
- Mayor coste de recuperación.
- Necesita **rehidratación** antes de poder leerse.

---

# 7. Lifecycle Management

Permite automatizar el ciclo de vida de los blobs.

Ejemplos:

- Hot → Cool.
- Cool → Archive.
- Eliminar blobs antiguos.

Si solo deben afectarse determinados Containers se utiliza:

**prefixMatch**

---

# 8. Protección de Blob Storage

Azure ofrece varias opciones.

## Blob Versioning

Mantiene versiones anteriores cuando un blob se modifica.

---

## Blob Soft Delete

Permite recuperar blobs eliminados.

---

## Container Soft Delete

Permite recuperar Containers eliminados.

---

## Snapshot

Realiza una copia puntual de un Blob.

---

## Point-in-Time Restore

Permite restaurar un Container completo a un instante anterior.

---

## Change Feed

Registra todas las operaciones realizadas sobre los blobs.

Se utiliza para:

- Auditoría.
- Integraciones.
- Seguimiento.

**No permite restaurar datos.**

---

# 9. Immutable Storage

Impide modificar o eliminar datos.

Modos:

- Time-based Retention
- Legal Hold

Muy utilizado para requisitos legales o regulatorios.

---

# 10. Azure Files

Azure Files proporciona recursos compartidos mediante:

- SMB
- NFS

Puerto SMB:

**445**

Es una de las preguntas más repetidas del examen.

---

# 11. Azure File Sync

Sincroniza un Azure File Share con servidores Windows.

Requiere instalar:

**Azure File Sync Agent**

Cloud Tiering mantiene únicamente los archivos más utilizados en el servidor local.

---

# 12. Backup Azure Files

Azure Files utiliza:

**Recovery Services Vault**

Puede restaurarse:

- Un archivo.
- Una carpeta.
- Un File Share completo.

---

# 13. Autenticación

Microsoft recomienda utilizar este orden:

1. Microsoft Entra ID + RBAC
2. User Delegation SAS
3. Service SAS
4. Account SAS
5. Access Keys

---

# 14. SAS

Tipos:

| SAS | Uso |
|-----|-----|
| Service SAS | Un servicio |
| Account SAS | Toda la Storage Account |
| User Delegation SAS | Blob + Entra ID |

### User Delegation SAS

Es la opción recomendada.

No utiliza Access Keys.

---

# 15. Stored Access Policy

Permite administrar varias SAS simultáneamente.

Disponible para:

- Blob Containers
- File Shares
- Queues
- Tables

Máximo:

**5 Policies por Blob Container**

---

# 16. Azure RBAC para Storage

Roles más utilizados:

| Rol | Permisos |
|------|----------|
| Storage Blob Data Reader | Leer |
| Storage Blob Data Contributor | Leer y escribir |
| Storage Blob Data Owner | Administrar datos y permisos |

En Azure Portal suele necesitarse:

**Reader + Storage Blob Data Contributor**

---

# 17. Networking

## Service Endpoint

- Configurado sobre una Subnet.
- Utiliza la red de Microsoft.
- **El Storage mantiene IP pública.**

---

## Private Endpoint

- Asigna una **Private IP**.
- El tráfico permanece dentro de la VNet.
- Normalmente requiere una **Private DNS Zone**.

---

# 18. Encryption

Todos los datos se cifran automáticamente.

Opciones:

- Microsoft Managed Keys (MMK)
- Customer Managed Keys (CMK)

### Infrastructure Encryption

Añade una segunda capa de cifrado.

**Solo puede habilitarse al crear la Storage Account.**

---

# 19. Backup vs Replicación

| Replicación | Backup |
|-------------|---------|
| Alta disponibilidad | Recuperación |
| Fallos hardware | Borrados accidentales |
| Fallos regionales | Ransomware |
| No recupera versiones antiguas | Sí |

No deben confundirse.

---

# 20. Buenas prácticas

Microsoft recomienda:

- Utilizar **StorageV2**.
- Usar **Microsoft Entra ID + RBAC**.
- Utilizar **Private Endpoint** cuando sea posible.
- Activar **Versioning** y **Soft Delete**.
- Utilizar **Lifecycle Management** para reducir costes.
- Utilizar **Recovery Services Vault** para Azure Files.

---

# Preguntas trampa del AZ-104

✅ Blob Storage almacena objetos.

✅ Azure Files utiliza SMB (445).

✅ Archive requiere rehidratación.

✅ RA-GRS y RA-GZRS permiten lectura en la región secundaria.

✅ Change Feed registra cambios; no restaura datos.

✅ Point-in-Time Restore restaura Containers completos.

✅ User Delegation SAS utiliza Microsoft Entra ID.

✅ Infrastructure Encryption solo puede habilitarse al crear la Storage Account.

✅ Service Endpoint mantiene IP pública.

✅ Private Endpoint utiliza Private IP.

✅ Azure Files Backup utiliza Recovery Services Vault.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| StorageV2 | Tipo recomendado |
| Blob / Files / Queue / Table | Tipos de almacenamiento |
| LRS / ZRS / GRS / RA-GRS / GZRS | Redundancia |
| Block / Append / Page Blob | Tipos de Blob |
| Hot / Cool / Archive | Access Tiers |
| Lifecycle Management | Gestión automática |
| Versioning | Versiones |
| Soft Delete | Recuperación |
| Point-in-Time Restore | Restauración |
| Change Feed | Auditoría |
| Azure Files | SMB (445) |
| Azure File Sync | Sincronización |
| User Delegation SAS | SAS recomendada |
| Storage Blob Data Contributor | Rol más habitual |
| Service Endpoint | IP pública |
| Private Endpoint | IP privada |
| Infrastructure Encryption | Solo al crear |
| Recovery Services Vault | Backup Azure Files |
