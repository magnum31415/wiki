[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# 📚 Resumen comparativo de servicios Azure

## 📑 Índice

- [Tabla comparativa rápida](#tabla-comparativa-rápida)
- [AzCopy](#azcopy)
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
- [Azure Function](#azure-function)
- [Azure Import/Export service](#azure-importexport-service)
- [Azure Log Analytics](#azure-log-analytics)
- [Azure Log Analytics Workspace](#azure-log-analytics-workspace)
- [Azure Migrate](#azure-migrate)
- [Azure Monitor](#azure-monitor)
- [Azure Monitor (Activity Log)](#azure-monitor-activity-log)
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
| Azure Analysis Services | Modelos analíticos tabulares | BI empresarial | Analytics |
| Azure Automation | Automatización de tareas | Runbooks | Operaciones |
| Azure Backup | Backup gestionado | Protección de datos | Backup / DR |
| Azure Blueprints | Plantillas de gobernanza | Estándares organizativos | Gobierno |
| Azure Data Factory | ETL/ELT gestionado | Integración de datos | Analytics |
| Azure Event Hubs | Streaming masivo | Ingesta de eventos | Big Data |
| Azure Function | Serverless compute | Código bajo demanda | Desarrollo |
| Azure Log Analytics | Consulta de logs | Análisis de telemetría | Observabilidad |
| Azure Log Analytics Workspace | Contenedor de logs | Almacenamiento y consulta | Monitorización |
| Azure Monitor | Monitorización integral | Métricas y logs | Observabilidad |
| Azure Monitor (Activity Log) | Log de operaciones Azure | Auditoría de cambios | Gobierno |
| RBAC | Control de acceso por roles | Autorización | Seguridad |

---

## Azure Analysis Services 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Servicio PaaS para modelos tabulares de análisis (similar a SQL Server Analysis Services).

### 🎯 Para qué se usa
- Modelos semánticos
- Power BI
- BI empresarial

### 🧠 Idea clave
**Modelo analítico centralizado en la nube.**

---

## Azure Automation 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Automatización basada en runbooks (PowerShell / Python).

### 🎯 Casos de uso
- Apagar VMs
- Parches automáticos
- Tareas programadas

### 🧠 Idea clave
**Automatización operativa en Azure.**

---

## Azure Blueprints 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Servicio para definir y aplicar estándares de gobernanza.

### 🧱 Componentes
- Blueprint Definition
- Blueprint Assignment
- Políticas
- RBAC
- ARM templates

### 🧠 Idea clave
**Gobernanza reusable a nivel de suscripción.**

---

## Azure Function 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Servicio serverless para ejecutar código bajo demanda.

### 🔁 Tipos de hosting

1️⃣ **Consumption Plan**
- Escala automática
- Pago por ejecución

2️⃣ **Premium Plan**
- Sin cold start
- Escalado rápido

3️⃣ **App Service Plan**
- Recursos dedicados

### 🎯 Casos de uso
- Webhooks
- Procesamiento de eventos
- APIs ligeras

### 🧠 Idea clave
**Azure Functions = Compute serverless basado en eventos.**

---

## Azure Log Analytics 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Motor de consulta de logs usando KQL.

### 🎯 Para qué se usa
- Consultas avanzadas
- Alertas
- Monitorización

### 🧠 Idea clave
**Lenguaje KQL para análisis de logs.**

---

## Azure Log Analytics Workspace 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Contenedor lógico donde se almacenan los logs.

### 🎯 Contiene
- Logs de VMs
- Logs de Azure Monitor
- Security logs

### 🧠 Idea clave
**Workspace = Base de datos de logs.**

---

## Azure Monitor 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Servicio central de monitorización en Azure.

### 📊 Incluye
- Métricas
- Logs
- Alertas
- Dashboards

### 🧠 Idea clave
**Monitor = Observabilidad completa.**

---

## Azure Monitor (Activity Log) 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Registro de operaciones administrativas en Azure.

### 🎯 Registra
- Creación/eliminación de recursos
- Cambios RBAC
- Modificaciones de configuración

### 🧠 Idea clave
**Activity Log = Auditoría de acciones en Azure.**

---

## Role-based access control (RBAC) 🔝 [Volver al índice](#-índice)

### 🔎 ¿Qué es?
Sistema de autorización basado en roles.

### 🧠 Fórmula clave
Security Principal + Role + Scope

### 🎯 Alcance
- Management Group
- Subscription
- Resource Group
- Recurso

### 🧠 Idea clave
**RBAC = Control granular de permisos.**
