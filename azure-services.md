[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# 📚 Resumen comparativo de servicios Azure

## 📑 Índice

- [Tabla comparativa rápida](#tabla-comparativa-rápida)
- [AzCopy](#azcopy)
- [Azure Arc](#azure-arc)
- [Azure Automation](#azure-automation)
- [Azure Backup](#azure-backup)
- [Azure Data Box](#azure-data-box)
- [Azure Data Box Edge](#azure-data-box-edge)
- [Azure Data Box Gateway](#azure-data-box-gateway)
- [Azure Data Factory](#azure-data-factory)
- [Azure Data Lake Storage](#azure-data-lake-storage)
- [Azure Database Migration Service](#azure-database-migration-service)
- [Azure DevOps](#azure-devops)
- [Azure Event Hubs](#azure-event-hubs)
- [Azure File Sync](#azure-file-sync)
- [Azure Import/Export service](#azure-importexport-service)
- [Azure Migrate](#azure-migrate)
- [Azure Notification Hubs](#azure-notification-hubs)
- [Azure Resource Manager (ARM)](#azure-resource-manager-arm)
- [Azure Service Bus](#azure-service-bus)
- [Azure Stack Hub](#azure-stack-hub)
- [Azure Storage Explorer](#azure-storage-explorer)
- [Azure Storage Sync](#azure-storage-sync)
- [Role-based access control (RBAC)](#role-based-access-control-rbac)

---

# 📊 Tabla comparativa rápida

| Servicio | Resumen en pocas palabras | Propósito principal | Orientado a |
|-----------|--------------------------|--------------------|-------------|
| AzCopy | CLI para copiar datos a Azure Storage | Transferencia rápida de datos | Migración |
| Azure Arc | Gestión híbrida y multi-cloud | Gobierno centralizado | Infraestructura |
| Azure Automation | Automatización de tareas | Runbooks y procesos automáticos | Operaciones / IT |
| Azure Backup | Backup gestionado en la nube | Protección y recuperación de datos | Backup / DR |
| Azure Data Box | Dispositivo físico de transferencia | Migración masiva offline | Migración |
| Azure Data Box Edge | Dispositivo físico edge con procesamiento | Transferencia + computación en edge | Edge / Híbrido |
| Azure Data Box Gateway | Pasarela virtual con caché local | Transferir datos a Azure Storage | Híbrido / Migración / DR |
| Azure Data Factory | Integración y transformación de datos | ETL/ELT y orquestación | Integración de datos |
| Azure Data Lake Storage | Data lake escalable para analítica | Almacenamiento masivo | Big Data / Analytics |
| Azure Database Migration Service | Migración gestionada de bases de datos | Migración online/offline | Bases de datos |
| Azure DevOps | Plataforma de CI/CD y gestión de proyectos | Desarrollo y despliegue continuo | Dev / DevOps |
| Azure Event Hubs | Ingesta masiva de eventos | Streaming de datos | Big Data / Telemetría |
| Azure File Sync | Sincroniza file servers con Azure | Extender almacenamiento a la nube | Híbrido / Files |
| Azure Import/Export service | Transferencia física con discos | Migraciones offline | Migración masiva |
| Azure Migrate | Plataforma de migración de servidores | Evaluar y migrar workloads | Infraestructura |
| Azure Notification Hubs | Push notifications móviles | Notificaciones masivas | Usuarios finales |
| Azure Resource Manager (ARM) | Motor de despliegue de recursos Azure | Infraestructura como código | Infraestructura |
| Azure Service Bus | Mensajería empresarial | Comunicación entre aplicaciones | Backend |
| Azure Stack Hub | Azure en tu datacenter | Extensión híbrida | Infraestructura |
| Azure Storage Explorer | Cliente gráfico para Storage | Gestión manual de datos | Administración |
| Azure Storage Sync | Sincronización con Azure Files | Extensión de almacenamiento | Híbrido / Files |
| Role-based access control (RBAC) | Control de acceso por roles | Autorización granular | Seguridad |

---

## AzCopy

### 🔎 ¿Qué es?
Herramienta CLI para copiar datos hacia/desde Azure Storage.

### 🧠 Idea clave examen
**AzCopy = Transferencia rápida de datos por red.**

---

## Azure Arc

### 🔎 ¿Qué es?
Gestión centralizada de recursos on-prem, multi-cloud y edge desde Azure.

### 🧠 Idea clave examen
**Azure Arc = Gobierno híbrido/multi-cloud.**

---

## Azure Automation

### 🔎 ¿Qué es?
Servicio para automatizar tareas administrativas en Azure y entornos híbridos.

### 🎯 Para qué se usa
- Runbooks (PowerShell / Python)
- Automatizar parches
- Apagar/encender VMs
- Gestión programada

### 🧠 Idea clave examen
**Azure Automation = Runbooks y automatización operativa.**

---

## Azure Backup

### 🔎 ¿Qué es?
Servicio de backup totalmente gestionado en Azure.

### 🧠 Idea clave examen
**Azure Backup = Protección y recuperación de datos.**

---

## Azure Data Box

### 🔎 ¿Qué es?
Dispositivo físico enviado por Microsoft para transferencias masivas offline.

### 🧠 Idea clave examen
**Data Box = Migración offline a gran escala.**

---

## Azure Data Box Edge

### 🔎 ¿Qué es?
Dispositivo físico con capacidad de procesamiento en edge.

### 🧠 Idea clave examen
**Data Box Edge = Edge computing + envío de datos.**

---

## Azure Data Box Gateway

### 🔎 ¿Qué es?
Dispositivo virtual con caché local para transferir datos a Azure.

### 🧠 Idea clave examen
**Data Box Gateway = Pasarela híbrida virtual.**

---

## Azure Data Factory

### 🔎 ¿Qué es?
Servicio de integración de datos (ETL/ELT).

### 🧠 Idea clave examen
**Data Factory = Orquestación y transformación de datos.**

---

## Azure Data Lake Storage

### 🔎 ¿Qué es?
Almacenamiento optimizado para Big Data y analítica.

### 🧠 Idea clave examen
**Data Lake = Almacenamiento masivo para analítica.**

---

## Azure Database Migration Service

### 🔎 ¿Qué es?
Servicio gestionado para migrar bases de datos con mínimo downtime.

### 🧠 Idea clave examen
**DMS = Migración de bases de datos.**

---

## Azure DevOps

### 🔎 ¿Qué es?
Plataforma para desarrollo y despliegue continuo.

### 🎯 Incluye
- Repos (Git)
- Pipelines (CI/CD)
- Boards
- Artifacts

### 🧠 Idea clave examen
**Azure DevOps = Desarrollo + CI/CD en Azure.**

---

## Azure Event Hubs

### 🔎 ¿Qué es?
Plataforma de ingesta masiva de eventos en tiempo real.

### 🧠 Idea clave examen
**Event Hubs = Streaming masivo de eventos.**

---

## Azure File Sync

### 🔎 ¿Qué es?
Sincroniza file servers locales con Azure Files.

### 🧠 Idea clave examen
**File Sync = Sincronización híbrida de archivos.**

---

## Azure Import/Export service

### 🔎 ¿Qué es?
Permite enviar discos físicos a Azure para migración.

### 🧠 Idea clave examen
**Import/Export = Migración física manual.**

---

## Azure Migrate

### 🔎 ¿Qué es?
Plataforma para evaluar y migrar servidores y workloads a Azure.

### 🧠 Idea clave examen
**Azure Migrate = Evaluación + migración de infraestructura.**

---

## Azure Notification Hubs

### 🔎 ¿Qué es?
Servicio de notificaciones push móviles masivas.

### 🧠 Idea clave examen
**Notification Hubs = Push móvil.**

---

## Azure Resource Manager (ARM)

### 🔎 ¿Qué es?
Motor de despliegue y gestión de recursos en Azure.

### 🎯 Para qué se usa
- Deploy de recursos
- ARM Templates
- Gestión declarativa
- Infraestructura como código

### 🧠 Idea clave examen
**ARM = Motor que crea y gestiona recursos Azure.**

---

## Azure Service Bus

### 🔎 ¿Qué es?
Mensajería empresarial desacoplada.

### 🧠 Idea clave examen
**Service Bus = Colas y publish/subscribe.**

---

## Azure Stack Hub

### 🔎 ¿Qué es?
Azure ejecutándose en tu datacenter.

### 🧠 Idea clave examen
**Stack Hub = Azure on-prem.**

---

## Azure Storage Explorer

### 🔎 ¿Qué es?
Cliente gráfico para gestionar blobs, files y tables.

### 🧠 Idea clave examen
**Storage Explorer = Gestión manual de Azure Storage.**

---

## Azure Storage Sync

### 🔎 ¿Qué es?
Servicio que sincroniza almacenamiento local con Azure Files.

### 🧠 Idea clave examen
**Storage Sync = Extensión híbrida de almacenamiento.**

---

## Role-based access control (RBAC)

### 🔎 ¿Qué es?
Sistema de autorización basado en roles en Azure.

### 🎯 Para qué se usa
- Asignar permisos a usuarios, grupos o aplicaciones
- Definir alcance (scope): Management Group, Subscription, Resource Group o recurso
- Aplicar principio de mínimo privilegio

### 🧠 Idea clave examen
**RBAC = Security Principal + Role + Scope.**
