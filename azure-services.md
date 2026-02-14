[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 📚 Resumen comparativo de servicios Azure

## 📑 Índice

- [Tabla comparativa rápida](#tabla-comparativa-rápida)
- [Azure Analysis Services](#azure-analysis-services)
- [Azure Arc](#azure-arc)
- [Azure Automation](#azure-automation)
- [Azure Backup](#azure-backup)
- [Azure Blueprints](#azure-blueprints)
- [Azure Data Box](#azure-data-box)
- [Azure Data Box Edge](#azure-data-box-edge)
- [Azure Data Box Gateway](#azure-data-box-gateway)
- [Azure Data Factory](#azure-data-factory)
- [Azure Data Lake Storage](#azure-data-lake-storage)
- [Azure Database Migration Service](#azure-database-migration-service)
- [Azure DevOps](#azure-devops)
- [Azure Event Hubs](#azure-event-hubs)
- [Azure File Sync](#azure-file-sync)
- [Azure Functions](#azure-functions)
- [Azure Import/Export service](#azure-importexport-service)
- [Azure Logs Analytics](#azure-logs-analytics)
- [Azure Logs Analytics Workspace](#azure-logs-analytics-workspace)
- [Azure Migrate](#azure-migrate)
- [Azure Monitor](#azure-monitor)
- [Azure Monitor Activity Log](#azure-monitor-activity-log)
- [Azure Notification Hubs](#azure-notification-hubs)
- [Azure Resource Manager](#azure-resource-manager)
- [Azure Service Bus](#azure-service-bus)
- [Azure Stack Hub](#azure-stack-hub)
- [Azure Storage Explorer](#azure-storage-explorer)
- [Azure Storage Sync](#azure-storage-sync)
- [AzCopy](#azcopy)
- [Role-based access control (RBAC)](#role-based-access-control-rbac)

---

# 📊 Tabla comparativa rápida

| Servicio | Resumen en pocas palabras | Propósito principal | Orientado a |
|----------|--------------------------|--------------------|-------------|
| Azure Analysis Services | Modelo tabular BI en la nube | Análisis empresarial | BI |
| Azure Arc | Gestión híbrida y multi-cloud | Gobierno centralizado | Infraestructura |
| Azure Automation | Automatización cloud | Runbooks y tareas | DevOps |
| Azure Backup | Backup gestionado | Protección de datos | Backup / DR |
| Azure Blueprints | Plantillas de gobierno | Estándares y compliance | Governance |
| Azure Data Box | Transferencia física masiva | Migración offline | Migración |
| Azure Data Box Edge | Edge computing + transferencia | Procesamiento local | Edge |
| Azure Data Box Gateway | Pasarela híbrida virtual | Transferir datos a Azure | Híbrido |
| Azure Data Factory | ETL/ELT en la nube | Integración de datos | Data |
| Azure Data Lake Storage | Data lake escalable | Almacenamiento Big Data | Analytics |
| Azure Database Migration Service | Migración de bases de datos | Migración DB | Migración |
| Azure DevOps | CI/CD y gestión DevOps | Desarrollo colaborativo | DevOps |
| Azure Event Hubs | Streaming masivo de eventos | Ingesta en tiempo real | Big Data |
| Azure File Sync | Sincroniza file servers | Extensión híbrida | Files |
| Azure Functions | Serverless compute | Código por eventos | Serverless |
| Azure Import/Export service | Discos físicos a Azure | Migración offline | Migración |
| Azure Logs Analytics | Consulta de logs (KQL) | Análisis de logs | Monitorización |
| Azure Logs Analytics Workspace | Contenedor de logs | Centralización logs | Monitorización |
| Azure Migrate | Evaluación y migración | Migración infra | Migración |
| Azure Monitor | Monitorización global | Métricas y alertas | Operaciones |
| Azure Monitor Activity Log | Log de cambios en Azure | Auditoría | Seguridad |
| Azure Notification Hubs | Push móvil masivo | Notificaciones | Usuarios |
| Azure Resource Manager | Motor de despliegue Azure | Infraestructura como código | Infra |
| Azure Service Bus | Mensajería empresarial | Comunicación apps | Backend |
| Azure Stack Hub | Azure on-prem | Extensión híbrida | Infraestructura |
| Azure Storage Explorer | Cliente gráfico almacenamiento | Gestión de blobs/files | Storage |
| Azure Storage Sync | Sincroniza almacenamiento | Extensión Azure Files | Híbrido |
| AzCopy | Copia masiva de datos | Transferencia rápida | Storage |
| Role-based access control (RBAC) | Control de acceso granular | Seguridad | Seguridad |

---

### 🔝 [Volver al índice](#-índice)
## Azure Analysis Services

Servicio PaaS para modelos tabulares de análisis (BI).

---

### 🔝 [Volver al índice](#-índice)
## Azure Arc

Gestión centralizada de recursos on-prem y multi-cloud.

---

### 🔝 [Volver al índice](#-índice)
## Azure Automation

Automatiza tareas mediante runbooks (PowerShell/Python).

---

### 🔝 [Volver al índice](#-índice)
## Azure Backup

Backup gestionado para VMs, SQL, SAP HANA y servidores on-prem.

---

### 🔝 [Volver al índice](#-índice)
## Azure Blueprints

Plantillas que combinan:
- ARM templates  
- Policies  
- RBAC  
- Resource Groups  

---

### 🔝 [Volver al índice](#-índice)
## Azure Data Box

Dispositivo físico para migración masiva offline.

---

### 🔝 [Volver al índice](#-índice)
## Azure Data Box Edge

Dispositivo físico con procesamiento local (GPU opcional).

---

### 🔝 [Volver al índice](#-índice)
## Azure Data Box Gateway

Dispositivo virtual con caché local para enviar datos vía NFS/SMB.

---

### 🔝 [Volver al índice](#-índice)
## Azure Data Factory

Servicio ETL/ELT cloud para mover y transformar datos.

---

### 🔝 [Volver al índice](#-índice)
## Azure Data Lake Storage

Almacenamiento optimizado para analítica masiva.

---

### 🔝 [Volver al índice](#-índice)
## Azure Database Migration Service

Migración online/offline de bases de datos.

---

### 🔝 [Volver al índice](#-índice)
## Azure DevOps

Plataforma CI/CD, repositorios Git, pipelines y boards.

---

### 🔝 [Volver al índice](#-índice)
## Azure Event Hubs

Streaming masivo de eventos en tiempo real.

---

### 🔝 [Volver al índice](#-índice)
## Azure File Sync

Sincroniza servidores locales con Azure Files.

---

### 🔝 [Volver al índice](#-índice)
## Azure Functions

Serverless compute:

- Consumption → Pago por ejecución  
- Premium → Sin cold start  
- Dedicated → En App Service Plan  

---

### 🔝 [Volver al índice](#-índice)
## Azure Import/Export service

Migración mediante envío de discos físicos.

---

### 🔝 [Volver al índice](#-índice)
## Azure Logs Analytics

Consulta logs con KQL.

---

### 🔝 [Volver al índice](#-índice)
## Azure Logs Analytics Workspace

Contenedor central de logs para Azure Monitor.

---

### 🔝 [Volver al índice](#-índice)
## Azure Migrate

Evaluación y migración de VMs, bases de datos y apps.

---

### 🔝 [Volver al índice](#-índice)
## Azure Monitor

Monitorización integral de métricas, logs y alertas.

---

### 🔝 [Volver al índice](#-índice)
## Azure Monitor Activity Log

Registro de cambios en recursos Azure.

---

### 🔝 [Volver al índice](#-índice)
## Azure Notification Hubs

Push notifications móviles masivas.

---

### 🔝 [Volver al índice](#-índice)
## Azure Resource Manager

Motor de despliegue de recursos Azure (ARM templates).

---

### 🔝 [Volver al índice](#-índice)
## Azure Service Bus

Mensajería empresarial (queues, topics).

---

### 🔝 [Volver al índice](#-índice)
## Azure Stack Hub

Azure ejecutándose en tu datacenter.

---

### 🔝 [Volver al índice](#-índice)
## Azure Storage Explorer

Cliente gráfico para gestionar Storage.

---

### 🔝 [Volver al índice](#-índice)
## Azure Storage Sync

Sincroniza almacenamiento local con Azure Files.

---

### 🔝 [Volver al índice](#-índice)
## AzCopy

Herramienta CLI para copiar datos masivamente a Azure Storage.

---

### 🔝 [Volver al índice](#-índice)
## Role-based access control (RBAC)

Control de acceso basado en:

- Security Principal  
- Role  
- Scope  

Permite asignar permisos granulares.
