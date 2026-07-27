[AZ104](az104/readme.MD)

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
  - Blob Containers
  - Access Tiers
  - Lifecycle Management
  - Blob Versioning
  - Blob Soft Delete
  - Container Soft Delete
  - Change Feed
  - Snapshots
  - Point-in-Time Restore
  - Immutable Storage
  - Legal Hold
  - Time-based Retention
  - Encryption Scope
- Azure Files *(Parte 3)*
  - SMB y NFS
  - Azure File Shares
  - Azure File Sync
  - Cloud Tiering
  - Snapshots
  - Backup
  - Autenticación
  - RBAC
  - Casos de uso
- Seguridad *(Parte 4)*
  - Autenticación
  - Authorization
  - Access Keys
  - Shared Access Signature (SAS)
  - Stored Access Policy
  - Service SAS
  - Account SAS
  - User Delegation SAS
  - Azure RBAC
  - Storage Data Roles
  - Firewall
  - Service Endpoints
  - Private Endpoints
  - Networking Best Practices
- Protección de datos *(Parte 5)*
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

# 13. Azure Blob Storage

**Azure Blob Storage** es el servicio de almacenamiento de objetos de Azure.

Está diseñado para almacenar grandes cantidades de datos no estructurados, por ejemplo:

- imágenes
- vídeos
- backups
- documentos
- ficheros ISO
- logs
- datos de aplicaciones

Es el servicio más utilizado dentro de Azure Storage.

---

# 14. Blob Containers

Un **Blob Container** es equivalente a una carpeta lógica dentro de un Storage Account.

Características:

- Puede contener millones de blobs.
- No existe límite práctico de capacidad.
- Cada blob pertenece exactamente a un único Container.

Ejemplo:

```
Storage Account
│
├── images
│      logo.png
│      fondo.jpg
│
├── backups
│      vm1.vhd
│      sql.bak
│
└── documents
       contrato.pdf
```

---

# 15. Blob Types

Azure soporta tres tipos de Blob.

## Block Blob

Es el más utilizado.

Pensado para:

- documentos
- imágenes
- backups
- vídeos

Es el tipo que aparece prácticamente siempre en el examen.

---

## Append Blob

Optimizado para añadir información continuamente.

Ejemplos:

- logs
- auditorías
- trazas

Solo permite añadir información al final del archivo.

---

## Page Blob

Optimizado para acceso aleatorio.

Se utiliza principalmente para:

- discos administrados (Managed Disks)
- discos VHD

---

# 16. Access Tiers

Blob Storage soporta distintos niveles de almacenamiento.

---

## Hot Tier

Pensado para datos accedidos frecuentemente.

Características:

- almacenamiento más caro
- acceso muy barato

Ejemplos:

- aplicaciones
- imágenes
- páginas web

---

## Cool Tier

Pensado para datos consultados ocasionalmente.

Características:

- almacenamiento más barato
- acceso ligeramente más caro

Ejemplos:

- copias recientes
- documentos históricos

---

## Archive Tier

Es el nivel más barato.

Características:

- almacenamiento extremadamente económico
- recuperación lenta
- antes de leer un blob es obligatorio **rehidratarlo**

Ideal para:

- backups antiguos
- cumplimiento normativo
- conservación a largo plazo

---

# Resumen

| Tier | Coste almacenamiento | Coste acceso | Uso |
|---------|------------------:|-------------:|-----|
| Hot | Alto | Bajo | Uso frecuente |
| Cool | Medio | Medio | Uso ocasional |
| Archive | Muy bajo | Alto | Archivo |

---

# 17. Lifecycle Management

Una **Lifecycle Management Policy** automatiza el movimiento o eliminación de blobs.

Permite:

- mover blobs a Cool
- mover blobs a Archive
- eliminar blobs antiguos

Todo ello automáticamente.

---

Ejemplo:

Después de:

30 días

↓

Mover a Cool

↓

180 días

↓

Mover a Archive

↓

365 días

↓

Eliminar

---

## Acciones soportadas

Las acciones más utilizadas son:

```
tierToCool
```

```
tierToArchive
```

```
delete
```

---

## prefixMatch

Si una política solo debe afectar a un Container concreto, debe utilizarse:

```
prefixMatch
```

Ejemplo:

```
logs/
```

Solo se aplicará al Container:

```
logs
```

Esta es una pregunta muy repetida en AZ-104.

---

# 18. Blob Versioning

Cuando está habilitado **Blob Versioning**, Azure crea automáticamente una nueva versión cada vez que un blob se modifica.

Permite:

- recuperar versiones antiguas
- restaurar errores
- auditar cambios

No requiere realizar backups.

---

Ejemplo

```
contrato.docx

↓

Modificado

↓

Version 1

↓

Version 2

↓

Version 3
```

Todas permanecen almacenadas.

---

# 19. Blob Soft Delete

Protege frente a eliminaciones accidentales.

Cuando un blob se elimina:

❌ no desaparece inmediatamente

Permanece durante el periodo configurado.

Puede restaurarse desde el Portal.

---

# 20. Container Soft Delete

Hace exactamente lo mismo, pero para Containers completos.

Permite recuperar:

- el Container
- todos sus blobs

Es independiente de Blob Soft Delete.

---

# 21. Change Feed

**Change Feed** registra permanentemente todos los cambios realizados sobre los blobs.

Ejemplos:

- creación
- modificación
- eliminación

Se utiliza para:

- auditoría
- sincronización
- procesamiento de eventos

**No permite restaurar datos.**

Esta es una trampa clásica del examen.

---

# 22. Blob Snapshots

Un **Snapshot** es una copia de solo lectura de un Blob.

Características:

- ocupa únicamente los cambios
- puede utilizarse para restaurar un blob
- permanece asociado al blob original

Muy utilizado para protección frente a errores.

---

# 23. Point-in-Time Restore

Permite restaurar un Blob Container completo a un instante anterior.

Ejemplo:

Política:

```
7 días
```

Podremos restaurar cualquier momento de los últimos:

```
7 días
```

No será posible recuperar datos anteriores.

Pregunta muy frecuente.

---

# 24. Immutable Storage

Permite impedir modificaciones sobre los blobs.

Existen dos mecanismos.

---

## Time-based Retention

Durante el tiempo configurado:

- no puede modificarse
- no puede eliminarse

Ejemplo:

```
7 años
```

Ideal para cumplimiento normativo.

---

## Legal Hold

Bloquea el Blob indefinidamente.

Solo podrá eliminarse retirando manualmente el Legal Hold.

Muy utilizado en:

- auditorías
- procesos judiciales

---

# 25. Encryption Scope

Por defecto todos los blobs utilizan la clave de cifrado del Storage Account.

Si un Container necesita utilizar una clave distinta:

Debe configurarse un:

**Encryption Scope**

Pregunta muy habitual.

---

# 26. Replicación

Todos los blobs utilizan automáticamente la redundancia configurada en el Storage Account.

Ejemplo:

Storage Account:

```
RA-GRS
```

Todos los Containers serán:

```
RA-GRS
```

No puede configurarse redundancia distinta para un Container concreto.

---

# Preguntas trampa del AZ-104

✅ **Archive Tier** requiere **rehidratación** antes de poder leer un blob.

✅ **Blob Versioning** crea automáticamente nuevas versiones al modificar un blob.

✅ **Change Feed** registra eventos, **no restaura datos**.

✅ **Point-in-Time Restore** solo funciona dentro del periodo configurado.

✅ **Lifecycle Management** utiliza acciones como **tierToCool**, **tierToArchive** y **delete**.

✅ **prefixMatch** permite aplicar una política únicamente a determinados Containers.

✅ **Time-based Retention** y **Legal Hold** forman parte de **Immutable Storage**.

✅ Si un Container necesita una clave de cifrado diferente, debe utilizar un **Encryption Scope**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Block Blob | Tipo más utilizado |
| Archive | Requiere rehidratación |
| Versioning | Nueva versión automática |
| Blob Soft Delete | Recupera blobs eliminados |
| Container Soft Delete | Recupera containers completos |
| Change Feed | Auditoría, no restauración |
| Snapshot | Copia de solo lectura |
| Point-in-Time Restore | Restaura hasta el período configurado |
| Lifecycle | Automatiza movimiento/eliminación |
| Encryption Scope | Clave distinta por Container |
| Immutable Storage | Legal Hold + Time-based Retention |

---

# 27. Azure Files

**Azure Files** es un servicio de almacenamiento que proporciona **recursos compartidos de archivos (File Shares)** completamente administrados.

A diferencia de **Blob Storage**, permite montar una unidad de red desde Windows o Linux utilizando protocolos estándar.

Los usos más habituales son:

- Compartir archivos entre servidores.
- Sustituir servidores de ficheros tradicionales.
- Almacenar perfiles de usuario.
- Compartir configuraciones de aplicaciones.

---

# 28. Azure File Share

Un **Azure File Share** es un recurso compartido dentro de un **Storage Account**.

Puede montarse simultáneamente desde varios equipos.

Ejemplo:

```
Storage Account
│
└── File Share
      │
      ├── Documentos
      ├── Usuarios
      ├── Backups
      └── Software
```

---

# 29. Protocolos soportados

Azure Files soporta dos protocolos.

## SMB

(Server Message Block)

Compatible con:

- Windows
- macOS
- Linux

Es el protocolo utilizado en prácticamente todas las preguntas del AZ-104.

Puerto utilizado:

```
445/TCP
```

**Pregunta típica**

> ¿Qué puerto debe permitirse para montar un Azure File Share?

Respuesta:

```
445
```

---

## NFS

(Network File System)

Disponible para determinados tipos de Azure Files.

Principalmente utilizado por servidores Linux.

No requiere SMB.

---

# 30. Azure File Sync

**Azure File Sync** permite sincronizar un **Azure File Share** con uno o varios servidores Windows.

Arquitectura:

```
Azure File Share

        ▲

Azure File Sync

        ▲

Windows Server
```

El servidor mantiene una copia local mientras Azure conserva la copia principal.

---

# 31. Azure File Sync Agent

Para sincronizar un servidor Windows con Azure Files es obligatorio instalar:

**Azure File Sync Agent**

Sin este agente el servidor no puede participar en la sincronización.

---

# 32. Cloud Tiering

**Cloud Tiering** permite ahorrar espacio en el servidor local.

Funcionamiento:

Los archivos utilizados con frecuencia permanecen localmente.

Los menos utilizados se sustituyen por un pequeño marcador.

Cuando el usuario vuelve a abrir un archivo:

↓

Azure lo descarga automáticamente.

---

Ventajas:

- Reduce el almacenamiento local.
- Mantiene visibles todos los archivos.
- Descarga automática bajo demanda.

---

# 33. Azure File Share Snapshots

Los **Snapshots** permiten crear copias de solo lectura de un File Share.

Pueden utilizarse para:

- recuperar archivos
- recuperar carpetas
- restaurar versiones anteriores

No es necesario restaurar todo el recurso compartido.

---

# 34. Backup de Azure Files

Las copias de seguridad de un **Azure File Share** se realizan mediante:

**Recovery Services Vault**

No utilizan:

- Backup Vault
- Blob Backup

Es una de las preguntas más repetidas del examen.

---

# 35. Recovery Services Vault

Para proteger un Azure File Share es necesario:

1.

Crear un:

**Recovery Services Vault**

↓

2.

Crear una:

**Backup Policy**

↓

3.

Habilitar el Backup del File Share.

---

# 36. Restauración

Azure Backup permite restaurar:

- un archivo
- una carpeta
- todo el File Share

No es necesario restaurar toda la copia de seguridad.

---

# 37. Autenticación

Azure Files soporta distintos mecanismos de autenticación.

Los más habituales son:

## Access Keys

Acceso completo.

No recomendado para usuarios.

---

## Shared Access Signature (SAS)

Permite acceso temporal.

Puede limitar:

- permisos
- fecha
- IP
- protocolo

---

## Microsoft Entra ID

Método recomendado.

Permite utilizar:

Azure RBAC

Es la opción que suele aparecer cuando el examen menciona:

**mínimo privilegio**

---

# 38. Azure RBAC para Azure Files

Los permisos sobre un Azure File Share pueden asignarse mediante Azure RBAC.

Roles habituales:

- Storage File Data SMB Share Reader
- Storage File Data SMB Share Contributor
- Storage File Data SMB Share Elevated Contributor

Estos roles se asignan sobre:

**el propio File Share**

No sobre el Storage Account.

Pregunta muy frecuente.

---

# 39. SMB Share Reader

Permite:

- leer archivos
- listar carpetas

No puede:

- modificar
- eliminar
- crear archivos

---

# 40. SMB Share Contributor

Permite:

- leer
- escribir
- crear
- eliminar

No puede modificar permisos NTFS.

Es el rol recomendado para usuarios normales.

---

# 41. SMB Share Elevated Contributor

Incluye todos los permisos del Contributor.

Además permite:

- modificar permisos NTFS
- cambiar ACLs

Pensado para administradores.

---

# 42. Casos de uso

Azure Files está especialmente indicado para:

✅ sustituir servidores de archivos.

✅ compartir información entre máquinas virtuales.

✅ perfiles de usuario.

✅ aplicaciones Legacy.

No está pensado para almacenar grandes cantidades de objetos.

En ese caso debe utilizarse:

**Blob Storage**

---

# Comparativa Blob vs Azure Files

| Blob Storage | Azure Files |
|--------------|-------------|
| Objetos | Archivos |
| HTTP / REST | SMB / NFS |
| Imágenes | Unidades de red |
| Backups | File Server |
| Vídeos | Carpetas compartidas |
| Muy escalable | Compatible con SMB |

---

# Preguntas trampa del AZ-104

✅ Azure Files utiliza principalmente el protocolo **SMB**.

✅ El puerto utilizado por **SMB** es el **445/TCP**.

✅ Para sincronizar un servidor Windows debe instalarse **Azure File Sync Agent**.

✅ **Cloud Tiering** mantiene solo los archivos más utilizados en el servidor local.

✅ Los backups de Azure Files utilizan un **Recovery Services Vault**.

✅ Los permisos RBAC se asignan sobre el **File Share**, no sobre el Storage Account.

✅ **Blob Storage** y **Azure Files** son servicios diferentes; Blob almacena objetos y Azure Files proporciona recursos compartidos mediante SMB/NFS.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Files | Recursos compartidos |
| SMB | Puerto **445** |
| NFS | Soporte Linux |
| Azure File Sync | Sincroniza servidores Windows |
| Azure File Sync Agent | Obligatorio |
| Cloud Tiering | Reduce almacenamiento local |
| Snapshots | Recuperación rápida |
| Backup | Recovery Services Vault |
| SMB Reader | Solo lectura |
| SMB Contributor | Leer y escribir |
| SMB Elevated Contributor | Administra permisos NTFS |
| Blob vs Files | Objetos vs archivos compartidos |



---

# 43. Métodos de autenticación

Azure Storage admite varios métodos para autenticar el acceso.

De **más recomendable** a **menos recomendable**:

1. **Microsoft Entra ID + Azure RBAC**
2. **Shared Access Signature (SAS)**
3. **Access Keys**

Siempre que el examen mencione:

- mínimo privilegio
- acceso temporal
- usuarios

la respuesta casi nunca será **Access Keys**.

---

# 44. Access Keys

Cada **Storage Account** dispone de:

- Key1
- Key2

Las Access Keys proporcionan:

✅ Acceso completo

✅ Todos los servicios

✅ Sin utilizar Microsoft Entra ID

---

### Ventajas

- Muy fáciles de utilizar.

---

### Inconvenientes

- Acceso completo.
- No siguen el principio de mínimo privilegio.
- Difíciles de auditar.
- Si se filtran, cualquier usuario obtiene acceso total.

Por ello Microsoft recomienda utilizar **Microsoft Entra ID** siempre que sea posible.

---

# 45. Shared Access Signature (SAS)

Una **Shared Access Signature (SAS)** concede acceso temporal a un recurso del Storage Account sin compartir las Access Keys.

Puede limitar:

- permisos
- fecha de inicio
- fecha de expiración
- dirección IP
- protocolo HTTPS

Es uno de los temas más preguntados del AZ-104.

---

## Ejemplo

Una SAS puede permitir:

```
Leer

↓

Solo durante 24 horas

↓

Solo desde una IP concreta

↓

Solo mediante HTTPS
```

---

# 46. Tipos de SAS

Azure soporta tres tipos.

---

## Service SAS

Concede acceso únicamente a **un servicio**.

Ejemplos:

- Blob
- Files
- Queue
- Table

No permite acceder al resto de servicios del Storage Account.

Pregunta muy habitual.

---

## Account SAS

Puede conceder acceso simultáneamente a varios servicios.

Ejemplo:

```
Blob

+

Files

+

Queue
```

Utiliza un único token.

---

## User Delegation SAS

Es el método recomendado.

Características:

- utiliza Microsoft Entra ID
- no utiliza Access Keys
- únicamente disponible para Blob Storage

Pregunta muy repetida.

---

# 47. Stored Access Policy

Una **Stored Access Policy** almacena la configuración de una SAS dentro del Storage Account.

Posteriormente varias SAS pueden reutilizar esa política.

Permite modificar:

- fecha
- permisos
- expiración

sin volver a generar todas las SAS.

---

### Dónde puede utilizarse

Solo en:

- Blob Containers
- File Shares
- Queues
- Tables

**No puede asociarse al Storage Account.**

Es una pregunta clásica del examen.

---

### Límite

Cada **Blob Container** admite un máximo de:

**5 Stored Access Policies**

---

# 48. Restricciones de una SAS

Una SAS únicamente concede acceso cuando se cumplen simultáneamente todas las restricciones configuradas.

Ejemplo:

```
IP correcta

+

Fecha válida

+

HTTPS

+

Permiso Read
```

Si cualquiera de ellas falla:

↓

Azure denegará el acceso.

---

# 49. Azure RBAC

Microsoft recomienda utilizar:

**Microsoft Entra ID + Azure RBAC**

en lugar de Access Keys.

Ventajas:

- auditoría
- mínimo privilegio
- revocación inmediata
- integración con Microsoft Entra ID

---

# 50. Storage Blob Data Reader

Permite:

- leer blobs
- descargar blobs

No permite:

- crear
- modificar
- eliminar

---

# 51. Storage Blob Data Contributor

Permite:

- leer
- crear
- modificar
- eliminar blobs

Es el rol más habitual para aplicaciones.

---

# 52. Storage Blob Data Owner

Incluye todos los permisos anteriores.

Además puede administrar:

- permisos
- ACLs

Pensado para administradores.

---

# 53. Reader + Blob Contributor

Para utilizar el **Azure Portal** normalmente se necesita la combinación:

```
Reader

+

Storage Blob Data Contributor
```

¿Por qué?

El rol **Reader** permite visualizar el Storage Account.

El rol **Storage Blob Data Contributor** permite trabajar con los blobs.

Pregunta muy frecuente.

---

# 54. Storage Firewall

El **Storage Account Firewall** restringe quién puede acceder al Storage Account.

Puede permitir:

- direcciones IP
- Virtual Networks
- Private Endpoints
- Trusted Microsoft Services

Todo el resto del tráfico será bloqueado.

---

# 55. Trusted Microsoft Services

La opción:

```
Allow trusted Microsoft services
```

permite que determinados servicios administrados por Microsoft accedan al Storage Account aunque el Firewall esté habilitado.

No concede acceso:

- a usuarios
- a máquinas virtuales
- a Internet

---

# 56. Service Endpoints

Un **Service Endpoint** conecta una **Subnet** con un servicio PaaS utilizando la red troncal de Microsoft.

Importante:

El servicio **sigue teniendo IP pública**.

No crea una Private IP.

Esta es una de las preguntas más repetidas del AZ-104.

---

## Características

- Se configura sobre una **Subnet**.
- No sobre toda la Virtual Network.
- Compatible con Storage, SQL, Key Vault, etc.

---

# 57. Service Endpoint Policy

Una **Service Endpoint Policy** permite restringir qué **Storage Accounts** pueden utilizar un Service Endpoint.

Ejemplo:

```
Subnet

↓

Solo StorageAccount1

↓

Denegar el resto
```

Muy utilizada en preguntas de seguridad.

---

# 58. Private Endpoint

Un **Private Endpoint** asigna una **Private IP** del espacio de direcciones de la Virtual Network a un servicio PaaS.

Desde la perspectiva de la máquina virtual, el servicio parece estar dentro de la propia red.

Es la solución recomendada cuando el tráfico **no debe salir nunca a Internet**.

---

# Comparativa

| Service Endpoint | Private Endpoint |
|------------------|------------------|
| IP pública | Private IP |
| Usa backbone Microsoft | Usa backbone Microsoft |
| Configurado en la Subnet | Recurso independiente |
| No cambia el DNS | Requiere Private DNS |
| Menor aislamiento | Máximo aislamiento |

---

# 59. Private DNS Zone

Cuando se utiliza un **Private Endpoint**, normalmente también se configura una **Private DNS Zone**.

Su función es resolver el nombre del servicio hacia la **Private IP** en lugar de la dirección pública.

Sin una resolución DNS adecuada, el cliente podría seguir intentando acceder al endpoint público.

---

# 60. Buenas prácticas

Microsoft recomienda el siguiente orden de preferencia:

1. **Microsoft Entra ID + RBAC**
2. **User Delegation SAS**
3. **Service SAS**
4. **Account SAS**
5. **Access Keys**

Cuanto más abajo en la lista, mayor riesgo de seguridad.

---

# Preguntas trampa del AZ-104

✅ Una **User Delegation SAS** solo funciona con **Blob Storage**.

✅ Una **Stored Access Policy** **no** puede asociarse al **Storage Account**.

✅ Un **Blob Container** admite un máximo de **5 Stored Access Policies**.

✅ Una **SAS** solo concede acceso si se cumplen **todas** las restricciones configuradas.

✅ Un **Service Endpoint** **no** asigna una **Private IP** al servicio.

✅ Un **Private Endpoint** sí asigna una **Private IP** dentro de la Virtual Network.

✅ El principio de **mínimo privilegio** implica utilizar **Microsoft Entra ID + Azure RBAC**, no **Access Keys**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Access Keys | Acceso completo |
| SAS | Acceso temporal |
| Service SAS | Un solo servicio |
| Account SAS | Varios servicios |
| User Delegation SAS | Blob + Entra ID |
| Stored Access Policy | Máximo 5 por Container |
| Blob Data Reader | Solo lectura |
| Blob Data Contributor | Leer y escribir |
| Blob Data Owner | Administra permisos |
| Storage Firewall | Restringe acceso |
| Service Endpoint | Sigue usando IP pública |
| Private Endpoint | Usa Private IP |
| Private DNS Zone | Resuelve hacia la Private IP |
| Mejor práctica | Entra ID + RBAC |


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
